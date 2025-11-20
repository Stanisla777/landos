npm install --save-dev sass-loader@8.0.2
npm install --save-dev style-loader@1.3.0 --legacy-peer-deps
npm install --save-dev postcss@7 --legacy-peer-deps
npm install --save-dev css-loader@4.3.0 --legacy-peer-deps
npm install --save-dev cross-env
npm install concurrently@8.2.2 --save-dev



"scripts": {
    "start": "webpack-dev-server --mode development --hot --open",
    "dev": "webpack --mode development && prettier --print-width=120 --parser html --write dist/*.html",
    "build": "webpack --mode production",
    "lint": "eslint --ext .js, --ignore-path .gitignore ."
  },

import '../scss/style.scss';

document.addEventListener('DOMContentLoaded', () => {
    window.phoneMask = phoneMask;
    window.closeModal = closeModal;
    window.selectsInit = selectsInit;
    window.questionsAndAnswerSliders = questionsAndAnswerSliders;
    window.fetchLikes = fetchLikes;
    window.checkStatusTimer = checkStatusTimer;
    window.modals = modals;
    window.housingCooperativeMapList = housingCooperativeMapList;
    window.housingCooperativeMapDetail = housingCooperativeMapDetail;
    window.popupGallery = popupGallery;
    window.universalForm = universalForm;
    window.cardSliderfoufcolumn = cardSliderfoufcolumn;
    mainBannerSlider();
    mainBannerParallax();
    mainSlider();
    selectsInit();
    menu();
    svgHandWriting();
    cardsInstructionSlider();
    cardsNewsSlider();
    subcategoryAccordion();
    documentAccordion();
    coursesAccordion();
    catalogAsideAccordion();
    contentSlider();
    aboutSlider();
    questionsAndAnswerSliders();
    instructionDocuments();
    wasUseful();
    anchors();
    newMarathonScheduleSlider();
    contentNote();
    tabInstructions();
    tabDocuments();
    tabNews();
    tabFAQ();
    tabCatalog();
    toUp();
    headerConnection();
    modals();
    catalogModal();
    phoneMask();
    feedbackValidation();
    coursesFeedbackValidation();
    subscriptionValidation();
    chatValidation();
    callValidation();
    headerShowButtons();
//  geolocation();
    ringNav();
    numberFormatting();
    searchGlobal();
    setImageAttr();
    cookies();
    subscriptionSuccess();
    rangeSliders();
    catalogSort();
    fetchLikes();
    setVH();
    checkStatusPinInput();
    checkStatusValidation();
    checkStatusTimer();
    testSlider();
    testNumber();
    checkId();
    playVideo();
    videoWithPreview();
    mediaSlider();
    tabElement();
    activeSlideTest();
    testBuild();
    testPopUp();
    eventPageSlider();
    loaderTest();
    leftSideBar();
    modalCapture();
    taxToggle();
    inputFieldStatus();
    tabChange();
    SelectListChoice();
    educationalSlider();
    educationalSliderReviews();
    educationalSection();
    personalitiesSlider();
    collectionSmallFunctions();
    TabsCallFromColumn();
    slider4Col();
    birthdayReviewsSwiper();
    pageProgressBar();
    itMortgageJs();
    initializationPolls();
    breedingCalculator();
    expandListsItem();
    openUlList();
    openListInfo();
    beesenderAddMessage();
    beesenderNewWindow();
    housingCooperativeFilter();
    popupGallery();
    housingCooperativeListSlider();
    housingCooperativeDetailSlider();
    cardHousingCooperativeSlider();
//  housingCooperativeRequestValidation();
    housingCooperativeChoosenAccordion();
    housingCooperativeMapList();
    housingCooperativeMapDetail();
    housingCooperativeButtons();
    status450InformMail();
    marathonDetail();
    marathonNewDetail();
    initializationMarathonPolls();
    Tags();
    dropdownTopic();
    breadCrumbs();
    progressRs();
    progressSlider();
    progressSlider2();
    progressSlider4column();
    progressSliderPartners();
    atAdvantageSwiper();
    atBecomeSwiper();
    atSliderPartners();
    anotherThing();
    callGameTest();
    substituteStepChange();
    countdownPage();
    progressBarNew();
    jsCallModal();
    atBeesender();
    SliderCourse();
    testPollsSlider();
    izhsMaterialSlider();
    izhsPage();
    izhsBunnersSlider();
    formRating();
    authorsSlider();
    authorsDetailedSliderDes();
    authorsDetailedSliderMob();
    universalForm();
    snowFall();
    additionDropDown();
    housingCooperativeDropdown();
    yandexSearch();
    catalogNew();
    windowResizeDropDown();
    headerMenu();
    footerDropDown();
    pageLoader();
    chatBot();
    instruction();
    cardSlider();
    cardSliderfoufcolumn();
    commentsTexarea();
//    slideConstructor();
    marathon2024();
    hoverCheckboxLink();
    atGameConsists();
    formRatingUniversal();
    formUseFulnessSurvey();
    scrollToUp();
    anotherThingPrizes();
    accordeonHeight();
    scrollMenu();
    mobilePrize();
    counterThing();
    parallaxThing();
    parallaxSeveral();
    animationShow();
    swiperReviews();
    slideConstructor();
    formReportRadioOther();
    tooltipMobile();

// scrollToAddedItems();
// callModalWindow();
//  window.addEventListener('resize', throttle(menu, 1000));

    /**
     * запускает функцию, когда есть необходимый id на странице
     * @param {Object} obj - Информация по функции и ее id
     * @param {String} obj.id - идентификатор для запуска функции
     * @param {Function} obj.fn - функция, которую необходимо запустить
     */
    // eslint-disable-next-line no-unused-vars
    function initVueApp(obj) {
        const app = document.getElementById(obj.id);
        if (app) {
            obj.fn();
        }
    }

    // Устаревшее приложение для
    // блока ключевых показателей с данными из БД ЕИСЖС
    // function initKeyIndicator() {
    //     // eslint-disable-next-line no-new
    //     new Vue({
    //         el: '#appKi',
    //         components: {
    //             'key-indicators': KeyIndicators
    //         }
    //     });
    // }

    function initMrotgageCalc() {
        // eslint-disable-next-line no-new
        new Vue({
            el: '#m-calc',
            components: {
                'mortgage-calc': MortgageCalc
            }
        });
    }

  // eslint-disable-next-line no-unused-vars
    function test() {
      // eslint-disable-next-line no-new
      new Vue({
        el: '#test',
        components: {
          test: Test
        }
      });
    }

    // eslint-disable-next-line no-unused-vars
    function initMortgageRates() {
          // eslint-disable-next-line no-new
          new Vue({
            el: '#v-mortgage-rates',
            components: {
              'mortgage-rates': MortgageRates
            }
          });
        }
    // eslint-disable-next-line no-unused-vars
    function initTelegramBlock() {
        // eslint-disable-next-line no-new
        new Vue({
            el: '#telegram-block',
            components: {
                'telegram-block': TelegramBlock
            }
        });
    }
    function initStatusByKadastr() {
        // eslint-disable-next-line no-new
        new Vue({
            el: '#status-by-kadastr',
            components: {
                'status-by-kadastr': StatusByKadastr
            }
        });
    }

    // eslint-disable-next-line no-unused-vars
        function initTaxDeductionCalculator() {
              // eslint-disable-next-line no-new
              new Vue({
                el: '#v-tax-calculator',
                components: {
                  'tax-calculator': TaxDeductionCalculator
                }
              });
            }

    // eslint-disable-next-line no-unused-vars
        function initCreditHolidaysCalculator() {
              // eslint-disable-next-line no-new
              new Vue({
                el: '#v-credit-holidays-calculator',
                components: {
                  'credit-holidays-calculator': CreditHolidaysCalculator
                }
              });
            }

    // eslint-disable-next-line no-unused-vars
        function initITMortgageSurveys() {
              // eslint-disable-next-line no-new
              new Vue({
                el: '#v-mortgage-surveys',
                components: {
                  'it-mortgage-surveys': ItMortgageSurveys
                }
              });
            }
    // eslint-disable-next-line no-unused-vars
        function initMortgageCalculator() {
              // eslint-disable-next-line no-new
              new Vue({
                el: '#v-mortgage-calculator',
                components: {
                  'mortgage-calculator': MortgageCalculator
                }
              });
            }

    // eslint-disable-next-line no-unused-vars
      // function initPolls() {
            // eslint-disable-next-line no-new
            // new Vue({
              // el: '#v-polls',
              // components: {
                // 'v-polls': VPolls
              // }
            // });
        // }

    // eslint-disable-next-line no-unused-vars
          function initStories() {
                // eslint-disable-next-line no-new
                new Vue({
                  el: '#v-stories',
                  components: {
                    'v-stories': vStories
                  }
                });
            }

    // eslint-disable-next-line no-unused-vars
      function initHousingCooperativeTest() {
            // eslint-disable-next-line no-new
            new Vue({
              el: '#housing-cooperative-test',
              components: {
                'housing-cooperative-test': HousingCooperativeTest
              }
            });
        }

    // eslint-disable-next-line no-unused-vars
      function initPollsMarathon() {
            // eslint-disable-next-line no-new
            new Vue({
              el: '#v-polls-marathon',
              components: {
                'v-polls-marathon': VPollsMarathon
              }
            });
        }

    // eslint-disable-next-line no-unused-vars
        function initRefinancingCalculator() {
            // eslint-disable-next-line no-new
            new Vue({
              el: '#v-refinancing-calculator',
              components: {
                'refinancing-calculator': RefinancingCalculator
              }
            });
        }

    // eslint-disable-next-line no-unused-vars
        function initItMortgageSurveysOneRequest() {
          // eslint-disable-next-line no-new
          new Vue({
            el: '#v-mortgage-surveys-one-request',
            components: {
              'it-mortgage-surveys-one-request': ItMortgageSurveysOneRequest
            }
          });
        }

    // eslint-disable-next-line no-unused-vars
      function initDduCalculator() {
            // eslint-disable-next-line no-new
            new Vue({
              el: '#v-ddu-calculator',
              components: {
                'ddu-calculator': DduCalculator
              }
            });
          }

    // eslint-disable-next-line no-unused-vars
        function initCreditCalculator() {
            // eslint-disable-next-line no-new
            new Vue({
              el: '#v-credit-calculator',
              components: {
                'credit-calculator': CreditCalculator
              }
            });
        }

    // eslint-disable-next-line no-unused-vars
        function initFamilyMortgageCalculator() {
            // eslint-disable-next-line no-new
            new Vue({
              el: '#v-family-mortgage-calculator',
              components: {
                'family-mortgage-calculator': FamilyMortgageCalculator
              }
            });
        }

    // eslint-disable-next-line no-unused-vars
        function initTaxDeductionCalculatorOld() {
              // eslint-disable-next-line no-new
              new Vue({
                el: '#v-tax-calculator-old',
                components: {
                  'tax-calculator-old': TaxDeductionCalculatorOld
                }
              });
            }

    // eslint-disable-next-line no-unused-vars
        function initGameddRegistration() {
              // eslint-disable-next-line no-new
              new Vue({
                el: '#v-gamedd-registration',
                components: {
                  'gamedd-registration': GameddRegistration
                }
              });
            }
    // eslint-disable-next-line no-unused-vars
        function initGameddNotifications() {
              // eslint-disable-next-line no-new
              new Vue({
                el: '#v-gamedd-notifications',
                components: {
                  'gamedd-notifications': GameddNotifications
                }
              });
            }
    // eslint-disable-next-line no-unused-vars
        function initApexChartProgressCompany() {
              // eslint-disable-next-line no-new
              new Vue({
                el: '#schedule-circle',
                components: {
                  'apex-chart-progress-company': ApexChartProgressCompany
                }
              });
            }
    // eslint-disable-next-line no-unused-vars
        function initMinSport() {
          // eslint-disable-next-line no-new
          new Vue({
            el: '#v-minsport',
            components: {
              'min-sport': MinSport
            }
          });
        }
    // eslint-disable-next-line no-unused-vars
        function initApexChartAt() {
          // eslint-disable-next-line no-new
          new Vue({
            el: '#another-thing-end',
            components: {
              'apex-chart-at': ApexChartAt
            }
          });
        }
    // eslint-disable-next-line no-unused-vars
        function initOcenka() {
          // eslint-disable-next-line no-new
          new Vue({
            el: '#v-ocenka',
            components: {
              'v-ocenka': VOcenka
            }
          });
        }
    // eslint-disable-next-line no-unused-vars
        function initSingleCalculator() {
            // eslint-disable-next-line no-new
            new Vue({
              el: '#v-single-calculator',
              components: {
                'single-calculator': SingleCalculator
              }
            });
        }
    // eslint-disable-next-line no-unused-vars
        function initMortgageRepaymentCalculator() {
          // eslint-disable-next-line no-new
          new Vue({
            el: '#v-mortgage-repayment-calculator',
            components: {
              'mortgage-repayment-calculator': MortgageRepaymentCalculator
            }
          });
        }
  // eslint-disable-next-line no-unused-vars
        function initSearchFamilyMortgage() {
          // eslint-disable-next-line no-new
          new Vue({
            el: '#search-family-mortgage',
            components: {
              'search-family-mortgage': SearchFamilyMortgage
            }
          });
        }

    const obj = [
        {
            id: 'app',
            fn: ocenkaInit
        },
        // Устаревшее приложение для
        // блока ключевых показателей с данными из БД ЕИСЖС
        // {
        //     id: 'appKi',
        //     fn: initKeyIndicator
        // },
        {
            id: 'm-calc',
            fn: initMrotgageCalc
        },
        {
          id: 'test',
          fn: test
        },
        {
          id: 'v-mortgage-rates',
          fn: initMortgageRates
        },
        {
            id: 'telegram-block',
            fn: initTelegramBlock
        },
        {
          id: 'status-by-kadastr',
          fn: initStatusByKadastr
        },
        {
          id: 'v-tax-calculator',
          fn: initTaxDeductionCalculator
        },
        {
          id: 'v-credit-holidays-calculator',
          fn: initCreditHolidaysCalculator
        },
        {
          id: 'v-mortgage-surveys',
          fn: initITMortgageSurveys
        },
        // {
          // id: 'v-polls',
          // fn: initPolls
        // },
        {
          id: 'v-mortgage-calculator',
          fn: initMortgageCalculator
        },
        {
          id: 'v-stories',
          fn: initStories
        },
        {
          id: 'housing-cooperative-test',
          fn: initHousingCooperativeTest
        },
        {
          id: 'v-polls-marathon',
          fn: initPollsMarathon
        },
        {
          id: 'v-refinancing-calculator',
          fn: initRefinancingCalculator
        },
        {
          id: 'v-mortgage-surveys-one-request',
          fn: initItMortgageSurveysOneRequest
        },
        {
          id: 'v-ddu-calculator',
          fn: initDduCalculator
        },
        {
          id: 'v-credit-calculator',
          fn: initCreditCalculator
        },
        {
          id: 'v-family-mortgage-calculator',
          fn: initFamilyMortgageCalculator
        },
        {
          id: 'v-tax-calculator-old',
          fn: initTaxDeductionCalculatorOld
        },
        {
          id: 'v-gamedd-registration',
          fn: initGameddRegistration
        },
        {
          id: 'v-gamedd-notifications',
          fn: initGameddNotifications
        },
        {
          id: 'schedule-circle',
          fn: initApexChartProgressCompany
        },
        {
          id: 'v-minsport',
          fn: initMinSport
        },
        {
          id: 'another-thing-end',
          fn: initApexChartAt
        },
        {
          id: 'at-form-end',
          fn: atEndForm
        },
        {
          id: 'form-rating',
          fn: formRating
        },
        {
          id: 'modal-housing-cooperative-request',
          fn: izhsSendForm
        },
        {
          id: 'v-ocenka',
          fn: initOcenka
        },
        {
          id: 'key-indicators',
          fn: keyIndicatorsNewSource
        },
        {
          id: 'select-with-search',
          fn: SelectWithSearch
        },
        {
          id: 'modal-catalog-new-request',
          fn: CatalogNewSendForm
        },
        // {
          // id: 'tax-deduction-new',
          // fn: taxDeductionNew
        // },
        {
          id: 'v-single-calculator',
          fn: initSingleCalculator
        },
        {
          id: 'modal-marathon-test-inn',
          fn: marathon2024TestInn
        },
        {
          id: 'marathon-new-last--wr-countdown',
          fn: marathon2024Button
        },
        {
          id: 'marathon-rating',
          fn: marathon2024Rating
        },
        {
          id: 'v-mortgage-repayment-calculator',
          fn: initMortgageRepaymentCalculator
        },
        {
          id: 'search-family-mortgage',
          fn: initSearchFamilyMortgage
        },
        {
          id: 'another-thing-speaker',
          fn: speakerInit
        },
        // {
        //   id: 'form-rating-universal',
        //   fn: formRatingUniversal
        // },
        // {
        //     id: 'form-rating-universal',
        //     fn: formRatingUniversal
        // },
        {
          id: 'marathon-2025-shedule',
          fn: marathon2025Shedule
        },

    ];

    // for (let i = 0; i < obj.length; i += 1) {
    //     initVueApp(obj[i]);
    // }
  function waitForVueApp(id, initFn) {
    const el = document.getElementById(id);
    // eslint-disable-next-line no-underscore-dangle
    if (el && !el.__vue_initialized) {
      initFn();
      // eslint-disable-next-line no-underscore-dangle
      el.__vue_initialized = true;
      return;
    }

    const observer = new MutationObserver(() => {
      const element = document.getElementById(id);
      // eslint-disable-next-line no-underscore-dangle
      if (element && !element.__vue_initialized) {
        observer.disconnect();
        initFn();
        // eslint-disable-next-line no-underscore-dangle
        element.__vue_initialized = true;
      }
    });

    observer.observe(document.body, { childList: true, subtree: true });
  }

// Использование:
  obj.forEach((item) => {
    waitForVueApp(item.id, item.fn);
  });

  // создание несколько экземпляров vue для вызова тестов в них
  const vueTestRoots = document.querySelectorAll('.js-test-n');

  if (vueTestRoots.length) {
    for (let i = 0; i < vueTestRoots.length; i += 1) {
      vueTestRoots[i].id = `test-n-${i}`;

      // eslint-disable-next-line no-new
      new Vue({
        el: `#test-n-${i}`,
        components: {
          'test-n': TestN,
        }
      });
    }
  }

  // создание несколько экземпляров vue для вызова тестов в них
    const vuePolls = document.querySelectorAll('.js-polls-n');

    if (vuePolls.length) {
      for (let i = 0; i < vuePolls.length; i += 1) {
        vuePolls[i].classList.add(`js__polls-n-${i}`);

        // eslint-disable-next-line no-new
        new Vue({
          el: `.js__polls-n-${i}`,
          components: {
            'v-polls': VPolls,
          }
        });
      }
    }

  // создание несколько экземпляров vue для вызова тестов в них
    const vueTestAnotherThing = document.querySelectorAll('.js-test-another-thing');

    if (vueTestAnotherThing.length) {
      for (let i = 0; i < vueTestAnotherThing.length; i += 1) {
        vueTestAnotherThing[i].id = `test-n-${i}`;

        // eslint-disable-next-line no-new
        new Vue({
          el: `#test-n-${i}`,
          components: {
            'gamedd-task': GameddTask,
          }
        });
      }
    }

  // создание несколько экземпляров vue для вызова тестов в них
      const vueMarathon2024Test = document.querySelectorAll('.js-test-marathon-2024');

      if (vueMarathon2024Test.length) {
        for (let i = 0; i < vueMarathon2024Test.length; i += 1) {
          vueMarathon2024Test[i].id = `test-n-${i}`;

          // eslint-disable-next-line no-new
          new Vue({
            el: `#test-n-${i}`,
            components: {
              'marathon-2024-test': Marathon2024Test,
            }
          });
        }
      }
});
if (module.hot) {
  module.hot.accept('../scss/style.scss', () => {
    console.log('[HMR] ✅ CSS updated');
  });
}


----------------------------------------------------------

webpack.config.js

const path = require('path');
const fs = require('fs');
const webpack = require('webpack');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const TerserPlugin = require('terser-webpack-plugin');
const VueLoaderPlugin = require('vue-loader/lib/plugin');

function generateHtmlPlugins(templateDir) {
  const templateFiles = fs.readdirSync(path.resolve(__dirname, templateDir));
  return templateFiles.map(item => {
    const [name, extension] = item.split('.');
    return new HtmlWebpackPlugin({
      filename: `${name}.html`,
      template: path.resolve(__dirname, `${templateDir}/${name}.${extension}`),
      inject: true,
      chunks: ['main', 'styles']
    });
  });
}

module.exports = (env, argv) => {
  const mode = argv.mode || 'development';
  const isProduction = mode === 'production';

  return {
    entry: {
      main: './src/js/index.js',
      styles: './src/scss/style.scss'
    },
    output: {
      filename: isProduction ? './js/[name].[contenthash:8].js' : './js/[name].js',
      publicPath: '/dist/'
    },
    devtool: isProduction ? 'source-map' : 'eval-cheap-module-source-map',
    mode,

    optimization: {
      minimize: isProduction,
      minimizer: isProduction ? [
        new TerserPlugin({ parallel: true, terserOptions: { compress: { drop_console: true } } })
      ] : [],
      splitChunks: isProduction ? {
        chunks: 'all',
        cacheGroups: {
          vendor: { test: /[\\/]node_modules[\\/]/, name: 'vendors', chunks: 'all' }
        }
      } : false
    },

    devServer: {
      contentBase: path.resolve(__dirname, 'dist'),
      publicPath: '/dist/',
      hot: true,
      port: 8080,
      open: true,
      watchOptions: { ignored: /node_modules/, aggregateTimeout: 50 }
    },

    module: {
      rules: [
        { test: /\.vue$/, use: ['cache-loader', 'vue-loader'] },
        {
          test: /\.pug$/,
          oneOf: [
            {
              include: path.resolve(__dirname, 'src/pug/'),
              exclude: /\.vue$/,
              use: ['cache-loader', {
                loader: 'pug-loader',
                options: {
                  quiet: true,
                  compiler: {
                    // 🔥 Подавляем warning о multiple attributes
                    onwarn: (warning, parser) => {
                      if (warning.code !== 'multiple-attributes') parser.warn(warning);
                    }
                  }
                }
              }]
            },
            { use: ['pug-plain-loader'] }
          ]
        },
        {
          test: /\.js$/,
          exclude: /node_modules/,
          use: ['cache-loader', { loader: 'babel-loader', options: { cacheDirectory: true } }]
        },
        {
          test: /\.(sass|scss)$/i,
          use: [
            mode === 'development' ? 'style-loader' : MiniCssExtractPlugin.loader, // ✅ исправлено
            'css-loader',
            'cache-loader',
            {
              loader: 'sass-loader',
              options: {
                implementation: require('sass'),
                sassOptions: {
                  quietDeps: true,
                  silenceDeprecations: ['slash-div', 'import', 'legacy-js-api']
                }
              }
            }
          ]
        },
        {
          test: /\.(png|jpe?g|gif|svg|webp)$/i,
          loader: 'file-loader',
          options: {
            name: 'img/[name].[ext]',
            publicPath: '/dist/'
          }
        },
        {
          test: /\.(woff2?|eot|ttf|otf)$/i,
          loader: 'file-loader',
          options: {
            name: 'fonts/[name].[ext]',
            publicPath: '/dist/'
          }
        }
      ]
    },

    plugins: (function () {
      const plugins = [
        new VueLoaderPlugin(),
        isProduction && new MiniCssExtractPlugin({ filename: './css/all.css' }), // ✅ только в prod
        new CopyWebpackPlugin([
          { from: './src/fonts', to: './fonts' },
          { from: './src/img', to: './img' }
        ]),
        new webpack.DefinePlugin({ 'process.env.NODE_ENV': JSON.stringify(mode) })
      ];

      if (mode === 'development') plugins.push(new webpack.HotModuleReplacementPlugin());
      if (isProduction) plugins.push(new CleanWebpackPlugin());

      if (!isProduction) {
        plugins.push(...generateHtmlPlugins('./src/pug/views'));
      }

      return plugins.filter(Boolean);
    }()),

    resolve: {
      alias: { vue: isProduction ? 'vue/dist/vue.min.js' : 'vue/dist/vue.js' },
      extensions: ['.js', '.vue', '.json']
    }
  };
};

