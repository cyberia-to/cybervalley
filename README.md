# cybervalley.io

Site sources. Hosting is GitHub Pages out of this repo
(`cyberia-to/cybervalley`, branch `main`): deploy is `git push` — no
server and no SSH anywhere in the publishing loop.

## layout

```
cybervalley/
├── index.html                  # https://cybervalley.io/
├── autonomy-tour/index.html    # https://cybervalley.io/autonomy-tour/
├── CNAME                       # custom domain for Pages
└── README.md                   # this file
```

## editing

Edit the HTML. One file per page, styles and scripts inline — no build
step. Check locally:

```sh
cd ~/cyber/cybervalley
python3 -m http.server 8080
# open http://localhost:8080/  and  http://localhost:8080/autonomy-tour/
```

Publish:

```sh
git add -A && git commit -m 'feat: ...' && git push
# live on https://cybervalley.io/ within a minute
```

## analytics

The lytics tracker loads from `https://cyberstates.net/lytics/` — the
shared ingest on cyberproxy. Cross-origin requests are allowed by the
ingest CORS allowlist (`LYTICS_CORS_ORIGINS=https://cybervalley.io`);
dashboard: <https://cyberstates.net/lytics/>. Sources: `~/cyber/lytics`.

## autonomy-tour RSVP

The button posts a notification straight to a Telegram bot from the
browser (api.telegram.org sends CORS `*`, so no server is involved).
The bot token sits in the page source by design: the bot is disposable
and bound to one chat; revoke through `@BotFather` if it gets abused.
