# CONTEXT — sellvia-auto-promotion

Лендинг Sellvia. Одностраничный сайт под оффер автоматического продвижения магазина
(auto-promotion / реклама в один клик).

## Проект
- Локальная папка: `D:\Work\sellvia-auto-promotion\`
- GitHub: https://github.com/daryakravchenko-oss/sellvia-auto-promotion (ветка `main`)
- Открывать страницу: `D:/Work/sellvia-auto-promotion/index.html` как файл или через локальный
  http-сервер — отображается одинаково (пути относительные).
- Происхождение: собран из присланного `free-ecommerce-store-plus-amazon-package.html` (страница amazon-package); hero переписан под оффер auto-promotion.

## Структура
```
sellvia-auto-promotion/
├── index.html          # единый файл: HTML + inline CSS + inline JS (~5000 строк)
├── .gitignore
├── CONTEXT.md           # этот файл
└── images-for-landings/
    ├── free-ecommerce-store-plus-amazon-package/            # ассеты со страницы amazon-package
    ├── free-online-store-plus-instagram-and-tiktok-store/   # ассеты со страницы instagram-tiktok
    └── sellvia-auto-promotion/   # НОВЫЕ ассеты этого проекта, по секциям
        ├── hero/          a.svg f.svg g.svg i.svg t.svg  (логотипы Amazon/Facebook/Google/Instagram/TikTok)
        ├── bonus/         1.webp + amazon.svg facebook.svg google.svg insta.svg tiktok.svg
        ├── freebasic/     screen2.webp
        ├── what-do-i-get/ 6 webp-иконок get-sec
        └── awards/ faq/ final-cta/ footer/ how-it-works/ ratings/ stat-bar/
            store-designs/ testimonials/ what-will-i-sell/   (пока пустые, .gitkeep)
```

## Пути к картинкам — ВАЖНО
- Все пути к картинкам — ОТНОСИТЕЛЬНЫЕ, вида `images-for-landings/{slug}/{папка}/{файл}`
  (относительно index.html).
- НИКОГДА не использовать абсолютные `file://`-URL — только относительные пути.
  (Относительные работают и при открытии как файла, и через http-сервер; `file://` — нет.)
- Три slug-папки в `images-for-landings/`:
  - `free-ecommerce-store-plus-amazon-package` — перенесённые ассеты amazon-страницы;
  - `free-online-store-plus-instagram-and-tiktok-store` — перенесённые ассеты tiktok-страницы;
  - `sellvia-auto-promotion` — НОВЫЕ ассеты этого проекта, разложены по подпапкам-секциям.
- ПРАВИЛО: все новые картинки/элементы складывать в `images-for-landings/sellvia-auto-promotion/{секция}/`
  и ссылаться относительным путём.

## Секции страницы (сверху вниз)
Topbar, Nav, Hero, Stat bar, Freebasic, Bonus, How it works, Store designs marquee,
What do I get, What will I sell, Awards, Ratings, Testimonials, FAQ, Final CTA, Footer, Sticky CTA.

## Что уже сделано
- index.html собран из amazon-package; все пути к картинкам — относительные.
- Topbar → «FREE store + automated promotion!»
- Hero: H1 «Claim your FREE store with auto-promotion!», подзаголовок про hands-free advertising,
  фон полностью пересобран — эффект «плитки-прожектор» (см. ниже).
- Freebasic: телефонный скрин (`pv-screen-img`) → `sellvia-auto-promotion/freebasic/screen2.webp`.
- Bonus: пересобран как анимированный bento-showcase (коммит `28bd3f4`). Заголовок «Enjoy guaranteed
  sales on full autopilot!», подзаголовок про TOP 5 рекламных гигантов, пилюля «STEP 2 – AUTO-PROMOTION».
- How-sec: карточка 2 переписана под тему авто-промоушна («We promote it for you»).
- Get-sec («What do I get»): секция перенесена из проекта `sellvia-instagram-and-tiktok-store`
  (металлическая сетка плиток `.get-list`/`.get-tile`, glow-orb за курсором, параллакс иконок).
- Final CTA: заголовок «Start selling on full autopilot», подзаголовок про hands-free promotion;
  плоский тёмный фон `#0b0b0d`. По нижнему краю — «эквалайзер» из 15 столбиков впритык, профиль-долина
  (низкие в центре под текстом, к краям дорастают до верха заголовка). Столбики плавно
  опускаются/поднимаются (CSS `scaleY`, у каждого свои фаза и скорость) и переливаются мягкими
  холодными оттенками снизу вверх (голубой→сирень→розовый, без красного/жёлтого/зелёного: анимация
  `background-position` по циклическому градиенту + `mask`-фейд сверху). Высоты/тайминги задаёт JS
  (`#finalBars`), CSS-переменные `--h/--s1/--s2/--bd/--bdelay/--cdelay`.

## Hero-эффект (текущая реализация)
Сетка плиток-логотипов, по умолчанию невидимых; локально открывается движущимся «прожектором».
- CSS: `@property --bx/--by` (registered %-переменные); `@keyframes hero-blob-path` — траектория
  пятна, цикл ~30с, огибает центр с текстом; анимация на `.hero`.
  - `.hero-tiles` (z-index 1) — CSS-grid; маска `radial-gradient(circle 300px at var(--bx) var(--by) ...)`
    — плитки видны только у пятна.
  - `.hero-tile` — цвет бренда через `--c` (тройка «r g b»): яркая обводка + многослойный
    box-shadow-glow + цветной ореол у логотипа.
  - `.hero-textshield` (z-index 2) и `.hero-inner::before` — тёмная подложка под текстом и кнопками.
  - `.hero-inner` — z-index 3.
- JS: IIFE `// ── HERO TILE GRID` генерирует сетку (в каждой плитке логотип; 2 одинаковых рядом
  не стоят), ставит `--c` каждой плитке; пересобирается при resize.
- Логотипы `a/f/g/i/t.svg` = Amazon / Facebook / Google / Instagram / TikTok. Бренд-цвета — в JS-карте `BRAND`.

## Незакрытые хвосты / заметки
- Мёртвый CSS: ниже нового tile-CSS в index.html остались ~340 строк старых hero-фон стилей
  (`.hero-glow*`, `.hero-portrait*`, `.hp-amz*`, `.hero-float*`, `.hf-*`, их `@keyframes`,
  `.hero-bg::after`) — не используются, безвредны, ждут уборки.
- В старых slug-папках есть уже неиспользуемые ассеты (`.../amazon-package/Hero/`, `.../amazon-package/Bonus/`
  — эти секции пересобраны на новые ассеты) — можно подчистить при желании.
- Папка `img/` удалена. В CSS остался один `url('img/Hero/arrow.svg')` (декоративная стрелка) —
  файла нет, фон битый (мелочь).
- Скриншот-инструменты в сессиях не работали (preview/Chrome screenshot — таймаут: страница
  тяжёлая из-за аналитики + rAF-маркизов). Проверка велась через DOM-инспекцию (`preview_eval`).
  Визуально — открыть index.html (как файл или через локальный сервер).

## Git
Репозиторий `daryakravchenko-oss/sellvia-auto-promotion`, ветка `main`. Последние коммиты:
- `28bd3f4` — Rebuild bonus section as an animated bento showcase
- `4958c01` — Build tiled-logo spotlight hero background
- `d562285` — Remove unused img/ folder
- `b1dabe4` — Clean hero background and scaffold section image folders
- `5bd3c2c` — Rebuild index.html from new base page
- `96b91d0` — Add landing page based on free-ecommerce-store-plus-amazon-package

Коммитить/пушить — только по команде пользователя («сделай пуш» / «да»).

## Как работать
- Единый файл index.html — править HTML/CSS/JS инлайн.
- Новые картинки → `images-for-landings/sellvia-auto-promotion/{секция}/`, ссылаться ОТНОСИТЕЛЬНЫМ путём
  (не `file://`).
- Пользователь смотрит результат, открывая index.html, и даёт правки по секциям.
- Идём по секциям: hero, freebasic, bonus, how-sec, get-sec, final-cta — готовы; дальше остальные.
- Для Sellvia-контекста (бренд, тон, терминология) вызвать skill `sellvia-knowledge`.
  Терминология: не использовать «dropshipping», «passive income», «get rich quick» и т.п.

Новому диалогу: сначала прочитать этот CONTEXT.md, затем вызвать skill `sellvia-knowledge`.
