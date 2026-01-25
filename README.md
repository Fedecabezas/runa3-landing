# Runa3 Landing Page

Static landing page with dynamic backend health monitoring.

---

## Technical Overview

Self-contained static site with integrated server health verification. Implements automatic endpoint availability checking and graceful degradation when backend services are unavailable.

**Stack**: Vanilla JS, PWA-enabled with Service Worker

---

## Configuration

## Configuration

Backend endpoints are defined in `index.html`:

```javascript
const APP_URL = 'https://app.runa3.fit';
const HEALTH_URL = 'https://api.runa3.fit/health';
```

---

## Health Check Protocol

## Health Check Protocol

**Endpoint**: `GET /health`

**Response Schema**:
```json
{
  "status": "ok",
  "timestamp": "2026-01-25T01:30:00.000Z",
  "service": "runa3-server",
  "version": "1.0.0",
  "buildDate": "2026-01-24T22:15:30.000Z",
  "buildNumber": 245
}
```

**Client Behavior**:
- Request timeout: 5 seconds
- Retry interval: 30 seconds (on failure)
- UI state transitions: `loading` → `available` | `unavailable`

---

## Architecture

## Architecture

```
Landing/
├── index.html          # Main entry point
├── manifest.json       # PWA configuration
└── sw.js              # Service Worker for offline capabilities
```

**Dependencies**: None (zero build process)

---

## CORS Requirements

Backend must allow cross-origin requests:

```typescript
app.enableCors({
  origin: process.env.CORS_ORIGIN || '*',
  credentials: true,
});
```

---

EMBRAGUE CORP © 2026
