# 📺 ErsatzTV en Docker — Ubuntu VPS

Despliegue de [ErsatzTV](https://ersatztv.org) con Docker Compose en un VPS Ubuntu, con soporte para **Filler Presets** (comerciales cada 15 minutos).

---

## 🚀 Instalación rápida

```bash
# 1. Clona el repositorio
git clone https://github.com/luisitoys12/ersatztv-docker.git
cd ersatztv-docker

# 2. Edita las rutas de tus medios en docker-compose.yml
nano docker-compose.yml

# 3. Crea el directorio de configuración
mkdir -p config

# 4. Levanta el contenedor
docker compose up -d
```

Accede al panel en: `http://TU_IP_VPS:8409`

---

## 📁 Estructura de volúmenes

| Volumen | Ruta en contenedor | Descripción |
|---|---|---|
| `./config` | `/config` | Base de datos y configuración persistente |
| `/ruta/a/tus/medios/tv` | `/tv` | Contenido principal (series, películas) |
| `/ruta/a/tus/comerciales` | `/comerciales` | Spots/publicidad para Filler Presets |
| tmpfs | `/transcode` | Transcodificación temporal en RAM |

> ⚠️ Las rutas de medios usan `:ro` (read-only) para proteger tus archivos.

---

## 📡 URLs de integración (Plex / Jellyfin)

| Recurso | URL |
|---|---|
| Lista M3U | `http://TU_IP:8409/iptv.m3u` |
| Guía EPG (XMLTV) | `http://TU_IP:8409/xmltv.xml` |

---

## 🎬 Configurar comerciales cada 15 minutos (Filler Presets)

1. **Media Sources → Local** — agrega `/comerciales` como biblioteca tipo *Other Video*
2. **Lists → Filler Presets → Add** — crea un preset (ej: `Comerciales_15min`)
3. Agrega tus spots a la colección del preset
4. En **modo "Pad"** selecciona `15 minutos`
5. **Channels → tu canal → Edit** — asigna el preset como `Mid-roll` o `Tail`
6. **Schedules → Playouts** — verifica que los bloques terminan en `:00`, `:15`, `:30`, `:45`

### Tipos de Filler disponibles

| Tipo | Cuándo se reproduce |
|---|---|
| `Pre-roll` | Antes de cada programa |
| `Mid-roll` | Entre capítulos del mismo programa |
| `Post-roll` | Después de cada programa |
| `Tail` | Al final de un bloque, antes del siguiente ítem |
| `Fallback` | Para rellenar espacios vacíos en la programación |

---

## ⚠️ Notas importantes

- Se usa la imagen **legacy** (`ghcr.io/ersatztv/legacy`) ya que la versión *Next* (en Rust) aún no tiene paridad completa de funciones de programación.
- El modo Pad **no corta programas en vivo** cada 15 min; rellena espacios para que el bloque termine en un múltiplo de 15.
- Si cambias `PUID`/`PGID`, asegúrate que el usuario tiene permisos sobre `./config`.

---

## 🔄 Comandos útiles

```bash
# Ver logs en tiempo real
docker compose logs -f ersatztv

# Reiniciar el servicio
docker compose restart ersatztv

# Actualizar a la última imagen
docker compose pull && docker compose up -d

# Detener sin borrar datos
docker compose down
```

---

Hecho con ❤️ por [Estacionkusmedios](https://estacionkusmedios.org) — Irapuato, Guanajuato, MX
