# Architecture: Sovereign Wazuh SIEM

## Overview

**Package ID:** `PKG-028`  
**Domain:** Cybersecurity & SIEM Compliance  
**Microservice Port:** `8808`  
**n8n Webhook Path:** `wazuh-siem-trigger`  
**GitHub:** [BlackFoxgamingstudio/wazuh-siem](https://github.com/BlackFoxgamingstudio/wazuh-siem)

Wazuh SIEM integration layer with real-time alert correlation, threat intelligence enrichment, compliance reporting (SOC2/HIPAA/PCI), and automated incident response.

---

## System Architecture

```
                     ┌──────────────────────────────────┐
                     │       Sovereign Wazuh SIEM          │
                     │       Port: 8808            │
                     ├──────────────┬───────────────────┤
   n8n Webhook ────▶ │  REST API    │   Core Engine     │
   HTTP POST         │  /api/v1/*   │   Dispatcher      │
                     └──────┬───────┴────────┬──────────┘
                            │                │
              ┌─────────────▼────────────────▼─────────┐
              │          Component Layer                 │
              │  AlertCorrelator | ThreatEnricher  | ComplianceRe  │
              └────────────────────────┬────────────────┘
                                       │
              ┌────────────────────────▼────────────────┐
              │      n8n Central Event Bus (:5678)       │
              └─────────────────────────────────────────┘
```

## Core Components

### `AlertCorrelator`
Handles all alertcorrelator operations. Exposes async methods callable from the core dispatcher.

### `ThreatEnricher`
Handles all threatenricher operations. Exposes async methods callable from the core dispatcher.

### `ComplianceReporter`
Handles all compliancereporter operations. Exposes async methods callable from the core dispatcher.

### `IncidentResponder`
Handles all incidentresponder operations. Exposes async methods callable from the core dispatcher.

### `WazuhAPIBridge`
Handles all wazuhapibridge operations. Exposes async methods callable from the core dispatcher.

---

## API Contract

All interactions follow the SBB standard envelope:

```http
POST /api/v1/execute
Content-Type: application/json
X-SBB-API-Key: <api-key>

{
  "action": "<operation>",
  "payload": {},
  "trace_id": "optional-uuid"
}
```

**Success Response (HTTP 200):**
```json
{
  "status": "success",
  "data": {},
  "trace_id": "...",
  "timestamp": "2025-01-01T00:00:00Z"
}
```

**Health Check:**
```http
GET /health
→ {"status": "healthy", "service": "sovereign-wazuh-siem", "port": 8808}
```

## Integration Matrix

| System | Protocol | Direction | Purpose |
|--------|----------|-----------|---------|
| n8n Event Bus (:5678) | HTTP POST | Outbound | Event forwarding |
| n8n Webhook | HTTP POST | Inbound | Trigger execution |
| SBB Codebase Vault (:8766) | HTTP | Outbound | Code analysis |
| SBB Patterns Bible (:8794) | HTTP | Outbound | Standards validation |
| External APIs | HTTPS | Outbound | Domain-specific data |

## Deployment Architecture

```yaml
# docker-compose excerpt
sovereign-wazuh-siem:
  image: sovereign-wazuh-siem:latest
  ports: ["8808:8808"]
  healthcheck:
    test: curl -f http://localhost:8808/health
    interval: 30s
```

## Security Model

| Control | Implementation |
|---------|---------------|
| Authentication | `X-SBB-API-Key` header (env: `SBB_API_KEY`) |
| Rate Limiting | 100 req/min per client IP |
| Input Validation | Pydantic models (strict mode) |
| Container Security | Non-root user (`appuser:1001`) |
| Secrets | Environment variables only (never hardcoded) |
| TLS | Terminate at reverse proxy (nginx/caddy) |

## Tags
`siem`, `wazuh`, `cybersecurity`, `compliance`
