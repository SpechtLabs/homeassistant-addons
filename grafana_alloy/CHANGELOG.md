<!-- https://developers.home-assistant.io/docs/add-ons/presentation#keeping-a-changelog -->

## 0.2.0

- Per-signal configuration: `loki_endpoint`, `mimir_endpoint` and `tempo_endpoint` are now independent and optional — only configured signals are wired up.
- Add traces: setting `tempo_endpoint` enables an OTLP receiver (ports 4317/gRPC, 4318/HTTP) that forwards to Tempo over OTLP/HTTP.
- Add basic-auth per signal (`loki_username`/`loki_password`, `mimir_*`, `tempo_*`), so Grafana Cloud and other secured backends now actually work. Passwords use Home Assistant's masked field.
- `tenant_id` is now optional: omitted when empty (for backends/gateways that inject tenancy server-side).
- Config is generated at runtime from the set options; a custom `/config/config.alloy` still overrides everything.
- Remove the unused `grafana_cloud` git-imported module (no startup network dependency).
- Fix the Alloy self-scrape to target the real listen port.
- Bump bundled Grafana Alloy from v1.7.5 to v1.17.0.

## 0.1.3

- Fix Alloy failing to start due to read-only file-system

## 0.1.2

- Fix HomeAssistant addon_config syntax in docker volume mount map

## 0.1.1

- Label Docker container correctly

## 0.1.0

- Initial release
