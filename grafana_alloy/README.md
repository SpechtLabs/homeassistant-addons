# Home Assistant Add-on: Grafana Alloy

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

Easily export Home Assistant metrics, logs and traces to Grafana Cloud or your own observability stack.

---

## 📘 Overview

This add-on integrates [Grafana Alloy][grafana-alloy] with Home Assistant, allowing you to collect and forward system metrics, logs and traces to [Grafana Cloud][grafana-cloud] or a self-hosted Grafana stack.
With Alloy and Prometheus support, you can create powerful, custom dashboards to visualize your Home Assistant data.

Each signal is independent: configure only the endpoints you need. Set a Loki endpoint to ship logs, a Mimir endpoint to ship metrics, a Tempo endpoint to receive and forward traces — any combination works.

---

## ⚙️ Requirements

This add-on requires the [Prometheus integration] to be enabled in Home Assistant (only needed if you ship metrics).

To enable it with the default settings, add the following to your `configuration.yaml` and restart Home Assistant:

```yaml
prometheus:
```

For advanced configuration options (such as filtering specific stats), refer to the [Prometheus integration] documentation.

---

## 🔧 Add-on Configuration

All options are optional. The add-on enables a signal only when its endpoint is set, so you wire up exactly what your backend needs.

| Option | Description |
| --- | --- |
| `loki_endpoint` | Loki push URL. Enables **logs** when set. |
| `loki_username` / `loki_password` | Basic-auth credentials for Loki. Applied only when **both** are set. |
| `mimir_endpoint` | Prometheus remote-write URL. Enables **metrics** when set. |
| `mimir_username` / `mimir_password` | Basic-auth credentials for Mimir. Applied only when **both** are set. |
| `tempo_endpoint` | OTLP/HTTP base URL. Enables an **OTLP traces receiver** (ports 4317/gRPC, 4318/HTTP) that forwards to Tempo. `/v1/traces` is appended automatically — use the bare host. |
| `tempo_username` / `tempo_password` | Basic-auth credentials for Tempo. Applied only when **both** are set. |
| `tenant_id` | Optional. Sent as Loki `tenant_id` and the Mimir `X-Scope-OrgID` header. Leave empty when your backend assigns tenancy itself (Grafana Cloud, or a gateway that injects the header). |
| `instance_name` | `instance` label on all telemetry. Default `homeassistant`. |
| `scrape_interval` | Metrics scrape/send interval. Default `60s`. |
| `log_level` | Verbosity of Alloy's own logs. |

> **Passwords/tokens** use Home Assistant's masked password field and are never written into a world-readable file beyond the add-on's own private config directory.

### Example: Grafana Cloud

Grafana Cloud uses HTTP basic auth: each service has its own numeric **user ID** as the username, and your **Access Policy token** as the password (the same token for all three). Find the user IDs on each service's "Details" page in the Grafana Cloud portal. Leave `tenant_id` empty.

```yaml
loki_endpoint: https://logs-prod-XXX.grafana.net/loki/api/v1/push
loki_username: "111111"
loki_password: glc_xxxxxxxxxxxxxxxxxxxx
mimir_endpoint: https://prometheus-prod-XXX.grafana.net/api/prom/push
mimir_username: "222222"
mimir_password: glc_xxxxxxxxxxxxxxxxxxxx
tempo_endpoint: https://tempo-prod-XXX.grafana.net
tempo_username: "333333"
tempo_password: glc_xxxxxxxxxxxxxxxxxxxx
instance_name: homeassistant
```

### Example: self-hosted with a tenant header

```yaml
loki_endpoint: https://loki.example.net/loki/api/v1/push
mimir_endpoint: https://mimir.example.net/api/v1/push
tenant_id: my-tenant
instance_name: homeassistant
```

### Example: gateway that injects tenancy (no client auth/tenant)

```yaml
loki_endpoint: http://my-loki.internal/loki/api/v1/push
mimir_endpoint: http://my-mimir.internal/api/v1/push
instance_name: homeassistant
# tenant_id omitted — the gateway sets X-Scope-OrgID server-side
```

### Custom Alloy configuration (full override)

For advanced setups, place a complete Alloy config in your Home Assistant config directory at `/config/config.alloy`. When present, it is used **verbatim** and the built-in options above are ignored — giving you full control over the Alloy pipeline.

## 🧪 Setting up Grafana Cloud

1. Create a Grafana Cloud account and set up your stack.
2. Create an Access Policy.

![Create Access Policy](https://github.com/grafana/home-assistant-addons/raw/main/grafana_cloud/images/create-access-policy.png)

   * Display Name: Home Assistant
   * Name: home-assistant
   * Realms: Your stack
   * Scopes: Metrics `write`, Logs `write`, Traces `write`, Stacks `read`

3. Generate an API token on the access policy and copy it.
4. For each of Loki, Mimir (Prometheus) and Tempo, copy the endpoint URL and the numeric user ID from its "Details" page in the portal, and paste the URL into the matching `*_endpoint`, the user ID into `*_username`, and the token into `*_password`.

## ✅ Start Exporting

Once configured, start the add-on. It will begin collecting and sending the enabled signals to your Grafana Cloud or self-hosted endpoints.

[grafana]: https://grafana.com
[grafana-cloud]: https://grafana.com/products/cloud/
[grafana-alloy]: https://grafana.com/docs/alloy/latest/
[integration]: https://grafana.com/solutions/home-assistant/monitor/
[Prometheus integration]: https://www.home-assistant.io/integrations/prometheus/
[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
