# Home Assistant Add-on: Grafana Alloy

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

Easily export Home Assistant metrics and logs to Grafana Cloud or your own observability stack.

---

## 📘 Overview

This add-on integrates [Grafana Alloy][grafana-alloy] with Home Assistant, allowing you to collect and forward system metrics and logs to [Grafana Cloud][grafana-cloud] or a self-hosted Grafana stack.
With Alloy and Prometheus support, you can create powerful, custom dashboards to visualize your Home Assistant data.

---

## ⚙️ Requirements

This add-on requires the [Prometheus integration] to be enabled in Home Assistant.

To enable it with the default settings, add the following to your `configuration.yaml` and restart Home Assistant:

```yaml
prometheus:
```

For advanced configuration options (such as filtering specific stats), refer to the [Prometheus integration] documentation.

---

## 🚀 Getting Started

To use this add-on, you'll need either:

1. A Grafana Cloud account (recommended)
2. A self-hosted Grafana stack with Loki and Mimir endpoints available.

## 🔧 Add-on Configuration

You can configure Alloy using either the built-in configuration UI or by supplying a full custom Alloy config.

### Option 1: Built-in Configuration

Provide the required values directly via the add-on configuration panel:

```yaml
tenant_id: foobar               # Tenant used for Mimir and Loki
instance_name: homeassistant    # Optional: useful for identifying multiple Home Assistant instances
scrape_interval: 60s
log_level: info
loki_endpoint: https://loki.example.net/loki/api/v1/push
mimir_endpoint: https://mimir.example.net/api/v1/push
```

### Option 2: Custom Alloy Configuration

For advanced setups, create a full Alloy config file and place it in your Home Assistant config directory:

* Path: `/config/config.alloy`

When this file is present, it will override the add-on's built-in configuration. This approach gives you full control over the Alloy setup.

## 🧪 Setting up Grafana Cloud

1. Create a Grafana Cloud account and set up your stack.
2. Copy your stack name and paste it into the add-on configuration.

### 3. Create an Access Policy

![Create Access Policy](https://github.com/grafana/home-assistant-addons/raw/main/grafana_cloud/images/create-access-policy.png)

* Display Name: Home Assistant
* Name: home-assistant
* Realms: Your stack
* Scopes:
  * Metrics: write
  * Logs: write
  * Stacks: read


### 4. Generate an API Token

Add a token to your access policy.

Copy the generated token and paste it into the token field of the add-on configuration.

## ✅ Start Exporting

Once configured, start the add-on. It will begin collecting and sending logs and metrics to your Grafana Cloud or self-hosted endpoints.

[grafana]: https://grafana.com
[grafana-cloud]: https://grafana.com/products/cloud/
[grafana-alloy]: https://grafana.com/docs/alloy/latest/
[integration]: https://grafana.com/solutions/home-assistant/monitor/
[Prometheus integration]: https://www.home-assistant.io/integrations/prometheus/
[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg