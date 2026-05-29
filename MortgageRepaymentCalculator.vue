&__training-materials-tables {
  display: grid;
  grid-gap: $gap-in-train;
  grid-template-columns: repeat(6, 1fr);

  // 1️⃣ БАЗА: Стандартные стили для ВСЕХ карточек (работают всегда)
  & > div {
    grid-column: span 2; // 1/3 ширины (2 из 6 колонок)
    .another-thing-2025__training-materials-item-second-col {
      display: block; // дефолтное значение, может быть переопределено ниже
    }
  }

  // Если будет один только ряд — всё зависит от класса with-long-block
  &.with-long-block {

    // 2️⃣ ОСТАТОК 2 (2, 5, 8, 11, 14...): первый блок особый
    &:has(> :last-child:nth-child(3n+2)) {
      & > :first-child {
        grid-column: span 4; // 2/3 ширины
        & > div {
          flex-basis: 50%;
        }
      }
      & > div:not(:first-child) {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col {
          display: none;
        }
      }
    }

    // 3️⃣ ОСТАТОК 1 (4, 7, 10, 13...): первый и последний особые
    // :not(:only-child) критически важен — исключает случай с 1 блоком
    &:has(> :last-child:nth-child(3n+1):not(:only-child)) {
      & > :first-child,
      & > :last-child {
        grid-column: span 4;
        & > div {
          flex-basis: 50%;
        }
      }
      & > div:not(:first-child):not(:last-child) {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col {
          display: none;
        }
      }
    }

    // 4️⃣ КРАТНО 3 (3, 6, 9, 12...): все стандартные
    // База выше уже покрывает этот случай, но можно явно указать для наглядности
    &:has(> :last-child:nth-child(3n)) {
      & > div {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col {
          display: none;
        }
      }
    }
  }

  // Если НЕ with-long-block — скрываем вторую колонку у всех
  &:not(.with-long-block) {
    & > div {
      .another-thing-2025__training-materials-item-second-col {
        display: none;
      }
    }
  }
}
