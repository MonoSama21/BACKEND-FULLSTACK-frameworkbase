# 🚀 Guía de Despliegue en Vercel - Backend Boda Diter y Vivian

## 📋 Ventajas de Vercel vs Render

- ✅ **Serverless**: No necesitas keep-alive, escala automáticamente
- ✅ **Más rápido**: Cold start de ~1 segundo vs 50 segundos de Render
- ✅ **Gratis ilimitado**: Sin límite de horas
- ✅ **Edge Network**: CDN global automático
- ✅ **Ambientes automáticos**: Preview, Development, Production

---

## 🏗️ Configuración del Proyecto

### Paso 1: Preparar el proyecto

Ya está configurado con:
- ✅ `vercel.json` - Configuración de Vercel
- ✅ `.vercelignore` - Archivos a ignorar
- ✅ `.gitignore` actualizado

### Paso 2: Instalar Vercel CLI (opcional para deploy desde terminal)

```bash
npm install -g vercel
```

---

## 🚀 MÉTODO 1: Deploy desde Dashboard (RECOMENDADO)

### 1️⃣ Preparar GitHub

```bash
# Asegúrate de tener todo commiteado
git add .
git commit -m "config: preparar proyecto para Vercel"
git push origin main
git push origin develop
```

### 2️⃣ Conectar Vercel

1. **Ve a https://vercel.com** y regístrate con GitHub
2. **Click en "Add New..." → "Project"**
3. **Importa tu repositorio** desde GitHub
4. **Configura el proyecto:**

   **Framework Preset**: `Other`
   
   **Root Directory**: `./` (dejar por defecto)
   
   **Build Command**: `npm run build`
   
   **Output Directory**: `dist`
   
   **Install Command**: `npm install`

### 3️⃣ Configurar Variables de Entorno para PRODUCCIÓN

En el dashboard de Vercel, antes de hacer deploy:

**Environment Variables** (para producción):
```
NODE_ENV=production
SUPABASE_URL=tu_supabase_url_produccion
SUPABASE_KEY=tu_supabase_key_produccion
JWT_SECRET=tu_secret_jwt_produccion
ADMIN_USERNAME=diter-vivian
ADMIN_PASSWORD=BodaDyV2026!
EMAIL_USER=tu_email@gmail.com
EMAIL_PASSWORD=tu_app_password
EMAIL_DESTINO=diter.vivian@example.com
```

**IMPORTANTE**: Selecciona **"Production"** en el selector de ambiente.

### 4️⃣ Deploy Inicial

Click en **"Deploy"** y espera ~2 minutos.

Tu API estará disponible en: `https://tu-proyecto.vercel.app`

---

## 🟢 Configurar Ambiente de DESARROLLO

### 1️⃣ Ir a Settings del proyecto

En el dashboard de Vercel → Tu proyecto → **Settings** → **Git**

### 2️⃣ Configurar rama de desarrollo

En **"Git Integration"**:
- ✅ **Production Branch**: `main`
- ✅ **Preview Branches**: Habilitar `develop`

### 3️⃣ Agregar variables de entorno para DESARROLLO

**Settings** → **Environment Variables**:

Agrega las mismas variables pero seleccionando **"Preview"** (desarrollo):

```
NODE_ENV=development
SUPABASE_URL=tu_supabase_url_desarrollo
SUPABASE_KEY=tu_supabase_key_desarrollo
JWT_SECRET=tu_secret_jwt_desarrollo
ADMIN_USERNAME=diter-vivian
ADMIN_PASSWORD=BodaDyV2026!
EMAIL_USER=tu_email@gmail.com
EMAIL_PASSWORD=tu_app_password
EMAIL_DESTINO=diter.vivian@example.com
```

### 4️⃣ Hacer push a develop

```bash
git checkout develop
git push origin develop
```

Vercel automáticamente creará un deploy de preview en:
`https://tu-proyecto-git-develop-tu-usuario.vercel.app`

---

## 🚀 MÉTODO 2: Deploy desde Terminal

```bash
# Login
vercel login

# Deploy a producción
vercel --prod

# Deploy a desarrollo (preview)
vercel
```

---

## 🔄 Workflow de Desarrollo con Vercel

### Desarrollo:
```bash
git checkout develop
# Hacer cambios
git add .
git commit -m "feat: nueva funcionalidad"
git push origin develop
# Vercel desplegará automáticamente a preview
```

### Producción:
```bash
git checkout main
git merge develop
git push origin main
# Vercel desplegará automáticamente a producción
```

---

## 📊 URLs de tus Ambientes

Después del deploy:

- **Producción (main)**: `https://tu-proyecto.vercel.app`
- **Desarrollo (develop)**: `https://tu-proyecto-git-develop-username.vercel.app`
- **Preview (cualquier branch)**: `https://tu-proyecto-git-branch-username.vercel.app`

---

## ⚡ Diferencias importantes vs Render

### ✅ NO necesitas keep-alive

Vercel usa **serverless functions** que:
- Se activan solo cuando hay requests
- No se "duermen" (simplemente escalan a 0)
- Cold start de ~1 segundo vs 50 segundos

**Puedes eliminar:**
- ❌ `services/keepAliveService.ts` (ya no necesario)
- ❌ El ping interno en `index.ts`
- ❌ Cron jobs en Supabase para keep-alive

### ⚠️ Límites del plan FREE

- ✅ **Invocaciones**: 100 GB-horas gratis/mes
- ✅ **Bandwidth**: 100 GB/mes
- ✅ **Ejecución**: 10 segundos máximo por request
- ✅ **Sin límite de proyectos**

---

## 🔧 Configuración de Dominio Personalizado (Opcional)

1. **Settings** → **Domains**
2. **Add Domain**: `api.boda-diter-vivian.com`
3. Configura los DNS según las instrucciones de Vercel

---

## 🧪 Probar los Ambientes

### Producción:
```bash
curl https://tu-proyecto.vercel.app/health
```

### Desarrollo:
```bash
curl https://tu-proyecto-git-develop-username.vercel.app/health
```

---

## 📝 Checklist de Migración a Vercel

- [ ] Proyecto en GitHub (main y develop)
- [ ] Cuenta en Vercel creada
- [ ] Proyecto importado en Vercel
- [ ] Variables de entorno configuradas para producción
- [ ] Variables de entorno configuradas para preview/desarrollo
- [ ] Deploy de producción exitoso
- [ ] Deploy de desarrollo exitoso
- [ ] Pruebas en ambos ambientes
- [ ] (Opcional) Eliminar keep-alive code si no lo necesitas
- [ ] (Opcional) Eliminar servicios de Render

---

## 🆘 Solución de Problemas

### Error: "Module not found"
**Solución**: Verifica que `npm run build` funcione localmente

### Error: "Function timeout"
**Solución**: Vercel FREE tiene límite de 10s por request. Optimiza consultas pesadas.

### Error 500 en producción
**Solución**: Revisa los logs en Vercel Dashboard → Tu proyecto → Deployments → Click en el deploy → Functions

---

## 💡 Consejos Pro

1. **Preview Deployments**: Cada branch tiene su propia URL de preview
2. **Rollback instantáneo**: Vercel guarda todos los deploys, puedes volver a cualquiera
3. **Logs en tiempo real**: Dashboard → Functions → Ver logs de cada request
4. **Analytics**: Vercel Analytics te da métricas de uso gratis
5. **No necesitas keep-alive**: Serverless escala automáticamente

---

## 🎯 Próximos Pasos

1. Deploy inicial en Vercel
2. Probar ambos ambientes
3. (Opcional) Limpiar código de keep-alive
4. (Opcional) Configurar dominio personalizado
5. (Opcional) Habilitar Vercel Analytics

---

## ✅ ¡Listo!

Tu backend ahora está en Vercel con:
- 🔵 **Producción**: rama `main` → URL principal
- 🟢 **Desarrollo**: rama `develop` → URL de preview
- ⚡ **Serverless**: Sin cold starts largos
- 🆓 **Gratis**: Sin límite de horas

¡Tu API está lista para escalar! 🎉
