import FormRatingWrapper from './modules/FormRatingWrapper.vue';

const vueComponents = [
  {
    selector: '.form-rating',
    component: FormRatingWrapper,
    getProps: (el) => ({
      instanceId: el.id || `form-rating-${Date.now()}`
    })
  }
];

-------------------------------------
FormRatingWrapper.vue
<template>
  <slot></slot>
</template>

<script>
import IMask from 'imask';
import Cookies from 'js-cookie';

export default {
  name: 'FormRatingWrapper',
  data() {
    return {
      variant_placeholder: [
        'Что разочаровало?',
        'Какие изменения вы бы предложили?',
        'Чем стоит дополнить?',
        'Что показалось сложным?',
        'Поделитесь своими эмоциями!'
      ],
      place_holder: '',
      array_star: [],
      rate: 0,
      textValue: '',
      show_error: false,
      length_error: false,
      max_length: 300,
      btn_active: false,
      form_show: true
    };
  },
  computed: {
    cookie_key() {
      return `was_useful_rating_${window.location.pathname}`;
    }
  },
  methods: {
    // Инициализация массива звёзд
    initArrayStar(instanceId) {
      this.array_star = [
        `form-rating-input-${instanceId}-1`,
        `form-rating-input-${instanceId}-2`,
        `form-rating-input-${instanceId}-3`,
        `form-rating-input-${instanceId}-4`,
        `form-rating-input-${instanceId}-5`
      ];
    },

    changeGrade(el, index) {
      const element = el.currentTarget;
      const parent = this.$refs.formRating || this.$el.querySelector('.form-rating__wr-grade');
      this.rate = this.array_star.length - index;
      
      const array_star = parent.querySelectorAll('.js--grade-item');
      for (let i = 0; i < array_star.length - this.rate; i++) {
        array_star[i].checked = false;
      }
      for (let i = 0; i < this.rate; i++) {
        array_star[(array_star.length - 1) - i].checked = true;
      }
      
      // формирование плэйсхолдер
      1 === this.rate ? this.place_holder = this.variant_placeholder[0] : 
      2 === this.rate ? this.place_holder = this.variant_placeholder[1] : 
      3 === this.rate ? this.place_holder = this.variant_placeholder[2] : 
      4 === this.rate ? this.place_holder = this.variant_placeholder[3] : 
      5 === this.rate && (this.place_holder = this.variant_placeholder[4]);
      
      const hiddenInput = this.$el.querySelector('input[type="hidden"][name*="ARTICLE_USE"]');
      if (hiddenInput) {
        hiddenInput.value = this.rate;
      }
    },

    clearField(el) {
      this.textValue = '';
      this.show_error = false;
    },

    showError() {
      this.show_error = true;
    },

    endFocus() {
      this.show_error = false;
      this.length_error = false;
    },

    beginFocus() {
      this.show_error = false;
      this.length_error = false;
    },

    textKeyDown(el) {
      this.show_error = false;
      const element = el.currentTarget;
      if (element.value.length === this.max_length) {
        this.length_error = true;
      }
    },

    inputIMask() {
      const textArea = this.$el.querySelector('textarea');
      if (!textArea) return;
      
      const maskOptions = {
        mask: /\S\s?$/
      };
      IMask(textArea, maskOptions);
    },

    textInput(el) {
      const element = el.currentTarget;
      if (element.value.trim().length > 0) {
        this.btn_active = true;
      } else {
        this.btn_active = false;
      }
      if (element.value.length <= this.max_length) {
        this.length_error = false;
      }
    },

    localStorageLoading() {
      let local_stage = Cookies.get(this.cookie_key);
      if ((local_stage !== undefined) && (parseInt(local_stage) !== 0)) {
        const parent = this.$el.querySelector('.form-rating__wr-grade');
        const array_star = parent.querySelectorAll('.js--grade-item');
        for (let i = 0; i < parseInt(local_stage); i++) {
          array_star[(array_star.length - 1) - i].checked = true;
        }
        this.form_show = false;
      }
    },

    localStorageClick() {
      Cookies.set(this.cookie_key, this.rate);
    },

    submitForm(el) {
      el.preventDefault();
      el.stopPropagation();
      
      const form = el.currentTarget.closest('form');
      if (!form) {
        console.error('Форма не найдена');
        return;
      }
      
      let formData = new URLSearchParams(Array.from(new FormData(form))).toString();
      const _success = form.querySelector('.js--webform-success');
      
      // Оригинальный BX.ajax (доступен глобально)
      BX.ajax({
        url: '/local/ajax/form.result.new.php',
        data: formData,
        method: 'POST',
        dataType: 'json',
        timeout: 30,
        async: true,
        processData: true,
        scriptsRunFirst: true,
        emulateOnload: true,
        start: true,
        cache: false,
        onsuccess: (result) => {
          if (!!result) {
            if (result['debug'])
              console.log(result);
            if (result['result']) {
              if (typeof window.dmpkitdl !== "undefined" && window.dmpkitdl !== null) {
                window.dmpkitdl.push({event: 'evaluation-fos-send'});
              }
              if (_success) {
                this.localStorageClick();
                this.form_show = false;
                _success.classList.add('show');
                document.body.classList.add('modal-opened');
                document.body.classList.add('body-modal');
                document.body.classList.add('body-modal-modals');
                
                setTimeout(() => {
                  _success.classList.remove('show');
                  document.body.classList.remove('modal-opened');
                  document.body.classList.remove('body-modal');
                  document.body.classList.remove('body-modal-modals');
                }, 10000);
                
                _success.addEventListener('click', function (e) {
                  _success.classList.remove('show');
                  document.body.classList.remove('modal-opened');
                  document.body.classList.remove('body-modal-modals');
                  setTimeout(() => {
                    document.body.classList.remove('body-modal');
                  }, 500);
                });
              }
            } else {
              if (result['errors']) {
                console.warn(result['errors']);
                return;
              }
            }
          }
        },
        onfailure: function(error) {
          console.warn(error);
        }
      });
    }
  },
  mounted() {
    const instanceId = this.$el.id || 
                      this.$el.closest('[id]')?.id || 
                      Math.random().toString(36).substring(7);
    this.initArrayStar(instanceId);
    
    this.inputIMask();
    this.localStorageLoading();

    const gradeItems = this.$el.querySelectorAll('.js--grade-item');
    gradeItems.forEach((item, index) => {
      item.addEventListener('click', (e) => this.changeGrade(e, index));
    });

    const textArea = this.$el.querySelector('textarea');
    if (textArea) {
      textArea.addEventListener('input', this.textInput.bind(this));
      textArea.addEventListener('keydown', this.textKeyDown.bind(this));
      textArea.addEventListener('blur', this.endFocus.bind(this));
      textArea.addEventListener('focus', this.beginFocus.bind(this));
    }

    const submitBtn = this.$el.querySelector('.form-rating__button');
    if (submitBtn) {
      submitBtn.addEventListener('click', this.submitForm.bind(this));
    }
  },
  beforeUnmount() {
    const gradeItems = this.$el.querySelectorAll('.js--grade-item');
    gradeItems.forEach((item, index) => {
      item.removeEventListener('click', this.changeGrade);
    });

    const textArea = this.$el.querySelector('textarea');
    if (textArea) {
      textArea.removeEventListener('input', this.textInput);
      textArea.removeEventListener('keydown', this.textKeyDown);
      textArea.removeEventListener('blur', this.endFocus);
      textArea.removeEventListener('focus', this.beginFocus);
    }

    const submitBtn = this.$el.querySelector('.form-rating__button');
    if (submitBtn) {
      submitBtn.removeEventListener('click', this.submitForm);
    }
  }
};
</script>
