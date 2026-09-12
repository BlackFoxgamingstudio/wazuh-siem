# Changelog — Sovereign Wazuh SIEM

All notable changes follow [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format.
Versioning follows [Semantic Versioning](https://semver.org/).

## [Unreleased]

### Added
- Full ecosystem documentation suite (ARCHITECTURE, DEVELOPER_GUIDE, SME_PLAYBOOK, SOP)
- GitHub Actions CI matrix (Python 3.10 / 3.11 / 3.12)
- Multistage Dockerfile with non-root user, health check, OCI labels
- docker-compose.yml with SBB platform network
- n8n custom node integration via `SovereignTools`
- `.env.example` environment template
- CONTRIBUTING, CODE_OF_CONDUCT, SECURITY governance files
- OpenAPI 3.1 compatible REST API spec
- Bandit security scan in CI

## [1.0.0] — 2024-01-01

### Added
- Initial production release of Sovereign Wazuh SIEM (PKG-028)
- Core microservice on port `8808`
- n8n webhook adapter (`n8n/webhook_adapter.py`)
- REST API (`POST /api/v1/execute`, `GET /health`)
- Components: AlertCorrelator, ThreatEnricher, ComplianceReporter, IncidentResponder, WazuhAPIBridge
- pyproject.toml packaging with `[dev]` extras
- CLI: `sovereign-wazuh-siem --help`
- Unit test suite (pure `unittest.TestCase`, no external test framework required)

### Domain: Cybersecurity & SIEM Compliance
Wazuh SIEM integration layer with real-time alert correlation, threat intelligence enrichment, compliance reporting (SOC2/HIPAA/PCI), and automated incident response.

[Unreleased]: https://github.com/BlackFoxgamingstudio/wazuh-siem/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/BlackFoxgamingstudio/wazuh-siem/releases/tag/v1.0.0
