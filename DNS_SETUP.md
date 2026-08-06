# Pointing cozylogic.store at the site — Namecheap

The GitHub side is done: the repo now claims `cozylogic.store`, which is the
order GitHub's own documentation insists on — claiming the domain first is what
stops anyone else hosting a site on it while DNS is in flight.

What is left is DNS, and that lives in your Namecheap account.

## Where to go

`Namecheap → Domain List → cozylogic.store → Manage → Advanced DNS`

## 1 · Delete what is there now

The domain currently answers with Namecheap's parking page. Those records have
to go or they will fight the new ones:

- the `A Record` for `@` pointing at `192.64.119.192`
- the `CNAME Record` for `www` pointing at `parkingpage.namecheap.com`
- any `URL Redirect Record`

## 2 · Add these six records

**Four A records** — all with Host `@`, all with the same TTL (Automatic is
fine). Four separate rows, not one row with four values:

| Type | Host | Value |
|---|---|---|
| A Record | `@` | `185.199.108.153` |
| A Record | `@` | `185.199.109.153` |
| A Record | `@` | `185.199.110.153` |
| A Record | `@` | `185.199.111.153` |

**One CNAME** so `www.cozylogic.store` reaches the same site:

| Type | Host | Value |
|---|---|---|
| CNAME Record | `www` | `mohammedbenchaa.github.io.` |

⚠️ Namecheap sometimes strips the trailing dot. Either form works.

**Optionally**, four AAAA records for IPv6, same Host `@`:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

Not required. Add them if you want the site reachable over IPv6.

## 3 · Wait

Namecheap usually publishes within about 30 minutes; DNS can take a few hours
to spread. GitHub rechecks on its own.

## 4 · Then HTTPS

Once DNS resolves, GitHub issues a Let's Encrypt certificate automatically —
usually within an hour. Only after the certificate exists can **Enforce HTTPS**
be turned on, in the repo's Pages settings or by API. Until then the site is
served over plain HTTP.

**Do not paste the URL into Play until HTTPS is on.** Play wants a working
`https://` privacy policy link, and a certificate that is still being issued
will fail their check.

## The URLs, once it is live

| | |
|---|---|
| Privacy policy — for Play, twice | `https://cozylogic.store/privacy.html` |
| Website — the listing's optional field | `https://cozylogic.store/` |

The old `mohammedbenchaa.github.io/cozylogic/` addresses will redirect here, so
nothing that already points at them breaks.

## Checking it yourself

```bash
nslookup cozylogic.store 8.8.8.8
```

Success looks like the four `185.199.*` addresses. While it still shows
`192.64.119.192`, the old records are either still cached or were not removed.
