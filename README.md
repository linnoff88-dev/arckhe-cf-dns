# ðŸŒ cf-dns

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

Registro de DNS no **Cloudflare** pela linha de comando â€” um arquivo Python, zero dependÃªncias.

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/cf-dns/guia/**

## Instalar

```bash
git clone https://github.com/inematds/cf-dns
install -m 755 cf-dns/cf-dns.py ~/bin/cf-dns.py
export CLOUDFLARE_API_TOKEN=seu_token     # ou: export CF_ENV_FILE=~/.env
```

Requisitos: Python 3 (sÃ³ a biblioteca padrÃ£o) e um **API Token** do Cloudflare com
permissÃ£o `Zone â†’ DNS â†’ Edit` nas zonas que vocÃª vai alterar
(painel â†’ My Profile â†’ API Tokens â†’ Create Token â†’ template *Edit zone DNS*).

O token vem da variÃ¡vel `CLOUDFLARE_API_TOKEN` ou da linha `CLOUDFLARE_API_TOKEN=` de um
arquivo `.env` apontado por `CF_ENV_FILE`. Nenhum segredo mora neste repositÃ³rio.

## Usar

```bash
cf-dns.py list   inema.club              # a zona inteira
cf-dns.py list   inema.club dlp2         # sÃ³ um nome
cf-dns.py add    inema.club A rede 192.168.2.99
cf-dns.py add    inema.club CNAME app destino.exemplo.com --ttl=300
cf-dns.py add    inema.club A site 203.0.113.10 --proxied
cf-dns.py delete inema.club <record_id>  # o id sai no `list`
```

### SubdomÃ­nio novo no Vercel

Cria o CNAME e o TXT de verificaÃ§Ã£o de uma vez, com o proxy desligado (com o proxy
ligado a verificaÃ§Ã£o nÃ£o passa):

```bash
cf-dns.py vercel inema.club dlp2 \
  820ed52053a39f8f.vercel-dns-017.com \
  vc-domain-verify=dlp2.inema.club,5bc6ac41fe34ada1f4f9
```

VÃ¡rios TXT em `_vercel` convivem â€” um por subdomÃ­nio. Depois que o domÃ­nio fica
verificado, o TXT daquele subdomÃ­nio pode ser removido.

## ConvenÃ§Ãµes que evitam dor de cabeÃ§a

1. **Rode `list` antes de qualquer alteraÃ§Ã£o** â€” Ã© assim que se descobre um registro jÃ¡
   ocupando o host.
2. **Um A e um CNAME nÃ£o coexistem no mesmo nome.** Trocar um pelo outro derruba o que
   estava respondendo ali; anote o valor antigo antes de apagar.
3. **Proxy desligado** para Vercel e para IP privado â€” o Cloudflare nÃ£o proxia endereÃ§o
   privado, e o proxy quebra a verificaÃ§Ã£o do Vercel.
4. **TTL 60** enquanto algo estÃ¡ sendo verificado; suba depois, se quiser.

## LicenÃ§a

Uso interno INEMA. Sem garantias â€” leia o script antes de rodar (sÃ£o ~120 linhas).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>