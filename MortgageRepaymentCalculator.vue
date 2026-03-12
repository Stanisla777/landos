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
  <!-- НЕТ ДОПОЛНИТЕЛЬНОГО DIV - просто слот -->
  <slot></slot>
</template>

<script>
import IMask from 'imask';
import Cookies from 'js-cookie';

export default {
  name: 'FormRatingWrapper',
  props: {
    instanceId: {
      type: String,
      default: () => Math.random().toString(36).substring(7)
    }
  },
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
    },
    // Поиск элементов внутри компонента
    $formRating() {
      return this.$el.querySelector('.form-rating__wr-grade');
    },
    $textArea() {
      return this.$el.querySelector('textarea');
    },
    $hiddenInput() {
      return this.$el.querySelector('input[type="hidden"][name*="ARTICLE_USE"]');
    },
    $submitButton() {
      return this.$el.querySelector('.form-rating__button');
    },
    $gradeItems() {
      return this.$el.querySelectorAll('.js--grade-item');
    }
  },
  methods: {
    // Обновление звёздочек
    updateStars() {
      const items = this.$gradeItems;
      if (!items.length) return;
      
      for (let i = 0; i < items.length - this.rate; i++) {
        items[i].checked = false;
      }
      for (let i = 0; i < this.rate; i++) {
        items[items.length - 1 - i].checked = true;
      }
    },

    // Установка placeholder'а
    setPlaceholder() {
      const placeholders = [
        this.variant_placeholder[0],
        this.variant_placeholder[1],
        this.variant_placeholder[2],
        this.variant_placeholder[3],
        this.variant_placeholder[4]
      ];
      this.place_holder = placeholders[this.rate - 1] || '';
      
      if (this.$textArea) {
        this.$textArea.placeholder = this.place_holder;
      }
      if (this.$hiddenInput) {
        this.$hiddenInput.value = this.rate;
      }
    },

    // Обработка клика по звезде
    handleGradeClick(event, index) {
      const items = this.$gradeItems;
      this.rate = items.length - index;
      
      this.updateStars();
      this.setPlaceholder();
    },

    // Обработка ввода текста
    handleTextInput(event) {
      const value = event.target.value.trim();
      this.btn_active = value.length > 0;
      
      if (event.target.value.length <= this.max_length) {
        this.length_error = false;
      }
    },

    handleTextKeyDown(event) {
      this.show_error = false;
      if (event.target.value.length === this.max_length) {
        this.length_error = true;
      }
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

    // Загрузка сохранённой оценки из cookie
    loadSavedRating() {
      const saved = Cookies.get(this.cookie_key);
      if (saved && parseInt(saved) !== 0) {
        this.rate = parseInt(saved);
        this.form_show = false;
        this.$nextTick(() => {
          this.updateStars();
        });
      }
    },

    // Сохранение оценки в cookie
    saveRating() {
      Cookies.set(this.cookie_key, this.rate);
    },

    // Отправка формы
    async submitForm(event) {
      event.preventDefault();
      event.stopPropagation();

      const form = event.target.closest('form');
      if (!form) {
        console.error('Форма не найдена');
        return;
      }

      const formData = new URLSearchParams(Array.from(new FormData(form))).toString();
      const _success = form.querySelector('.js--webform-success');

      try {
        // Используем fetch вместо BX.ajax для современного подхода
        const response = await fetch('/local/ajax/form.result.new.php', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded',
          },
          body: formData
        });

        const result = await response.json();

        if (result.result) {
          // Отправляем событие в dmpkitdl если есть
          if (typeof window.dmpkitdl !== "undefined" && window.dmpkitdl !== null) {
            window.dmpkitdl.push({event: 'evaluation-fos-send'});
          }

          this.saveRating();
          this.form_show = false;

          if (_success) {
            _success.classList.add('show');
            document.body.classList.add('modal-opened', 'body-modal', 'body-modal-modals');

            setTimeout(() => {
              _success.classList.remove('show');
              document.body.classList.remove('modal-opened', 'body-modal', 'body-modal-modals');
            }, 10000);

            _success.addEventListener('click', function handler() {
              _success.classList.remove('show');
              document.body.classList.remove('modal-opened', 'body-modal-modals');
              setTimeout(() => {
                document.body.classList.remove('body-modal');
              }, 500);
              _success.removeEventListener('click', handler);
            });
          }
        } else if (result.errors) {
          console.warn(result.errors);
        }
      } catch (error) {
        console.warn('Ошибка отправки формы:', error);
      }
    }
  },
  mounted() {
    // Инициализация IMask для textarea
    if (this.$textArea) {
      IMask(this.$textArea, {
        mask: /\S\s?$/
      });
    }

    // Загрузка сохранённой оценки
    this.loadSavedRating();

    // Навешивание обработчиков на существующие элементы
    if (this.$gradeItems.length) {
      Array.from(this.$gradeItems).forEach((input, index) => {
        input.addEventListener('click', (e) => this.handleGradeClick(e, index));
      });
    }

    if (this.$textArea) {
      this.$textArea.addEventListener('input', this.handleTextInput.bind(this));
      this.$textArea.addEventListener('keydown', this.handleTextKeyDown.bind(this));
      this.$textArea.addEventListener('blur', this.endFocus.bind(this));
      this.$textArea.addEventListener('focus', this.beginFocus.bind(this));
    }

    if (this.$submitButton) {
      this.$submitButton.addEventListener('click', this.submitForm.bind(this));
    }
  },
  beforeUnmount() {
    // Очистка обработчиков
    if (this.$gradeItems.length) {
      Array.from(this.$gradeItems).forEach((input) => {
        input.removeEventListener('click', this.handleGradeClick);
      });
    }
    if (this.$textArea) {
      this.$textArea.removeEventListener('input', this.handleTextInput);
      this.$textArea.removeEventListener('keydown', this.handleTextKeyDown);
      this.$textArea.removeEventListener('blur', this.endFocus);
      this.$textArea.removeEventListener('focus', this.beginFocus);
    }
    if (this.$submitButton) {
      this.$submitButton.removeEventListener('click', this.submitForm);
    }
  }
};
</script>
