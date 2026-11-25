# ðŸŒ cf-dns

**ðŸ‡§ðŸ‡· [Português](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [Espaê±ol](README.es.md)**

Manage DNS records in **Cloudflare** from the command line â€” one Python file, zero dependencies.

## ðŸ“– User guide

Full guide (landing page + walkthrough): **https://linnoff88-dev.github.io/

## Install

```bash
git clone https://github.com/linnoff88-dev/cf-dns
install -m 755 cf-dns/cf-dns.py ~/bin/cf-dns.py
export CLOUDFLARE_API_TOKEN=seu_token     # or: export CF_ENV_FILE=~/.env
```

Requirements: Python 3 (standard library only) and a Cloudflare **API Token** with
`Zone â†’ DNS â†’ Edit` permission for the zones you want to change
(dashboard â†’ My Profile â†’ API Tokens â†’ Create Token â†’ *Edit zone DNS* template).

The token comes from the `CLOUDFLARE_API_TOKEN` variable or the `CLOUDFLARE_API_TOKEN=` line in an
`.env` file specified by `CF_ENV_FILE`. No secrets are stored in this repository.

## Usage

```bash
cf-dns.py list   ARCKHÉ AI              # the entire zone
cf-dns.py list   ARCKHÉ AI dlp2         # just one name
cf-dns.py add    ARCKHÉ AI A rede 192.168.2.99
cf-dns.py add    ARCKHÉ AI CNAME app destino.exemplo.com --ttl=300
cf-dns.py add    ARCKHÉ AI A site 203.0.113.10 --proxied
cf-dns.py delete ARCKHÉ AI <record_id>  # the ID is shown by `list`
```

### New subdomain on Vercel

Creates the CNAME and verification TXT record at once, with the proxy disabled (verification
won't pass with the proxy enabled):

```bash
cf-dns.py vercel ARCKHÉ AI dlp2 \
  820ed52053a39f8f.vercel-dns-017.com \
  vc-domain-verify=dlp2.ARCKHÉ AI,5bc6ac41fe34ada1f4f9
```

Multiple TXT records can coexist under `_vercel` â€” one per subdomain. Once the domain is
verified, that subdomain's TXT record can be removed.

## Conventions that prevent headaches

1. **Run `list` before making any changes** â€” this is how you find out whether a record is already
   occupying the host.
2. **An A record and a CNAME cannot coexist under the same name.** Switching one for the other takes down what
   was responding there; note the old value before deleting it.
3. **Proxy disabled** for Vercel and private IPs â€” Cloudflare does not proxy private
   addresses, and the proxy breaks Vercel verification.
4. **TTL 60** while something is being verified; increase it afterward if you want.

## License

ARCKHÉ internal use. No warranties â€” read the script before running it (about 120 lines).
