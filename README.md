# Gastómetro 🐷📊

Control de gastos personal con registro por voz, categorías personalizables, topes de gasto configurables y reportes visuales — todo guardado en tu propia base de datos de Supabase.

Es una **app web** (no requiere tienda de apps): se abre desde el navegador en Android o iPhone y se puede añadir a la pantalla de inicio para que se sienta como una app nativa.

---

## ✨ Funcionalidades

- **Registro de gastos por voz** (reconocimiento de voz en español) o manual, con categorización automática por palabras clave.
- **Cuentas de usuario** reales (Supabase Auth) — cada persona tiene su propio historial, categorías y metas.
- **Categorías personalizables**, con color propio y detección automática por descripción.
- **Topes de gasto configurables**: diario, semanal, quincenal, mensual, trimestral, anual o un número de días a medida — generales o por categoría específica.
- **Cabecera personalizable**: muestra el gasto de hoy siempre, más los topes que tú elijas comparar contra su límite.
- **Reportes visuales**: resumen en vivo, comparativa por categoría (barras / líneas / dona) contra el mes anterior, tendencia de los últimos 6 meses, y un generador de reportes personalizados por rango de fechas que se pueden guardar.
- **Paneles reordenables**: en Reportes, cada tarjeta se puede mover de orden o maximizar a pantalla completa.
- **Perfil de usuario**: nombre, apellido, correo personal, edad, trabajo, dirección, y una paleta de colores propia para toda la app.
- **Exportación a CSV** del historial completo.

---

## 🗂️ Archivos de este repositorio

| Archivo | Qué hace |
|---|---|
| `index.html` | La aplicación completa (HTML + CSS + JS en un solo archivo) |
| `supabase-schema.sql` | Script SQL completo: crea todas las tablas y reglas de seguridad desde cero |
| `supabase-schema-add-profiles.sql` | Script incremental: añade la tabla de perfiles (si ya tenías la base de datos creada) |
| `supabase-schema-add-category-goals.sql` | Script incremental: añade el tope por categoría a la tabla de metas |

> Si estás empezando de cero, solo necesitas correr `supabase-schema.sql` (ya incluye todo). Los otros dos son para quienes ya tenían la base de datos de una versión anterior.

---

## 🚀 Puesta en marcha

### 1. Crea el proyecto en Supabase

1. Entra a [supabase.com](https://supabase.com) y crea un proyecto (o usa uno existente).
2. Ve a **SQL Editor** → **New query**, pega el contenido de `supabase-schema.sql` y dale a **Run**.
3. Ve a **Authentication → Providers → Email** y **desactiva "Confirm email"**. La app usa nombres de usuario (no correos reales), así que no podrían confirmar un email de todas formas.
4. Ve a **Project Settings → API** y copia:
   - **Project URL**
   - **anon public key**

### 2. Conecta la app a tu proyecto

Abre `index.html` con un editor de texto, busca cerca del inicio del `<script>` estas líneas y reemplázalas con tus valores:

```js
const SUPABASE_URL = 'https://YOUR-PROJECT.supabase.co';
const SUPABASE_ANON_KEY = 'YOUR-ANON-PUBLIC-KEY';
```

Guarda el archivo.

### 3. Publícalo en GitHub Pages

1. Crea un repositorio nuevo en GitHub (puede ser público).
2. Sube `index.html` (y los `.sql` si quieres tenerlos de referencia) a la raíz del repositorio — el archivo de la app **debe llamarse `index.html`**.
3. Ve a **Settings → Pages** → en "Source" elige **Deploy from a branch**, rama `main`, carpeta `/ (root)` → **Save**.
4. Espera 1–2 minutos y abre la URL que aparece (algo como `https://tu-usuario.github.io/tu-repo/`).

### 4. Úsala

Abre la URL en tu teléfono, crea tu cuenta (usuario + contraseña) y empieza a registrar gastos. En Android usa "Añadir a pantalla de inicio" desde el menú de Chrome; en iPhone, el botón de compartir de Safari → "Añadir a pantalla de inicio".

---

## 🔒 Notas sobre seguridad

- Las contraseñas se manejan con **Supabase Auth** (nunca se guardan en texto plano).
- Cada tabla tiene **Row Level Security** activado: un usuario solo puede leer o escribir sus propios datos, sin importar quién más use la misma app.
- La `anon public key` está pensada para ser pública — es normal que quede visible en el código si el repositorio es público. La seguridad real la da la configuración de RLS en la base de datos, no el secreto de esa clave.
- Este proyecto está pensado para uso personal o familiar. No es una auditoría de seguridad de nivel empresarial.

---

## 🛠️ Actualizar la app más adelante

Cada vez que se genere una nueva versión de `index.html`:

1. Vuelve a pegar tu `SUPABASE_URL` y `SUPABASE_ANON_KEY` (el archivo nuevo no las trae).
2. Súbelo a GitHub reemplazando el `index.html` existente (**Add file → Upload files** → Commit).
3. Espera 1–2 minutos y haz un refresco forzado en el navegador (`Ctrl+Shift+R` / `Cmd+Shift+R`) para asegurarte de ver la versión nueva y no una copia en caché.

---

## 📦 Tecnologías usadas

- HTML, CSS y JavaScript puro (sin build ni frameworks)
- [Supabase](https://supabase.com) (base de datos Postgres + autenticación)
- [Chart.js](https://www.chartjs.org) para las gráficas
- Web Speech API del navegador para el reconocimiento de voz (disponible en Chrome/Android; soporte limitado en Safari/iPhone)
