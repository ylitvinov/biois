# biois_docs

Статический сайт с инструкциями по применению биологических средств защиты растений для компании BIOIS. Astro + GitHub Pages, домен `docs.biois.ru`.

## Стек
- **Astro 6** (static site generator), Node ≥ 22.12
- Без CSS-фреймворков: стили инлайн в `.astro` через `<style>`
- Без TypeScript-логики (только `tsconfig.json` для проверок)

## Структура
```
src/
  layouts/BaseLayout.astro   — общий каркас (header/footer/meta)
  pages/
    index.astro              — главная: карточки-ссылки на статьи
    instructions/*.astro     — статьи-инструкции (одна страница = одна статья)
public/
  images/                    — картинки, ссылаются как /images/...
  videos/
astro.config.mjs             — site: https://docs.biois.ru
CNAME                        — docs.biois.ru (GitHub Pages)
.github/workflows/           — деплой при push в main
```

## Команды
| Команда | Что делает |
|---|---|
| `npm run dev` | Локальный сервер (по умолчанию :4321; занят — соседний порт) |
| `npm run dev -- --port 7890` | На указанном порту |
| `npm run build` | Сборка в `./dist/` |
| `npm run preview` | Локальный предпросмотр собранного |

## Деплой
Push в `main` → GitHub Actions (`.github/workflows/`) собирает `dist/` и публикует на GitHub Pages → `https://docs.biois.ru` (1–2 минуты).

Стандартный порядок:
```sh
git add <конкретные файлы>          # без -A, чтобы не зацепить лишнее
git commit -m "<сообщение на английском>"
git fetch origin && git rebase origin/main  # если remote ушёл вперёд
git push origin main
```

Remote: `git@github.com:ylitvinov/biois_docs.git` (старый URL `ylitvinov/biois` работает по редиректу — лучше обновить через `git remote set-url`).

## Добавление новой статьи
1. Создать `src/pages/instructions/<slug>.astro` — копировать структуру существующей (например `phytoseiulus.astro`).
2. Добавить карточку-ссылку в `src/pages/index.astro` в секции `<section class="cards">` с `href="/instructions/<slug>"`.
3. Картинки класть в `public/images/<slug>/`, ссылаться как `/images/<slug>/file.jpg`.

## Соглашения по статьям-инструкциям
Чтобы стиль не разъезжался между статьями:
- Заголовок: `Инструкция по применению <чего>` в `<h1>`, title — то же + `— BIOIS`.
- Возврат на главную: `<a href="/" class="back-link">&larr; Все инструкции</a>` первым элементом.
- Нумерованные разделы: `1. О продукте`, `2. Подготовка перед применением`, `3. Способ внесения / применения`, `4. Нормы выпуска / применения`, `5. Оптимальные условия`, `6. Оценка эффективности`, `7. Наблюдение и мониторинг` (если применимо).
- Блоки `class="section warning"` (жёлтый, `<h2>Важно!</h2>`) и `class="section danger"` (красный, `<h2>Особые указания!</h2>`) — **без номера**, `danger` всегда последний.
- Латинские названия видов — в `<em>`.

## Полезное
- Локальный URL после `npm run dev`: смотреть в выводе (обычно `http://localhost:4321/`).
- Если карточка на главной добавлена, а статьи нет — `npm run build` упадёт с broken link.
