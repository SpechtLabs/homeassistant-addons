# Home Assistant Add-on: CalendarAPI

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

CalendarAPI is a service that parses iCal files and exposes their content via gRPC or a REST API. It uses Viper for configuration, which supports runtime reloads.

---

## 📘 Overview

This add-on runs a small gRPC/REST service designed to parse iCal calendar files and expose relevant event data for use in meeting room displays, including ePaper panels. It supports advanced rule-based filtering and relabeling to ensure only the most important events are shown, and can be configured to auto-refresh on a schedule.

Built with flexibility and integration in mind, this service is ideal for displaying live calendar status on small, low-power devices.

---

## ⚙️ Features

- ✅ Parse iCal (.ics) files from **URLs or local files**
- ✅ Exposes events via **REST** and **gRPC** APIs
- ✅ Built-in **rule engine** for relabeling, filtering, and skipping events
- ✅ Supports **hot configuration reloads** (with [Viper](https://github.com/spf13/viper))
- ✅ Designed for use with **E-Paper displays** in Home Assistant environments

---

## 🛠 Configuration

### 🖥️ Server Configuration

```yaml
server:
  server: ""          # Host binding (default: all interfaces)
  httpPort: 8080      # HTTP API port
  grpcPort: 50051     # gRPC API port
  debug: false        # Enable debug logging
  refresh: 5m         # How often to refresh calendars (Go duration string)
```

⚠️ `server`, `httpPort`, and `grpcPort` cannot be changed at runtime.

### 📆 Calendar Configuration

You can define multiple calendar sources, each loading from either a local file or a remote URL:

```yaml
calendars:
  work:
    from: url
    ical: https://example.com/calendar.ics

  personal:
    from: url
    ical: https://example.com/calendar.ics
```

### 🧠 Rules Configuration

Rules let you filter, relabel, or skip events based on properties like title, time, or availability.

#### ✅ Relabeling Example

```yaml
rules:
  - name: "1:1s"
    key: "title"
    contains:
      - "1:1"
    relabelConfig:
      message: "1:1"
      important: true
```

This rule matches events with "1:1" in the title and marks them as important with a custom display message.

#### 🚫 Skipping Events

```yaml
rules:
  - name: "Skip all-day events"
    key: "all_day"
    contains:
      - "true"
    skip: true

  - name: "Skip non-blocking events"
    key: "busy"
    contains:
      - "Free"
    skip: true
```

These rules filter out non-essential events from the API output.

#### 🃏 Wildcard Rule

```yaml
rules:
  - name: "Allow everything else"
    key: "*"
    contains:
      - "*"
    important: false
```

Acts as a catch-all to ensure unmatched events are still included.

## 🚀 API Access

Once the add-on is running, the service exposes:

- A REST API on the configured HTTP port
- A gRPC server on the configured gRPC port

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg
