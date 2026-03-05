extends ../templates/base

block title
    title Инструкция
    meta(name="description", content="Описание страницы")

block content
    .pages-wrap
        .pages-content.js--page-content
            include ../includes/header-personal-account
            +header(0)
            include ../includes/blocks/redesign-main-page/sidebar
            main
                .container
                    .block
                        .page-tags
                            .page-tags__item.page-tags__item_time 25.08.2023
                            a(href="#").page-tags__item #ипотека
                            a(href="#").page-tags__item #covid-19
                            a(href="#").page-tags__item #Оформление собственности
                        .page-title-block
                            h1.page-title-block__headline.page-title-block__headline_little Как сэкономить до 2 млн рублей на ипотеке? Скажите мне
                        .page-desc
                            p Актуальные способы сэкономить на ипотеке до 2 млн рублей в нашей инструкции.
                        include ../mixins/card-instruction
                .white-back
                    .container
                        .block.block_content
                            .block-with-aside
                                .block-with-aside__main
                                    .content-wrapper
                                        .content-block
                                            h2.content-title#title1 Какие меры господдержки позволяют сэкономить до 2 млн рублей на ипотечном кредите?
                                            p В России действует целый ряд программ, позволяющих молодым семьям с детьми значительно сэкономить при покупке жилья в ипотеку. Меры государственной поддержки могут покрыть до 40% расходов по кредиту.
                                        hr
                                        .content-block
                                            h2.content-title#title7 Другие льготные программы
                                            p В России также действуют и другие программы поддержки семей
                                            ul
                                                li
                                                    a(href="#")
                                                        b Транспортная
                                                    |  доступность. Транспортная доступность. Транспортная доступность;
                                                li
                                                    a(href="#") Транспортная
                                                li Общественный 
                                                    b транспорт
                                                li
                                                    p Общественный 
                                                        b транспорт
                                                li
                                                    b Транспорт

                                            p Например, в Московской области действуют две региональные программы, благодаря которым молодые семьи могут оформить ипотеку на льготных условиях.
                                            ol
                                                li
                                                    a(href="#")
                                                        b Транспортная
                                                    |  доступность. Транспортная доступность. Транспортная доступность;
                                                li
                                                    a(href="#") Транспортная
                                                li Общественный 
                                                    b транспорт
                                                li
                                                    p Общественный 
                                                        b транспорт
                                                li
                                                    b Транспорт
                                            p Программа «Семейная ипотека в Московской области» позволяет семьям, в которых с 1 января 2020 года родился ребенок, дополнительно снизить процентную ставку по кредиту на 3%.
                                            p Другая программа «Обеспечение жильем молодых семей» дает возможность оплатить почти 30% стоимости жилья за счет социальной выплаты из регионального бюджета.
                                            p Аналогичные меры поддержки воплощаются региональными властями и в других субъектах Российской Федерации
                                        
                                        .content-block
                                            .block-add-favorites
                                                button(type="button", data-modal="#modal-registration-state-service").js--modal-opener.btn_s.transparent_black_border.medium.btn-icon-heart.personal-account.mobile-100
                                                    span Добавить в избранное
                                                    span Добавлено в избранное
                                                include ../mixins/soc-list
                                                +soc-list('soc-list_big', true)

                                            .block-add-favorites
                                                button(type="button").btn_s.transparent_black_border.medium.btn-icon-heart.personal-account.mobile-100
                                                    span Добавить в избранное
                                                    span Добавлено в избранное
                                                include ../mixins/soc-list
                                                +soc-list('soc-list_big', true)

                                            .block-add-favorites
                                                button(type="button").btn_s.transparent_black_border.medium.btn-icon-heart.personal-account.mobile-100.active
                                                    span Добавить в избранное
                                                    span Добавлено в избранное
                                                include ../mixins/soc-list
                                                +soc-list('soc-list_big', true)

                                .block-with-aside__aside.container-flex
                                    .container-flex__col
                                        .sticky-aside
                                            .instruction-documents.js--instruction-documents
                                                span.instruction-documents__headline Документы по теме
                                                    .instruction-documents__fullopener
                                                        svg(width='20', height='20', viewBox='0 0 20 20', fill='none', xmlns='http://www.w3.org/2000/svg')
                                                            path(d='M2 6L9.29289 13.2929C9.68342 13.6834 10.3166 13.6834 10.7071 13.2929L18 6', stroke='#6B7081', stroke-width='2', stroke-linecap='round', stroke-linejoin='round')
                                                .instruction-documents__wrapper
                                                    .instruction-documents__list-wrap
                                                        .instruction-documents__list
                                                            .instruction-documents__list-item
                                                                img(src="img/icon-download.svg", alt="")
                                                                a(href="", download).instruction-documents__list-text Бланк заявления на получение ипотечного кредита (.doc, 1 mb)
                                                            .instruction-documents__list-item
                                                                img(src="img/icon-download.svg", alt="")
                                                                a(href="", download).instruction-documents__list-text Бланк заявления на получение справки по форме 2-НДФЛ (.doc,750 kb)
                                                            .instruction-documents__list-item
                                                                img(src="img/icon-download.svg", alt="")
                                                                a(href="", download).instruction-documents__list-text Бланк заявления на получение справки по форме 2-НДФЛ (.doc,750 kb)
                                                            .instruction-documents__list-item
                                                                img(src="img/icon-download.svg", alt="")
                                                                a(href="", download).instruction-documents__list-text Бланк заявления на получение ипотечного кредита (.doc, 1 mb)
                                                            .instruction-documents__list-item
                                                                img(src="img/icon-download.svg", alt="")
                                                                a(href="", download).instruction-documents__list-text Бланк заявления на получение справки по форме 2-НДФЛ (.doc,750 kb)
                                                            .instruction-documents__list-item
                                                                img(src="img/icon-download.svg", alt="")
                                                                a(href="", download).instruction-documents__list-text Бланк заявления на получение справки по форме 2-НДФЛ (.doc,750 kb)
                                                    .instruction-documents__opener-wrap
                                                        .instruction-documents__opener
                                                            span Показать все
                                                            span Свернуть
                                                            svg(width='20', height='20', viewBox='0 0 20 20', fill='none', xmlns='http://www.w3.org/2000/svg')
                                                                path(d='M2 6L9.29289 13.2929C9.68342 13.6834 10.3166 13.6834 10.7071 13.2929L18 6', stroke='#1C1B28', stroke-width='2', stroke-linecap='round', stroke-linejoin='round')
                                            
                                            .block-with-aside__hidden-elements
                                                .aside-links
                                                    -
                                                        var anchors = [
                                                            {text: 'Какие меры господдержки позволяют сэкономить до 2 млн рублей на ипотечном кредите?', id: '#title1'},
                                                            {text: 'Государственная программа «Семейная ипотека»', id: '#title2'},
                                                            {text: 'Материнский (семейный) капитал', id: '#title3'},
                                                            {text: 'Выплата на погашение кредита для многодетных семей', id: '#title4'},
                                                            {text: 'Имущественный вычет при покупке жилья в ипотеку', id: '#title5'},
                                                            {text: 'Налоговый вычет с процентов по ипотечному кредиту', id: '#title6'},
                                                            {text: 'Другие льготные программы', id: '#title7'}
                                                        ]
                                                    each anchor in anchors
                                                        a(href=anchor.id).aside-links__item.js--anchor= anchor.text

                                    .container-flex__col
                                        .form-rating
                                            p.form-rating__title Была ли полезна страница?
                                            .form-rating__wr-form.js--form-rating#js--form-rating0(v-cloak)
                                                form(method="", action="")
                                                    .form-rating__wr-grade(ref="formRating")
                                                        each val in [1,2,3,4,5]
                                                            .form-rating__grade-item(:class="!form_show?'unactive':''")
                                                                input.js--grade-item(type="radio", :for-raiting="array_star.length-index", :id="item", @click="changeGrade($event,index)", :disabled="!form_show")
                                                                label(:for="item")
                                                                    svg(width='24', height='24', viewBox='0 0 24 24', fill='none', xmlns='http://www.w3.org/2000/svg')
                                                                        path(fill-rule='evenodd', clip-rule='evenodd', d='M10.1416 2.27888C10.8196 0.573703 13.1803 0.573711 13.8583 2.27888L15.8871 7.38124C15.8912 7.39141 15.8957 7.39712 15.8984 7.39991L21.1563 7.84083C22.9969 7.99518 23.591 10.2867 22.3142 11.4307L18.3021 15.0258C18.299 15.0285 18.2921 15.0363 18.2873 15.0518C18.2825 15.0673 18.2816 15.085 18.2855 15.1023L19.5113 20.4776C19.889 22.1339 18.1199 23.7235 16.4946 22.6851L12 19.8135L7.50536 22.6851C5.88008 23.7235 4.11093 22.1339 4.48864 20.4776L5.71443 15.1023C5.71837 15.085 5.71744 15.0673 5.71262 15.0518C5.70782 15.0363 5.701 15.0285 5.6979 15.0258L1.68571 11.4307C0.408924 10.2867 1.00304 7.99518 2.8437 7.84083L8.10151 7.39991C8.10424 7.39712 8.10876 7.39141 8.1128 7.38124L10.1416 2.27888ZM12 3.01814L9.97127 8.12021C9.69314 8.81969 9.04938 9.32744 8.2769 9.39222L3.01667 9.83334C3.01349 9.83829 3.00925 9.84629 3.00564 9.85793C2.99838 9.88129 2.99917 9.90197 3.00211 9.91405C3.00338 9.91926 3.00497 9.92276 3.00658 9.92548C3.00799 9.92787 3.01151 9.93325 3.02037 9.94119L7.03255 13.5362C7.59759 14.0425 7.83037 14.819 7.66437 15.547L6.43858 20.9222C6.43428 20.9411 6.43428 20.9532 6.43474 20.9597C6.43522 20.9665 6.43652 20.9715 6.43807 20.9756C6.43983 20.9802 6.44241 20.9848 6.44553 20.9889L10.937 18.1192C11.5874 17.7037 12.4126 17.7037 13.0629 18.1192L17.5544 20.9889C17.5575 20.9848 17.5601 20.9802 17.5619 20.9756C17.5634 20.9715 17.5647 20.9665 17.5652 20.9597C17.5657 20.9532 17.5657 20.9411 17.5614 20.9222L16.3356 15.547C16.1696 14.819 16.4024 14.0425 16.9674 13.5362L20.9796 9.94119C20.9884 9.93325 20.992 9.92787 20.9934 9.92548C20.995 9.92276 20.9966 9.91926 20.9978 9.91405C21.0008 9.90197 21.0016 9.88129 20.9943 9.85793C20.9907 9.84629 20.9865 9.8383 20.9833 9.83334L15.7231 9.39222C14.9506 9.32744 14.3068 8.81969 14.0287 8.12021L12 3.01814Z', fill='#6D6D6D')
                                                                    svg(width='24', height='24', viewBox='0 0 24 24', fill='none', xmlns='http://www.w3.org/2000/svg')
                                                                        path(fill-rule='evenodd', clip-rule='evenodd', d='M10.1416 2.27888C10.8196 0.573703 13.1803 0.573711 13.8583 2.27888L15.8871 7.38124C15.8912 7.39141 15.8957 7.39712 15.8984 7.39991L21.1563 7.84083C22.9969 7.99518 23.591 10.2867 22.3142 11.4307L18.3021 15.0258C18.299 15.0285 18.2921 15.0363 18.2873 15.0518C18.2825 15.0673 18.2816 15.085 18.2855 15.1023L19.5113 20.4776C19.889 22.1339 18.1199 23.7235 16.4946 22.6851L12 19.8135L7.50536 22.6851C5.88008 23.7235 4.11093 22.1339 4.48864 20.4776L5.71443 15.1023C5.71837 15.085 5.71744 15.0673 5.71262 15.0518C5.70782 15.0363 5.701 15.0285 5.6979 15.0258L1.68571 11.4307C0.408924 10.2867 1.00304 7.99518 2.8437 7.84083L8.10151 7.39991C8.10424 7.39712 8.10876 7.39141 8.1128 7.38124L10.1416 2.27888Z', fill='#F8C000')
                                                    
                                                    .form-rating__wr-textarea(v-show="rate!==0&&form_show")
                                                        .form-rating__textarea(:class="(show_error)||length_error?'error':''")
                                                            textarea(:placeholder="place_holder", v-model="textValue", @blur="endFocus", @input="textInput", @keydown="textKeyDown", @focus="beginFocus", ref="textArea")
                                                            p.form-rating__textarea-error(v-if="show_error") Введите комментарий
                                                            p.form-rating__textarea-error(v-if="length_error") Максимальное количество символов - {{max_length}}
                                                        input(hidden, type="text", ref="hiddenInput")
                                                        button(type="button", v-if="!btn_active", @click="showError").form-rating__button Отправить
                                                        button(type="submit", v-if="btn_active", @click="localStorageClick").form-rating__button.active Отправить

                        .block-container.block_content.block-authorship
                            h3.authorship__title Материал подготовлен
                            .authorship__container.swiper-wrapper-desctop
                                .authorship__col-authors.general-style-slider
                                    .swiper-container.js--swiper-authors
                                        .swiper-wrapper
                                            - for (var i = 1; i <= 4; i++)
                                                .swiper-slide
                                                    a(href="#").authorship__item-author
                                                        .authorship__author-img
                                                            img(src="img/at-user.jpg")
                                                        .authorship__des
                                                            p.authorship__name Щеголев Станислав #{i}
                                                            p.authorship__post Юрисконсульт 1 категории по вопросам господдержки
                                    .cards-instruction-slider__pagination.js--pag-authorship.js--pagination-slider_s
                                .authorship__col-publication-date
                                    .authorship__wrap-publication-date
                                        p.authorship__date-publication 06.09.2022
                                        .authorship__soc-list
                                            include ../mixins/soc-list
                                            +soc-list()
                        
                        .block
                            .swiper-container.cards-instruction-slider.cards-instruction-slider_no-mt.js--cards-instruction-slider
                                .headline-row
                                    span.headline-row__title Популярные инструкции
                                .swiper-wrapper
                                    - var slides = [1,2,3,4,5,6,7,8]
                                    each slide in slides
                                        .swiper-slide
                                            +card-instruction({
                                                img: 'icon-house.svg',
                                                text: 'Ипотека'
                                            }, 'Программа Дальневосточная ипотека', 'instruction-01.png')
                                .cards-instruction-slider__prev.icon-round.icon-round_gray.swiper-button-prev
                                    svg(width='28', height='28', viewBox='0 0 28 28', fill='none', xmlns='http://www.w3.org/2000/svg')
                                        path(fill-rule='evenodd', clip-rule='evenodd', d='M11.9589 7.9594C12.3494 7.56887 12.9826 7.56887 13.3731 7.9594C13.7636 8.34992 13.7636 8.98309 13.3731 9.37361L9.74689 12.9998H20.666C21.2183 12.9998 21.666 13.4476 21.666 13.9998C21.666 14.5521 21.2183 14.9998 20.666 14.9998H9.7469L13.3731 18.6261C13.7636 19.0166 13.7636 19.6498 13.3731 20.0403C12.9826 20.4308 12.3494 20.4308 11.9589 20.0403L6.62624 14.7076C6.62384 14.7052 6.62145 14.7028 6.61908 14.7004C6.44193 14.52 6.33268 14.2727 6.33268 13.9998C6.33268 13.8642 6.35967 13.735 6.40856 13.6171C6.457 13.5 6.52863 13.3902 6.62346 13.2949M11.9589 7.9594L6.62584 13.2925L11.9589 7.9594Z')
                                .cards-instruction-slider__next.icon-round.icon-round_gray.swiper-button-next
                                    svg(width='28', height='28', viewBox='0 0 28 28', fill='none', xmlns='http://www.w3.org/2000/svg')
                                        path(fill-rule='evenodd', clip-rule='evenodd', d='M16.0411 20.0406C15.6506 20.4311 15.0174 20.4311 14.6269 20.0406C14.2364 19.6501 14.2364 19.0169 14.6269 18.6264L18.2531 15.0002L7.33399 15.0002C6.7817 15.0002 6.33399 14.5524 6.33399 14.0002C6.33399 13.4479 6.7817 13.0002 7.33399 13.0002L18.2531 13.0002L14.6269 9.37394C14.2364 8.98341 14.2364 8.35025 14.6269 7.95972C15.0174 7.5692 15.6506 7.5692 16.0411 7.95972L21.3738 13.2924C21.3762 13.2948 21.3786 13.2972 21.3809 13.2996C21.5581 13.48 21.6673 13.7273 21.6673 14.0002C21.6673 14.1358 21.6403 14.265 21.5914 14.3829C21.543 14.5 21.4714 14.6098 21.3765 14.7051M16.0411 20.0406L21.3742 14.7075L16.0411 20.0406Z')
                                .cards-instruction-slider__pagination.swiper-pagination
                            
                            .headline-row.headline-row_not-first-in-block
                                span.headline-row__title Сервисы
                            
                            include ../mixins/card-service
                            .card-list.card-list_2-col
                                .card-list__col
                                    +card-service({
                                        img: 'icon-house.svg',
                                        text: 'Оценка жилья'
                                    }, 'Узнайте, сколько стоит ваша квартира', 'Оценить', 'service-sofa', 'orange')
                                .card-list__col
                                    +card-service({
                                        img: 'icon-house.svg',
                                        text: 'Ипотека'
                                    }, 'Пошаговый сервис индивидуального подбора ипотечной программы', 'Подобрать программу', 'service-notebook', 'green')
                
                .container
                    .block
                        .headline-row.headline-row_with-to-all
                            span.headline-row__title.headline-row__title_small Ваши вопросы по теме
                            a(href="#").link-to-all.link-to-all_questions Все вопросы
                                svg(viewBox='0 0 70 36')
                                    path(d='M7.1739 30.8153H63.0244C65.5269 24.8152 71.5358 12.31529 37.4998 11.81531C-16.1598 15.2025 0.894099 33.9766 26.9922 34.3153C104.062 35.3153 54.5169 6.31531 23.489 16.31527')
                        
                        include ../mixins/card-question
                        .card-list.card-list_4-col.card-list_only-one-row
                            - for (var i = 0; i < 4; i++)
                                .card-list__col
                                    +card-question(['Ипотека', 'Covid-19'], 'Что делать с ипотекой, если заболел или потерял доход из-за коронавируса?', 'card-question_white')
                        
                        a(href="#").link-to-all.link-to-all_mobile.link-to-all_questions Все вопросы
                            svg(viewBox='0 0 70 36')
                                path(d='M7.1739 30.8153H63.0244C65.5269 24.8152 71.5358 12.31529 37.4998 11.81531C-16.1598 15.2025 0.894099 33.9766 26.9922 34.3153C104.062 35.3153 54.5169 6.31531 23.489 16.31527')
                    
                    form(action="", novalidate).feedback.js--universal-form
                        span.feedback__input-title Выберите категорию
                        .feedback__row
                            .select.js--select
                                .select__background.js--select-background
                                .select__choosen.select__choosen-grey.js--select-place Ипотека
                                input.js--validate-mask.js--validate-mask-select(type="hidden", value="", required, data-required-hint="Надо что-то выбрать")
                                .select__list-wrapper
                                    ul.select__list
                                        - var options = ['Ипотека', 'Ипотека 1', 'Ипотека 2', 'Ипотека 1', 'Ипотека 2', 'Ипотека 1', 'Ипотека 2']
                                        each option in options
                                            li.select__list-item.js--select-item(data-value=option)= option
                        
                        .feedback__row
                            label.input
                                input(type="text", inputmode="email", placeholder="E-mail", data-mask-hint="Не допустимый формат", data-maxlength="20", data-maxlength-hint="Допустимое количество символов", data-required-hint="Поле обязательно для заполнения", required).js--validate-mask.js--validate-mask-email
                        
                        .feedback__row
                            label.input
                                input(type="text", placeholder="Имя", data-mask-hint="Не допустимый формат", data-maxlength="50", data-maxlength-hint="Допустимое количество символов", data-required-hint="Введите полностью фамилию, имя и отчество (при наличии)", required).js--validate-mask.js--validate-mask-name
                        
                        .feedback__row
                            label.input.input_bordered
                                input(type="tel", autocomplete="on", placeholder="Номер телефона", data-mask-hint="Не допустимый формат", data-required-hint="Поле обязательно для заполнения", required).js--validate-mask.js--validate-mask-tel
                        
                        .feedback__row
                            label.textarea
                                textarea(name="", cols="30", rows="10", placeholder="Ваш вопрос", data-maxlength="30000", data-mask-hint="Латиницу вбивать нельзя", data-maxlength-hint="Допустимое количество символов", data-required-hint="Поле обязательно для заполнения", required).js--validate-mask.js--validate-mask-message
                        
                        .feedback__container-bottom.js--feedback-bottom
                            .feedback__bottom
                                .feedback__checkbox_wrapper.js--checkbox_wrapper
                                    .feedback__checkbox
                                        label.checkbox
                                            input(type="checkbox", name="", required, data-required-hint="Согласитесь с условиями").js--feedback-check.js--validate-mask.js--validate-mask--check
                                            div
                                        span.feedback__checkbox-text
                                            | Я даю согласие на обработку
                                            |  
                                            a(href="privacy-policy.html") персональных данных
                                button(type="button").btn.js--btn-universal-form Отправить
    
    include ../includes/redesign-site/footer
    include ../mixins/modal-back-call-new.pug
    +modal-back-call-new('bg-grey','Заказать обратный звонок','Напишите, какого калькулятора вам не хватает','Ваше предложение было успешно отправлено')
    include ../mixins/modal-registration-state-service.pug
    +modal-registration-state-service('bg-grey')
    include ../mixins/modal-registration-state-service-header.pug
    +modal-registration-state-service-header('bg-grey')
    include ../mixins/modal-not-autorization-in-state-service.pug
    +modal-not-autorization-in-state-service('bg-grey')
    include ../mixins/modal-confirm-exit-from-personal-account.pug
    +modal-confirm-exit-from-personal-account('bg-grey')
    include ../includes/modals/modal-success-send
