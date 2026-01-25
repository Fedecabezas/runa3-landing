# Runa3 Landing Page

Landing page estática con health check dinámico al servidor backend.

🔗 **Repo**: https://github.com/Fedecabezas/runa3-landing  
🌐 **Live**: Configurar en Cloudflare Pages

---

## ✨ Características

- **Health Check Automático**: Verifica estado del servidor al cargar
- **Botón Dinámico**: Se habilita/deshabilita según disponibilidad del server
- **Auto-retry**: Si el servidor está offline, reintenta cada 30 segundos
- **Información de Versión**: Muestra versión y build del servidor cuando está online
- **PWA Ready**: Service Worker incluido para funcionamiento offline

---

## 🚀 Deploy con Cloudflare Pages

### Opción 1: GitHub Integration (Recomendado)

1. **Cloudflare Dashboard** → Pages → Create project
2. **Connect to Git** → Seleccionar `Fedecabezas/runa3-landing`
3. **Build settings**:
   - Framework preset: `None`
   - Build command: (dejar vacío)
   - Build output directory: `/`
4. **Deploy**
5. **Custom domain**: `app.embrague.xyz`

### Opción 2: Wrangler CLI

```bash
# Instalar Wrangler
npm install -g wrangler

# Login
wrangler login

# Deploy
wrangler pages deploy . --project-name=runa3-landing

# Configurar custom domain en dashboard
```

---

## 🔧 Configuración

Los endpoints están hardcodeados en `index.html`:

```javascript
const APP_URL = 'https://app.runa3.fit';        // URL de la PWA
const HEALTH_URL = 'https://api.runa3.fit/health'; // Backend health check
```

### Para desarrollo local:

```javascript
const APP_URL = 'http://localhost:5173';
const HEALTH_URL = 'http://localhost:3000/health';
```

---

## 📡 Health Check Endpoint

**Backend**: `GET /health` (sin `/api/v1` prefix)

**Response**:
```json
{
  "status": "ok",
  "timestamp": "2026-01-25T01:30:00.000Z",
  "service": "runa3-server",
  "version": "1.0.0",
  "buildDate": "2026-01-24T22:15:30.000Z",
  "buildNumber": 245,
  "minClientVersion": "2.5.1",
  "forceLogout": false
}
```

---

## 🎯 Comportamiento

1. **Al cargar**: Botón muestra "Verificando servidor..." (disabled)
2. **Server Online**: 
   - ✓ Botón habilitado → "Acceder a la app"
   - Muestra: `✓ Servidor online | v1.0.0 #245` (verde)
3. **Server Offline**:
   - ⚠ Botón disabled → "Servidor no disponible"
   - Muestra: `⚠ El servidor está temporalmente offline` (amarillo)
   - Auto-retry cada 30 segundos

---

## 📦 Archivos

```
Landing/
├── index.html          # Landing page principal
├── manifest.json       # PWA manifest
├── sw.js              # Service Worker
└── README.md          # Este archivo
```

---

## 🔐 CORS

El backend debe permitir requests desde el dominio de la landing:

```typescript
// runa3-server/src/main.ts
app.enableCors({
  origin: process.env.CORS_ORIGIN || '*',
  credentials: true,
});
```

Para producción, configurar `CORS_ORIGIN`:
```env
CORS_ORIGIN=https://app.embrague.xyz
```

---

## 📝 Actualizar

```bash
# Editar archivos localmente
git add .
git commit -m "feat: nueva funcionalidad"
git push

# Cloudflare Pages auto-deploya en cada push
```

---

## 🚨 Notas

- El health check tiene timeout de **5 segundos**
- Si falla, no bloquea la página (graceful degradation)
- El Service Worker cachea la landing para uso offline
- No requiere backend para funcionar (solo deshabilita botón)

---

**Developed by**: EMBRAGUE CORP © 2026
