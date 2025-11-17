data() {
  return {
    // ... остальные поля ...
    lastSentAnswers: null, // ← добавьте эту строку
  };
},

deepEqual(a, b) {
  if (a === b) return true;

  if (a == null || b == null) return a === b;

  if (typeof a !== 'object' || typeof b !== 'object') return a === b;

  if (Array.isArray(a) !== Array.isArray(b)) return false;

  const keysA = Object.keys(a);
  const keysB = Object.keys(b);
  if (keysA.length !== keysB.length) return false;

  for (let key of keysA) {
    if (!keysB.includes(key)) return false;
    if (!this.deepEqual(a[key], b[key])) return false;
  }

  return true;
},

sendingResultToApi(token) {
  // ← ← ← ФОРМИРУЕМ answersToSand как раньше:
  this.answersToSand = [];
  const obj_information = {};
  const obj_holidays = {};

  obj_information.loanAmount = `${this.loanAmount}`;
  obj_information.dateReceipt = JSON.stringify(this.dateLoanReceipt);
  obj_information.bet = `${this.annualInterestRate}`;
  obj_information.term = `${this.termMonthOrYear}`;
  obj_information.loanTerm = `${this.loanTerm}`;
  obj_information.paymentType = this.state.payment_type;
  this.answersToSand.push(obj_information);

  if (this.holidays_term !== 0) {
    obj_holidays.termHoliday = `${this.holidays_term}`;
  }
  if (this.holidays_date_to_send.length !== 0) {
    obj_holidays.beginningHolidays = JSON.stringify(this.holidays_date_to_send);
  }
  this.answersToSand.push(obj_holidays);

  // ДОСРОЧНЫЕ ПЛАТЕЖИ — как раньше:
  const array2MapEnd = this.array_repayment_end.reduce((map, item) => {
    map[item.id] = item;
    return map;
  }, {});
  this.array_early_repayment_to_send.forEach(item => {
    if (array2MapEnd[item.id]) item.earlyEnd = array2MapEnd[item.id];
  });

  const array2MapMain = this.array_repayment_main.reduce((map, item) => {
    map[item.id] = item;
    return map;
  }, {});
  this.array_early_repayment_to_send.forEach(item => {
    if (array2MapMain[item.id]) item.earlyMain = array2MapMain[item.id];
  });

  const array2MapAllPeriod = this.array_repayment_all_period.reduce((map, item) => {
    map[item.id] = item;
    return map;
  }, {});
  this.array_early_repayment_to_send.forEach(item => {
    if (array2MapAllPeriod[item.id]) item.earlyAllPeriod = array2MapAllPeriod[item.id];
  });

  const array2MapPeriod = this.array_repayment_select_period.reduce((map, item) => {
    map[item.key] = item;
    return map;
  }, {});
  this.array_early_repayment_to_send.forEach(item => {
    if (array2MapPeriod[item.id]) item.earlySelectPeriod = array2MapPeriod[item.id];
  });

  const dateMap = {};
  this.array_repayment_date.forEach(item => {
    const id = item[1];
    dateMap[id] = item;
  });
  this.array_early_repayment_to_send.forEach(obj => {
    const dateItem = dateMap[obj.id];
    if (dateItem) obj.earlyDate = dateItem;
  });

  const array2MapAmount = this.array_repayment_amount.reduce((map, item) => {
    map[item.index] = item;
    return map;
  }, {});
  this.array_early_repayment_to_send.forEach(item => {
    if (array2MapAmount[item.id]) item.earlyAmount = array2MapAmount[item.id];
  });

  const array2MapWhatReduce = this.array_repayment_what_reduce.reduce((map, item) => {
    map[item.index] = item;
    return map;
  }, {});
  this.array_early_repayment_to_send.forEach(item => {
    if (array2MapWhatReduce[item.id]) item.whatReduceArray = array2MapWhatReduce[item.id];
  });

  const array2MapBlockRequared = this.array_block_requared.reduce((map, item) => {
    map[item.id] = item;
    return map;
  }, {});
  this.array_early_repayment_to_send.forEach(item => {
    if (array2MapBlockRequared[item.id]) item.blockRequared = array2MapBlockRequared[item.id];
  });

  if (this.array_early_repayment_to_send.length > 0) {
    this.answersToSand.push(this.array_early_repayment_to_send);
  }

  // ← ← ← СРАВНЕНИЕ: если данные не изменились — НЕ отправляем
  if (this.lastSentAnswers && this.deepEqual(this.answersToSand, this.lastSentAnswers)) {
    console.log('Данные не изменились — запрос не отправляется.');
    return;
  }

  // ← ← ← ОТПРАВКА:
  let data = {
    calculatorId: this.calculatorId,
    answers: this.answersToSand,
    "smart-token": token
  };

  axios({
    method: 'post',
    url: '/api/local/calculator/answers/',
    headers: {
      "Content-type": "application/json; charset=UTF-8",
      'X-Bitrix-Csrf-Token': window.BX.bitrix_sessid(),
    },
    data
  })
    .then((res) => {
      if (res.data.code === 200 && res.data.result) {
        this.answersId = res.data.result.answersId;
        this.answerLink = res.data.result.answerLink;
        this.description_after_sand = null;

        // ← ← ← СОХРАНЯЕМ данные после успешной отправки
        this.lastSentAnswers = JSON.parse(JSON.stringify(this.answersToSand));
      }
      if (res.data.code !== 200) {
        if (res.data.description) this.description_after_sand = res.data.description;
        if (res.data.code) this.answer_code = res.data.code;
      }
      this.answersToSand = [];
    })
    .catch((error) => {
      if (error.response?.data?.description) {
        this.description_after_sand = error.response.data.description;
      }
      console.error('Ошибка запроса:', error);
    });
},
