# service-pinger

Pings your services on an interval so hosts that spin down idle apps (Render, Railway, Fly.io, Koyeb, Heroku, and similar) don't put them to sleep. Zero dependencies, pure Node.js. Includes a status dashboard.

## Usage

```
node index.js
```

Runs anywhere Node 18+ does.

## Environment variables

| Variable | Required | Description |
|---|---|---|
| `PING_URLS` | No | Comma-separated list of URLs to ping. `https://` is added automatically if missing. |
| `PING_INTERVAL_MS` | No | Ping interval in minutes. Defaults to 10. |
| `SELF_PING_URL` | No | Public URL of this service, so it pings itself too. Detected automatically on common hosts. |
| `PORT` | No | Dashboard port. Defaults to 3000. |

Example:
```
PING_URLS=example.com,myapp.up.railway.app,myapp.fly.dev,myapp.herokuapp.com
```

## Hosting the pinger itself

If the pinger runs on a host that sleeps idle apps, it needs to be pinged too. It pings itself automatically using `SELF_PING_URL`, or else the first of these that is set: `RENDER_EXTERNAL_URL`, `RAILWAY_PUBLIC_DOMAIN`, `RAILWAY_STATIC_URL`, `KOYEB_PUBLIC_DOMAIN`, `FLY_APP_NAME`, `HEROKU_APP_NAME` (Heroku needs dyno metadata enabled).

On any other host, or behind a custom domain, set `SELF_PING_URL` to the pinger's public URL, or add that URL to `PING_URLS`. With nothing set it falls back to `localhost`, which does not keep a hosted service awake. The pinger answers on `/ping` and `/health`.

## Notes

- All pings run concurrently.
- The dashboard auto-refreshes every 30 seconds.
- Times are shown in US Central time.

## License
This project is licensed under the MIT License - you are free to use, modify, and distribute it.
