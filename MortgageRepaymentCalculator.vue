&__training-materials-tables {
  display: grid;
  grid-gap: $gap-in-train;
  grid-template-columns: repeat(6, 1fr);

  // 1️⃣ БАЗА: Стандартные стили для ВСЕХ карточек
  & > div {
    grid-column: span 2;
    .another-thing-2025__training-materials-item-second-col {
      display: block;
    }
  }

  // =================================================================
  // 🔹 ЧАСТЬ 1: Явные правила для 2–9 блоков (под ваши медиа-запросы)
  // =================================================================
  &.with-long-block {

    // 2 блока
    &:has(> :nth-child(2):last-child) {
      > :first-child {
        grid-column: span 4;
        & > div { flex-basis: 50%; }
      }
      & > div:not(:first-child) {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col { display: none; }
      }
      // 👇 Сюда вставляйте ваши медиа-запросы именно для 2 блоков
      // @media (max-width: 768px) { ... }
    }

    // 3 блока
    &:has(> :nth-child(3):last-child) {
      & > div {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col { display: none; }
      }
      // 👇 Медиа-запросы для 3 блоков
    }

    // 4 блока
    &:has(> :nth-child(4):last-child) {
      > :first-child,
      > :nth-child(4) {
        grid-column: span 4;
        & > div { flex-basis: 50%; }
      }
      & > div:not(:first-child):not(:nth-child(4)) {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col { display: none; }
      }
      // 👇 Медиа-запросы для 4 блоков
    }

    // 5 блоков
    &:has(> :nth-child(5):last-child) {
      > :first-child {
        grid-column: span 4;
        & > div { flex-basis: 50%; }
      }
      & > div:not(:first-child) {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col { display: none; }
      }
      // 👇 Медиа-запросы для 5 блоков
    }

    // 6 блоков
    &:has(> :nth-child(6):last-child) {
      & > div {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col { display: none; }
      }
      // 👇 Медиа-запросы для 6 блоков
    }

    // 7 блоков
    &:has(> :nth-child(7):last-child) {
      > :first-child,
      > :nth-child(7) {
        grid-column: span 4;
        & > div { flex-basis: 50%; }
      }
      & > div:not(:first-child):not(:nth-child(7)) {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col { display: none; }
      }
      // 👇 Медиа-запросы для 7 блоков
    }

    // 8 блоков
    &:has(> :nth-child(8):last-child) {
      > :first-child {
        grid-column: span 4;
        & > div { flex-basis: 50%; }
      }
      & > div:not(:first-child) {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col { display: none; }
      }
      // 👇 Медиа-запросы для 8 блоков
    }

    // 9 блоков
    &:has(> :nth-child(9):last-child) {
      & > div {
        grid-column: span 2;
        .another-thing-2025__training-materials-item-second-col { display: none; }
      }
      // 👇 Медиа-запросы для 9 блоков
    }

    // =================================================================
    // 🔹 ЧАСТЬ 2: Универсальные правила для 10+ блоков (повторяют паттерн)
    // =================================================================
    // Срабатывают ТОЛЬКО если карточек 10 или больше
    &:has(> :nth-child(10)) {

      // Остаток 2 (11, 14, 17...): первый особый → как для 2, 5, 8
      &:has(> :last-child:nth-child(3n+2)) {
        & > :first-child {
          grid-column: span 4;
          & > div { flex-basis: 50%; }
        }
        & > div:not(:first-child) {
          grid-column: span 2;
          .another-thing-2025__training-materials-item-second-col { display: none; }
        }
      }

      // Остаток 1 (10, 13, 16...): первый и последний особые → как для 4, 7
      &:has(> :last-child:nth-child(3n+1)) {
        & > :first-child,
        & > :last-child {
          grid-column: span 4;
          & > div { flex-basis: 50%; }
        }
        & > div:not(:first-child):not(:last-child) {
          grid-column: span 2;
          .another-thing-2025__training-materials-item-second-col { display: none; }
        }
      }

      // Кратно 3 (12, 15, 18...): все стандартные → как для 3, 6, 9
      &:has(> :last-child:nth-child(3n)) {
        & > div {
          grid-column: span 2;
          .another-thing-2025__training-materials-item-second-col { display: none; }
        }
      }
    }
  }

  // =================================================================
  // 🔹 НЕ with-long-block: скрываем вторую колонку у всех
  // =================================================================
  &:not(.with-long-block) {
    & > div {
      .another-thing-2025__training-materials-item-second-col {
        display: none;
      }
    }
  }
}
