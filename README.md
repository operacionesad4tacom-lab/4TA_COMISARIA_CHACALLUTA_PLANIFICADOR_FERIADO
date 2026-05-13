# 🟢 Sistema de Feriados — 4ª Comisaría Carabineros Chacalluta (F)

Sistema web para gestión de feriados de invierno, basado en el modelo de reserva de vuelos.

---

## 📁 Estructura del proyecto

```
feriados-chacalluta/
├── index.html          ← Login funcionarios
├── dashboard.html      ← Calendario y solicitud (funcionario)
├── admin-login.html    ← Login administrador
├── admin.html          ← Panel completo administrador
├── css/
│   └── style.css       ← Estilos institucionales Carabineros
├── js/
│   └── supabase-config.js  ← ⚠️ Configurar con tus credenciales
├── sql/
│   └── 01_setup_completo.sql  ← Script completo de base de datos
├── assets/
│   └── escudo.png      ← Copiar el escudo aquí
└── README.md
```

---

## 🚀 Instalación paso a paso

### PASO 1 — Crear proyecto en Supabase

1. Ir a https://supabase.com y crear cuenta (gratuita)
2. Crear nuevo proyecto → nombre: `feriados-chacalluta`
3. Guardar la contraseña de la base de datos
4. Esperar que el proyecto se inicialice (~2 min)

### PASO 2 — Ejecutar el SQL

1. En Supabase → menú izquierdo → **SQL Editor**
2. Hacer clic en **New query**
3. Abrir el archivo `sql/01_setup_completo.sql`
4. Copiar TODO el contenido y pegarlo en el editor
5. Hacer clic en **Run** (▶️)
6. Verificar que diga "Success. No rows returned"

Esto creará:
- ✅ Tabla `funcionarios` con los 161 funcionarios cargados
- ✅ Tabla `solicitudes`
- ✅ Tabla `ventanas_grado`
- ✅ Tabla `fechas_bloqueadas`
- ✅ Tabla `configuracion`
- ✅ Tabla `admins` (usuario: admin / clave: Chacalluta2024!)

### PASO 3 — Configurar credenciales

1. En Supabase → **Settings** → **API**
2. Copiar:
   - **Project URL** → `https://xxxx.supabase.co`
   - **anon public** key
3. Abrir `js/supabase-config.js`
4. Reemplazar los valores:

```javascript
const SUPABASE_URL = 'https://TU_PROYECTO.supabase.co';  // ← tu URL
const SUPABASE_ANON_KEY = 'TU_ANON_KEY_AQUI';           // ← tu clave
```

### PASO 4 — Agregar el escudo

1. Copiar el archivo `escudo-carabineros-transparente.png` a la carpeta `assets/`
2. Renombrarlo a `escudo.png`

### PASO 5 — Publicar en GitHub Pages

1. Crear repositorio en GitHub (puede ser privado o público)
2. Subir todos los archivos
3. Ir al repositorio → **Settings** → **Pages**
4. En "Source" seleccionar **Deploy from a branch**
5. Rama: `main`, carpeta: `/ (root)`
6. Guardar → en ~2 min el sitio estará en:
   `https://TU_USUARIO.github.io/feriados-chacalluta/`

### PASO 6 — Configurar Row Level Security en Supabase

En Supabase → **Authentication** → **Policies**, para que el sistema funcione sin autenticación nativa de Supabase, las tablas deben tener RLS desactivado o con políticas permisivas:

```sql
-- Ejecutar en SQL Editor si hay errores de permisos:
ALTER TABLE funcionarios DISABLE ROW LEVEL SECURITY;
ALTER TABLE solicitudes DISABLE ROW LEVEL SECURITY;
ALTER TABLE ventanas_grado DISABLE ROW LEVEL SECURITY;
ALTER TABLE fechas_bloqueadas DISABLE ROW LEVEL SECURITY;
ALTER TABLE configuracion DISABLE ROW LEVEL SECURITY;
ALTER TABLE admins DISABLE ROW LEVEL SECURITY;
```

---

## 🔐 Credenciales

### Funcionarios
- **Usuario:** Código de dotación (ej: `007844T`)
- **Clave:** Últimos 4 dígitos del RUT, SIN dígito verificador
  - Ej: RUT `19.869.217-4` → clave `9217`

### Administrador (cambiar en producción)
- **Usuario:** `admin`
- **Clave:** `Chacalluta2024!`

Para cambiar la clave admin, ejecutar en SQL Editor:
```sql
UPDATE admins SET clave = 'NUEVA_CLAVE' WHERE usuario = 'admin';
```

---

## 📋 Reglas del sistema

| Regla | Valor |
|---|---|
| Límite funcionarios con feriado simultáneo | 15 |
| Días hábiles elegibles | 5 a 15 |
| Días extraordinarios (General Director) | +4 corridos pegados al final |
| Los 4 días cuentan en el límite diario | ❌ No |
| Modificaciones permitidas (antes de aprobación) | 2 |
| Modificación post-aprobación | ❌ No permitida |
| Cancelación | ✅ Libera cupo automáticamente |

---

## 🎯 Flujo del sistema

```
Funcionario hace login
    ↓
Verifica ventana de ingreso por grado
    ↓
Selecciona días hábiles (5 a 15)
    ↓
Elige fecha de inicio en calendario tipo vuelo
    ↓
Sistema calcula fin hábiles + 4 días extraordinarios
    ↓
Confirma → Estado: PENDIENTE
    ↓
Admin aprueba o rechaza desde panel
    ↓
Estado: APROBADO (no modificable)
```

---

## 🛠️ Funciones del administrador

- ✅ Aprobar / rechazar solicitudes con observación
- 📊 Dashboard con estadísticas y mapa de calor
- 🕐 Configurar ventanas de ingreso por grado (fecha + hora)
- 🚫 Bloquear fechas específicas
- ⚙️ Configurar límite diario y máximo de modificaciones
- 📤 Exportar solicitudes, aprobados y nómina en CSV

---

## 🏗️ Stack tecnológico

- **Frontend:** HTML5 + CSS3 + JavaScript puro (sin frameworks)
- **Base de datos:** Supabase (PostgreSQL)
- **Hosting:** GitHub Pages (gratuito)
- **Fuentes:** Google Fonts (Barlow Condensed + Barlow)

---

*4ª Comisaría de Carabineros Chacalluta (F) — Sistema desarrollado 2024*
