# cybervalley.io — работа без SSH

Исходники сайта. Ты правишь файлы у себя, собираешь архив, отдаёшь
админу с SSH — он катит на cyberproxy по чек-листу в
[HANDOFF.md](HANDOFF.md).

## структура

```
cybervalley/
├── index.html                          # https://cybervalley.io/
├── autonomy-tour/index.html            # https://cybervalley.io/autonomy-tour/
├── scripts/
│   ├── nginx-cybervalley.io.conf       # референс vhost (для админа)
│   └── deploy.nu                       # прямой rsync — работает только с SSH
├── HANDOFF.md                          # чек-лист админу
└── README.md                           # этот файл
```

## как править

Просто редактируй HTML. Один файл на страницу, стили и скрипты внутри —
никакого build-шага. Проверить локально:

```sh
cd ~/cyber/cybervalley
python3 -m http.server 8080
# открой http://localhost:8080/  и  http://localhost:8080/autonomy-tour/
```

Что учитывать локально:
- Трекер (`/lytics/tracker/loader.js`) 404-ит — это нормально, он живёт
  только на проде.
- RSVP-кнопка на autonomy-tour отвечает "Could not send" — плейсхолдер
  токена (см. ниже).

## перед выкаткой — заполнить Telegram-токены

В `autonomy-tour/index.html` две строки:

```js
const TG_BOT_TOKEN = 'PASTE_BOT_TOKEN_HERE';
const TG_CHAT_ID   = 'PASTE_CHAT_ID_HERE';
```

Токен попадёт в исходник страницы — это by design. Правила:

1. Заведи **одноразового** бота через `@BotFather` (`/newbot`).
   Название `cybervalley_autonomy_tour_bot` или подобное.
2. Добавь бота в чат, куда должны падать RSVP-ы.
3. Получи `chat_id`: отправь в чат любое сообщение и вызови
   `https://api.telegram.org/bot<TOKEN>/getUpdates` — там будет
   `"chat":{"id":-100...}`.
4. Впиши оба значения в `autonomy-tour/index.html` и **не коммить их
   в публичный git**. Если репозиторий уходит на GitHub — держи файл
   с токенами вне git (например, `autonomy-tour/index.html.local` +
   `.gitignore`), а для деплоя копируй.
5. Если бота начнут спамить — revoke в `@BotFather` (`/revoke`) и
   выпусти новый.

## собрать архив и отдать админу

```sh
cd ~/cyber
tar czf /tmp/cybervalley-deploy.tgz \
  --exclude='.git' --exclude='.DS_Store' \
  cybervalley/
ls -lh /tmp/cybervalley-deploy.tgz     # ~15 KB
```

Отправь `cybervalley-deploy.tgz` + ссылку на
[HANDOFF.md](HANDOFF.md) админу. Дальше — его работа.

## что деплоится в первый раз

- Правится `/var/www/html/cybervalley/index.html` (та же главная +
  ссылка на `/autonomy-tour/` + встроен lytics-трекер).
- Появляется `/var/www/html/cybervalley/autonomy-tour/index.html`.
- В nginx добавляется `location /lytics/` — прокси на локальный
  lytics-ingest (`127.0.0.1:8091`), тот же, что уже обслуживает
  cyberstates.net. Аналитика с cybervalley.io попадает в тот же store,
  бакетится по `data-domain`.

## следующие изменения (быстрый цикл)

Если nginx уже настроен (после первой выкатки), обычный workflow:

1. Правишь HTML у себя.
2. Локально проверил через `python3 -m http.server`.
3. `tar czf /tmp/cybervalley-deploy.tgz --exclude='.git' cybervalley/`.
4. Отдаёшь админу — он делает только шаг 1 из HANDOFF.md (rsync).
   Шаг 2 (nginx) не нужен — он одноразовый.

## что дальше

- **Больше событий** — новый каталог `<slug>/index.html` рядом с
  `autonomy-tour/`. Добавь ссылку на "рунге" в главной.
- **RSVP через lytics** — если Telegram-бот начнёт задалбывать спамом,
  вынести POST на серверную сторону: добавить `/lytics/rsvp` endpoint
  в `~/cyber/lytics/rs/ingest/` и убрать токен со страницы. Тогда
  админ выкатит и `lytics-ingest` через
  `~/cyber/lytics/scripts/deploy-cyberproxy.nu`.
