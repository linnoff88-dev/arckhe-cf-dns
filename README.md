# ê°Å¸ÅÂ cf-dns

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

Registro de DNS no **Cloudflare** pela linha de comando ê¢â¬â um arquivo Python, zero dependêÂªncias.

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

## Instalar

```bash
git clone https://github.com/linnoff88-dev/cf-dns
install -m 755 cf-dns/cf-dns.py ~/bin/cf-dns.py
export CLOUDFLARE_API_TOKEN=seu_token     # ou: export CF_ENV_FILE=~/.env
```

Requisitos: Python 3 (sêÂ³ a biblioteca padrêÂ£o) e um **API Token** do Cloudflare com
permissêÂ£o `Zone ê¢â â DNS ê¢â â Edit` nas zonas que vocêÂª vai alterar
(painel ê¢â â My Profile ê¢â â API Tokens ê¢â â Create Token ê¢â â template *Edit zone DNS*).

O token vem da variêÂ¡vel `CLOUDFLARE_API_TOKEN` ou da linha `CLOUDFLARE_API_TOKEN=` de um
arquivo `.env` apontado por `CF_ENV_FILE`. Nenhum segredo mora neste repositêÂ³rio.

## Usar

```bash
cf-dns.py list   ARCKHê AI              # a zona inteira
cf-dns.py list   ARCKHê AI dlp2         # sêÂ³ um nome
cf-dns.py add    ARCKHê AI A rede 192.168.2.99
cf-dns.py add    ARCKHê AI CNAME app destino.exemplo.com --ttl=300
cf-dns.py add    ARCKHê AI A site 203.0.113.10 --proxied
cf-dns.py delete ARCKHê AI <record_id>  # o id sai no `list`
```

### SubdomêÂ­nio novo no Vercel

Cria o CNAME e o TXT de verificaêÂ§êÂ£o de uma vez, com o proxy desligado (com o proxy
ligado a verificaêÂ§êÂ£o nêÂ£o passa):

```bash
cf-dns.py vercel ARCKHê AI dlp2 \
  820ed52053a39f8f.vercel-dns-017.com \
  vc-domain-verify=dlp2.ARCKHê AI,5bc6ac41fe34ada1f4f9
```

VêÂ¡rios TXT em `_vercel` convivem ê¢â¬â um por subdomêÂ­nio. Depois que o domêÂ­nio fica
verificado, o TXT daquele subdomêÂ­nio pode ser removido.

## ConvenêÂ§êÂµes que evitam dor de cabeêÂ§a

1. **Rode `list` antes de qualquer alteraêÂ§êÂ£o** ê¢â¬â êÂ© assim que se descobre um registro jêÂ¡
   ocupando o host.
2. **Um A e um CNAME nêÂ£o coexistem no mesmo nome.** Trocar um pelo outro derruba o que
   estava respondendo ali; anote o valor antigo antes de apagar.
3. **Proxy desligado** para Vercel e para IP privado ê¢â¬â o Cloudflare nêÂ£o proxia endereêÂ§o
   privado, e o proxy quebra a verificaêÂ§êÂ£o do Vercel.
4. **TTL 60** enquanto algo estêÂ¡ sendo verificado; suba depois, se quiser.

## LicenêÂ§a

Uso interno ARCKHê. Sem garantias ê¢â¬â leia o script antes de rodar (sêÂ£o ~120 linhas).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>