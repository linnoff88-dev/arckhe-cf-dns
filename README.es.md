# ê°Å¸ÅÂ cf-dns

**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

Registro de DNS en **Cloudflare** desde la lêÂ­nea de comandos: un archivo Python, cero dependencias.

## ê°Å¸ââ GuêÂ­a de uso

GuêÂ­a completa (landing + paso a paso): **https://linnoff88-dev.github.io/

## Instalar

```bash
git clone https://github.com/linnoff88-dev/cf-dns
install -m 755 cf-dns/cf-dns.py ~/bin/cf-dns.py
export CLOUDFLARE_API_TOKEN=seu_token     # ou: export CF_ENV_FILE=~/.env
```

Requisitos: Python 3 (solo la biblioteca estêÂ¡ndar) y un **API Token** de Cloudflare con
permiso `Zone ê¢â â DNS ê¢â â Edit` en las zonas que vas a modificar
(panel ê¢â â My Profile ê¢â â API Tokens ê¢â â Create Token ê¢â â plantilla *Edit zone DNS*).

El token se obtiene de la variable `CLOUDFLARE_API_TOKEN` o de la lêÂ­nea `CLOUDFLARE_API_TOKEN=` de un
archivo `.env` indicado por `CF_ENV_FILE`. NingêÂºn secreto estêÂ¡ en este repositorio.

## Usar

```bash
cf-dns.py list   ARCKHê AI              # toda la zona
cf-dns.py list   ARCKHê AI dlp2         # solo un nombre
cf-dns.py add    ARCKHê AI A rede 192.168.2.99
cf-dns.py add    ARCKHê AI CNAME app destino.exemplo.com --ttl=300
cf-dns.py add    ARCKHê AI A site 203.0.113.10 --proxied
cf-dns.py delete ARCKHê AI <record_id>  # el id aparece en `list`
```

### Nuevo subdominio en Vercel

Crea el CNAME y el TXT de verificaciêÂ³n de una sola vez, con el proxy desactivado (con el proxy
activado, la verificaciêÂ³n no funciona):

```bash
cf-dns.py vercel ARCKHê AI dlp2 \
  820ed52053a39f8f.vercel-dns-017.com \
  vc-domain-verify=dlp2.ARCKHê AI,5bc6ac41fe34ada1f4f9
```

Varios TXT en `_vercel` pueden coexistir: uno por subdominio. DespuêÂ©s de verificar el dominio,
se puede eliminar el TXT de ese subdominio.

## Convenciones para evitar dolores de cabeza

1. **Ejecuta `list` antes de cualquier cambio**: asêÂ­ puedes detectar si ya hay un registro
   ocupando el host.
2. **Un A y un CNAME no pueden coexistir con el mismo nombre.** Cambiar uno por otro elimina lo que
   respondêÂ­a allêÂ­; anota el valor anterior antes de borrarlo.
3. **Proxy desactivado** para Vercel y para IP privada: Cloudflare no proxifica direcciones
   privadas y el proxy impide la verificaciêÂ³n de Vercel.
4. **TTL 60** mientras se verifica algo; aumêÂ©ntalo despuêÂ©s, si quieres.

## Licencia

Uso interno de ARCKHê. Sin garantêÂ­as: lee el script antes de ejecutarlo (son ~120 lêÂ­neas).
