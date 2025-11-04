# Guía para Subir Plugin a WordPress.org

## 📋 Requisitos Previos

### 1. Cuenta en WordPress.org
- ✅ Necesitas una cuenta en https://wordpress.org/support/register.php
- ✅ Verifica tu email

### 2. Preparación del Plugin

#### ✅ Checklist Completado:
- ✅ `readme.txt` en formato WordPress.org
- ✅ Código cumple WordPress Coding Standards
- ✅ Licencia GPLv2 or later
- ✅ Text domain correcto: `wc-product-scheduler`
- ✅ Sin código ofuscado o malicioso
- ✅ Sin enlaces de afiliados
- ✅ Sin telemetría no autorizada
- ✅ Seguridad: sanitización, escaping, nonces

#### ⚠️ Ajustes Necesarios:

**1. Contributors en readme.txt**
Editar línea 2 de `readme.txt`:
```
Contributors: TU_USERNAME_WORDPRESS_ORG
```
Reemplaza con tu username real de WordPress.org

**2. Author URI (opcional)**
En `wc-product-scheduler.php` línea 7:
```php
* Author URI: https://dismaconsultores.com
```
Puedes cambiarlo a tu perfil de WordPress.org o dejarlo así.

---

## 🚀 Proceso de Envío

### Paso 1: Solicitar Hosting en WordPress.org

1. Ve a: https://wordpress.org/plugins/developers/add/
2. Completa el formulario:
   - **Plugin Name:** WooCommerce Product Scheduler
   - **Plugin Description:** Programa la publicación y despublicación automática de productos de WooCommerce
   - **Plugin URL:** https://github.com/adriaurora/WooCommerce-Product-Scheduler

3. Marca las casillas:
   - ✅ El plugin cumple con las directrices
   - ✅ El código es 100% compatible con GPL
   - ✅ El plugin no tiene contenido ilegal

4. Envía el formulario

### Paso 2: Esperar Revisión

- ⏰ **Tiempo de espera:** 1-14 días (normalmente 2-5 días)
- 📧 Recibirás un email con:
  - Aprobación y URL del SVN, o
  - Lista de cambios necesarios

### Paso 3: Subir al SVN (Cuando sea aprobado)

Una vez aprobado, recibirás acceso a un repositorio SVN:

```bash
# URL ejemplo (recibirás la tuya real):
https://plugins.svn.wordpress.org/wc-product-scheduler/
```

#### Estructura SVN Requerida:

```
wc-product-scheduler/
├── trunk/              (versión en desarrollo)
│   ├── wc-product-scheduler.php
│   ├── readme.txt
│   ├── includes/
│   ├── assets/
│   └── ...
├── tags/               (versiones publicadas)
│   └── 1.4.0/
│       ├── (mismos archivos que trunk)
└── assets/             (screenshots e iconos)
    ├── screenshot-1.png
    ├── screenshot-2.png
    ├── icon-128x128.png
    └── icon-256x256.png
```

---

## 📦 Comandos para Subir al SVN

### Checkout inicial (cuando recibas aprobación):

```bash
# Hacer checkout del SVN
svn co https://plugins.svn.wordpress.org/wc-product-scheduler wc-product-scheduler-svn
cd wc-product-scheduler-svn
```

### Subir archivos a trunk:

```bash
# Copiar archivos del plugin a trunk/
cp -r "/Users/adrianlaborda/Downloads/Plugin programación productos/"* trunk/

# Pero EXCLUIR archivos que NO deben ir a WordPress.org:
cd trunk/
rm -rf .git .gitignore
rm -rf AUDIT-FINAL.md
rm -rf CHANGELOG-v1.3.0.md
rm -rf CHANGELOG-v1.4.0.md
rm -rf COMO-FUNCIONA-EL-CRON.md
rm -rf OPTIMIZACIONES-RENDIMIENTO.md
rm -rf README.md  # WordPress.org usa readme.txt
rm -rf SUBIR-A-WORDPRESS-ORG.md

# Verificar qué archivos quedan
ls -la

# Deberías tener solo:
# - wc-product-scheduler.php
# - readme.txt
# - includes/
# - assets/
```

### Añadir archivos nuevos al SVN:

```bash
# Desde la carpeta wc-product-scheduler-svn/
svn add trunk/* --force
svn status  # Ver qué se va a subir
```

### Commit a trunk:

```bash
svn ci -m "Initial commit of WooCommerce Product Scheduler v1.4.0"
# Te pedirá tu username y password de WordPress.org
```

### Crear tag de versión 1.4.0:

```bash
svn cp trunk tags/1.4.0
svn ci -m "Tagging version 1.4.0"
```

---

## 🎨 Assets Opcionales (Screenshots e Iconos)

Para que tu plugin se vea profesional en WordPress.org:

### Screenshots
Crea capturas de pantalla y nómbralas:
- `screenshot-1.png` - Pestaña de programación en producto
- `screenshot-2.png` - Switches y datepickers
- `screenshot-3.png` - Columna en listado de productos
- `screenshot-4.png` - Email de notificación

Tamaño recomendado: 1280x720px

### Iconos
- `icon-128x128.png` - Icono pequeño
- `icon-256x256.png` - Icono grande

Colócalos en `assets/` (en la raíz del SVN, no en trunk):

```bash
# Crear carpeta assets en raíz SVN
mkdir -p assets
cp screenshot-*.png assets/
cp icon-*.png assets/

svn add assets/* --force
svn ci -m "Add plugin assets (screenshots and icons)"
```

---

## ✅ Checklist Pre-Envío

Antes de enviar tu solicitud a WordPress.org, verifica:

- [ ] Tienes cuenta en WordPress.org
- [ ] El plugin no tiene errores PHP
- [ ] `readme.txt` tiene tu username correcto en Contributors
- [ ] Licencia es GPLv2 or later
- [ ] No hay código ofuscado
- [ ] No hay enlaces de afiliados
- [ ] GitHub repo es público: https://github.com/adriaurora/WooCommerce-Product-Scheduler
- [ ] Has probado el plugin en una instalación limpia de WordPress

---

## 📚 Documentación Oficial

- **Plugin Developer Handbook:** https://developer.wordpress.org/plugins/
- **Plugin Guidelines:** https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/
- **SVN Guide:** https://developer.wordpress.org/plugins/wordpress-org/how-to-use-subversion/
- **readme.txt Validator:** https://wordpress.org/plugins/developers/readme-validator/

---

## ⚠️ Errores Comunes a Evitar

1. **No usar funciones prefijadas:** Todas las funciones tienen prefijo `wc_product_scheduler_` ✅
2. **No escapar output:** Todo escapado con `esc_html()`, `esc_attr()` ✅
3. **No sanitizar input:** Todo sanitizado con `sanitize_text_field()` ✅
4. **Usar `eval()` o `base64_decode()`:** No usamos ninguno ✅
5. **Código sin licencia:** Tenemos GPLv2 ✅
6. **Llamadas externas sin permiso:** No hacemos ninguna ✅

---

## 🎯 Después de la Aprobación

Una vez aprobado y subido:

1. **WordPress.org generará automáticamente:**
   - Página del plugin: `https://wordpress.org/plugins/wc-product-scheduler/`
   - ZIP de descarga
   - Búsqueda en el directorio
   - Capacidad de instalación desde admin de WordPress

2. **Para actualizar en el futuro:**
   ```bash
   # Actualizar trunk
   svn up
   # Hacer cambios en trunk/
   svn ci -m "Update to version 1.5.0"
   # Crear nuevo tag
   svn cp trunk tags/1.5.0
   svn ci -m "Tagging version 1.5.0"
   ```

3. **Cambiar Stable tag en readme.txt:**
   Cada vez que hagas una nueva versión, actualiza en `readme.txt`:
   ```
   Stable tag: 1.5.0
   ```

---

## 📞 Soporte

Si tienes problemas:
- **Forum:** https://wordpress.org/support/forum/plugins-and-hacks/
- **Slack:** https://make.wordpress.org/chat/

---

## 🎉 ¡Listo!

Tu plugin está preparado para WordPress.org. Solo necesitas:
1. Ajustar el contributor en readme.txt
2. Enviar solicitud en wordpress.org/plugins/developers/add/
3. Esperar aprobación
4. Subir al SVN cuando sea aprobado
