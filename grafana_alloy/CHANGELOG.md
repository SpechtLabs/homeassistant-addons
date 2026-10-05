<!-- https://developers.home-assistant.io/docs/add-ons/presentation#keeping-a-changelog -->

## 0.4.0

- Bump Grafana Alloy from v1.17.1 to v1.20.1. The OTLP receiver now closes idle HTTP connections after a minute, as upstream OpenTelemetry does; Alloy v1.18.0 changed that default from no timeout.
- Build the image with Home Assistant's BuildKit-based builder actions, on the multi-platform Debian base image, and sign it with Cosign.

## 0.3.1

- Fix the add-on failing to start whenever `loki_endpoint` is set: the generated config left the journal relabel block unclosed, and Alloy's own logs were sent to a `loki.process` component the config no longer defined.
- Map the add-on config directory as `addon_config` again; 0.3.0 renamed it to `app_config`, which the Supervisor does not support.

## 0.3.0

### ⚠️ **Breaking change**

- Switch log ingestion from file scraping (`home-assistant.log`) to native systemd journal (`loki.source.journal`).
- Switch base image to Debian Bookworm (`base-debian:bookworm`) for native `libsystemd` support.
- Improve journal relabeling pipeline with fallback handling for audit transport logs and missing log levels.
- Update Home Assistant metadata annotations (`type: app_config` in `config.yaml` and `io.hass.type="app"` in `Dockerfile`).
- Bump Alloy from `1.17.0` to `1.17.1`

## 0.2.1

- Fix add-on failing to start (`/usr/bin/grafana-alloy: Permission denied`, exit 126): the Alloy v1.17.0 release zip ships the binary without the executable bit, so `chmod +x` it after extraction.

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
