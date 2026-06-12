# ROI Calculator (Dubai) — схема проекта

Одностраничный калькулятор доходности недвижимости в Дубае. Статический сайт без сборки —
один `index.html` со встроенным CSS/JS. Хостится на GitHub Pages для доступа с телефона
и компьютера по постоянной ссылке.

## Структура

```
roi-calculator/
├── index.html              ← всё приложение (UI + расчёты + генерация PDF-отчёта)
├── manifest.webmanifest     ← PWA-манифест (установка на главный экран)
├── sw.js                     ← service worker (офлайн-кэш)
├── icons/                    ← иконки PWA (192/512/maskable)
└── CLAUDE.md                 ← вы здесь
```

## Логика расчёта (index.html)

- **Сценарии**: готовый объект / офф-план · наличные / ипотека · долгосрочная / краткосрочная аренда — переключаются через `M = {prop, fin, rent}` и `setMode()`.
- `calc()` — все формулы (DLD, комиссии, ипотечный платёж, NOI, cash-on-cash, ROI за 5 лет, офф-план ROI до handover).
- `recalc()` — рендерит блок результатов при каждом изменении инпута.
- `buildReport()` / `downloadPDF()` — экран отчёта и экспорт в PDF (html2canvas + jsPDF, подключены с CDN).

## Изменение допущений по умолчанию

Стартовые значения полей (цена, площадь, ставки и т.д.) — это атрибуты `value="..."` у
`<input>` в `index.html`. DLD/комиссии/трасти-сборы — константы внутри `calc()`.

## Ссылка

**https://levanskiy.github.io/roi-calculator/**

Репозиторий: https://github.com/Levanskiy/roi-calculator

## Деплой (GitHub Pages)

1. Закоммитить изменения в `main`.
2. `git push` — Pages пересобирается автоматически (Settings → Pages → Deploy from branch `main` / root).
3. Ссылка остаётся той же: https://levanskiy.github.io/roi-calculator/

После любых правок в `index.html`/иконках — обновить `CACHE` версию в `sw.js` (например `roi-dubai-v2`), иначе у пользователей со старым кэшем не подхватится новая версия сразу.

## Локальная проверка

```bash
cd roi-calculator
python3 -m http.server 8000
# открыть http://localhost:8000
```

(Service worker не зарегистрируется на `file://`, нужен http-сервер.)
