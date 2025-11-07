createSheduleBlocks() {
      const blocks = [];
      let holidayCounter = 0; // счётчик блоков каникул
      const totalItems = this.shedule.length; // общее количество элементов

      this.shedule.forEach((item, index) => {
        if (item.isHoliday) { //блоки с ипотечными каникулами
          holidayCounter++; //увеличиваю счётчик
          const blockHeight = holidayCounter === 1 ? 59 : 43; //высота блока ипотечные каникулы в графике платежей для первого блока 59, для последующих 42
          const contentWidth = 515 - 16 - 16; // ← вся ширина контента
          const badgeHeight = this.converPx(18); // высота плашки "Ипотечные каникулы"

          // Проверяем, является ли текущий блок последним в серии каникул
          const isLastHolidayBlock =
            (index === totalItems - 1) || // последний элемент в массиве
            (!this.shedule[index + 1].isHoliday); // следующий элемент — не каникулы
          blocks.push({
            stack: [
              // Фон на всю ширину
              {
                canvas: [
                  {
                    type: 'rect',
                    x: 0,
                    y: 0,
                    w: contentWidth, // ← 515pt
                    r: 8,
                    h: blockHeight,
                    color: '#f4f4f4'
                  }
                ],
                margin: [0, 0, 0, -blockHeight]
              },
              // Содержимое с отступами ВНУТРИ
              {
                stack: [
                  //плашка ипотечные каникулы с закруглением
                  ...(holidayCounter === 1 ? [
                    {
                      stack: [
                        {
                          canvas: [
                            {
                              type: 'rect',
                              x: 0,
                              y: 0,
                              w: this.converPx(135), // ширина плашки (как в макете)
                              h: badgeHeight,
                              r: 4, // радиус закругления
                              color: '#252628'
                            }
                          ],
                          margin: [0, 0, 0, -badgeHeight]
                        },
                        {
                          text: 'Ипотечные каникулы',
                          style: 'holidayBadgeText', // ← новый стиль без background
                          margin: [this.converPx(8), this.converPx(2), this.converPx(8), this.converPx(2)] // отступы внутри плашки
                        }
                      ],
                      margin: [this.converPx(16), 0, this.converPx(16), this.converPx(12)] // отступы вокруг плашки
                    },
                  ] : []),

                  {
                    columns: [
                      {
                        width: 120,
                        stack: [
                          {
                            text: `${this.$options.filters.capitalize(item.nameMonth)} ${item.year}`,
                            style: 'sheduleItemValueDate'
                          }
                        ]
                      },
                      {
                        width: contentWidth - 120, // ← 515 - 120 - 16 - 16
                        stack: [{ text: '0', style: 'holidayValue' }]
                      }
                    ],
                    margin: [this.converPx(16), holidayCounter === 1 ? 0 : 4, 16, this.converPx(11)] // вроде как тоже отступы от блока ипотечных каникул
                  }
                ],
                margin: [0, this.converPx(16), 0, this.converPx(16)] //отступы внутри блока ипотечных каникул
              },
              // ===== ЛИНИЯ ПОСЛЕ ПОСЛЕДНЕГО БЛОКА КАНИКУЛ =====
              ...(isLastHolidayBlock ? [
                {
                  canvas: [
                    {
                      type: 'line',
                      x1: this.converPx(16),
                      y1: 0,
                      x2: 515 - 16 - 16,
                      y2: 0,
                      lineWidth: 1,
                      lineColor: '#f4f4f4'
                    }
                  ],
                  margin: [0, 0, 0, this.converPx(12)] //отступы от линии
                }
              ] : [])
            ],
            unbreakable: true,
            margin: [0, 0, 0, 0] // ← НЕТ левого отступа!
          });
        } else {
          // Стандартный платёж
          blocks.push(this.createStandardPaymentBlock(item));

          // ДОСРОЧНЫЕ ПОГАШЕНИЯ
          if (item.prepayments && item.prepayments.length > 0) {
            item.prepayments.forEach((product, prepaymentIndex) => {
              // Пропускаем платежи с экспоненциальной записью
              if (String(product.amount).toLowerCase().includes('e')) {
                return;
              }

              blocks.push(this.createEarlyRepaymentBlock(product));
            });

            // ===== ПРОВЕРКА: есть ли каникулы ПОСЛЕ этого элемента? =====
            let hasNextItem = index < totalItems - 1;
            let nextItemIsHoliday = hasNextItem && this.shedule[index + 1].isHoliday;
           
            // Добавляем линию, если следующий элемент — НЕ досрочное погашение
            blocks.push({
              canvas: [
                {
                  type: 'line',
                  x1: this.converPx(16),
                  y1: 0,
                  x2: 515 - 16 - 16,
                  y2: 0,
                  lineWidth: 1,
                  lineColor: nextItemIsHoliday ? '#ffffff' : '#f4f4f4' // ← невидимая или видимая
                }
              ],
              //margin: [0, this.converPx(12), 0, this.converPx(12)]
              margin: [0, this.converPx(12), 0, nextItemIsHoliday ? 0 : this.converPx(12)]
            });
          }
        }
      });

      return {
        stack: blocks
      };
    },
