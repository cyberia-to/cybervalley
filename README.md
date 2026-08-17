# cybervalley.io

Исходники сайта. Хостинг — GitHub Pages из этого репо
(`cyberia-to/cybervalley`, ветка `main`): деплой = `git push`,
серверов и SSH в цикле публикации нет.

## структура

```
cybervalley/
├── index.html                  # https://cybervalley.io/
├── autonomy-tour/index.html    # https://cybervalley.io/autonomy-tour/
├── CNAME                       # кастомный домен для Pages
└── README.md                   # этот файл
```

## как править

Просто редактируй HTML. Один файл на страницу, стили и скрипты внутри —
никакого build-шага. Проверить локально:

```sh
cd ~/cyber/cybervalley
python3 -m http.server 8080
# открой http://localhost:8080/  и  http://localhost:8080/autonomy-tour/
```

Опубликовать:

```sh
git add -A && git commit -m 'feat: ...' && git push
# через ~минуту живо на https://cybervalley.io/
```

## аналитика

Трекер lytics грузится с `https://cyberstates.net/lytics/` — общий
ingest на cyberproxy. Кросс-доменные запросы разрешает CORS-настройка
ingest (`LYTICS_CORS_ORIGINS=https://cybervalley.io`); дашборд —
<https://cyberstates.net/lytics/>. Исходники: `~/cyber/lytics`.

## RSVP на autonomy-tour

Кнопка шлёт нотификацию напрямую в Telegram-бот из браузера
(у api.telegram.org CORS `*`, сервер не нужен). Токен бота вписан
в исходник страницы — так задумано: бот одноразовый, привязан к одному
чату; при спаме — revoke через `@BotFather`.
