# cybervalley.io

Site sources. Hosting is GitHub Pages out of this repo
(`cyberia-to/cybervalley`, branch `main`): deploy is `git push` — no
server and no SSH anywhere in the publishing loop.

Live at <https://cyberia-to.github.io/cybervalley/>. The custom domain
is off until cybervalley.io DNS points at Pages — the A records are
`185.199.108.153`, `185.199.109.153`, `185.199.110.153`,
`185.199.111.153`, plus `CNAME www → cyberia-to.github.io.`. To switch,
restore a `CNAME` file holding `cybervalley.io` and set the domain in
the repo's Pages settings; GitHub then issues the certificate.

## layout

```
cybervalley/
├── index.html                  # site root
├── autonomy-tour/index.html    # /autonomy-tour/ — the Aug 26 event
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
# live within a minute
```

## analytics

The lytics tracker loads from `https://cyberstates.net/lytics/` — the
shared ingest on cyberproxy. Cross-origin requests are allowed by the
ingest CORS allowlist (`LYTICS_CORS_ORIGINS=https://cybervalley.io`);
dashboard: <https://cyberstates.net/lytics/>. Sources: `~/cyber/lytics`.

## autonomy-tour RSVP

A plain link to the <https://t.me/cyberialand> group — new visitors say
they are coming there and the host answers. No form, no bot, no token:
the group is the guest list and the comms channel at once.
