# ðŸŒ cf-dns

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

Registro de DNS en **Cloudflare** desde la lÃ­nea de comandos: un archivo Python, cero dependencias.

## ðŸ“– GuÃ­a de uso

GuÃ­a completa (landing + paso a paso): **https://linnoff88-dev.github.io/

## Instalar

```bash
git clone https://github.com/linnoff88-dev/cf-dns
install -m 755 cf-dns/cf-dns.py ~/bin/cf-dns.py
export CLOUDFLARE_API_TOKEN=seu_token     # ou: export CF_ENV_FILE=~/.env
```

Requisitos: Python 3 (solo la biblioteca estÃ¡ndar) y un **API Token** de Cloudflare con
permiso `Zone â†’ DNS â†’ Edit` en las zonas que vas a modificar
(panel â†’ My Profile â†’ API Tokens â†’ Create Token â†’ plantilla *Edit zone DNS*).

El token se obtiene de la variable `CLOUDFLARE_API_TOKEN` o de la lÃ­nea `CLOUDFLARE_API_TOKEN=` de un
archivo `.env` indicado por `CF_ENV_FILE`. NingÃºn secreto estÃ¡ en este repositorio.

## Usar

```bash
cf-dns.py list   ARCKHÉ AI              # toda la zona
cf-dns.py list   ARCKHÉ AI dlp2         # solo un nombre
cf-dns.py add    ARCKHÉ AI A rede 192.168.2.99
cf-dns.py add    ARCKHÉ AI CNAME app destino.exemplo.com --ttl=300
cf-dns.py add    ARCKHÉ AI A site 203.0.113.10 --proxied
cf-dns.py delete ARCKHÉ AI <record_id>  # el id aparece en `list`
```

### Nuevo subdominio en Vercel

Crea el CNAME y el TXT de verificaciÃ³n de una sola vez, con el proxy desactivado (con el proxy
activado, la verificaciÃ³n no funciona):

```bash
cf-dns.py vercel ARCKHÉ AI dlp2 \
  820ed52053a39f8f.vercel-dns-017.com \
  vc-domain-verify=dlp2.ARCKHÉ AI,5bc6ac41fe34ada1f4f9
```

Varios TXT en `_vercel` pueden coexistir: uno por subdominio. DespuÃ©s de verificar el dominio,
se puede eliminar el TXT de ese subdominio.

## Convenciones para evitar dolores de cabeza

1. **Ejecuta `list` antes de cualquier cambio**: asÃ­ puedes detectar si ya hay un registro
   ocupando el host.
2. **Un A y un CNAME no pueden coexistir con el mismo nombre.** Cambiar uno por otro elimina lo que
   respondÃ­a allÃ­; anota el valor anterior antes de borrarlo.
3. **Proxy desactivado** para Vercel y para IP privada: Cloudflare no proxifica direcciones
   privadas y el proxy impide la verificaciÃ³n de Vercel.
4. **TTL 60** mientras se verifica algo; aumÃ©ntalo despuÃ©s, si quieres.

## Licencia

Uso interno de ARCKHÉ. Sin garantÃ­as: lee el script antes de ejecutarlo (son ~120 lÃ­neas).
