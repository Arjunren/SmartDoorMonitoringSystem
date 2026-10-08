# Architecture — SmartDoorMonitoringSystem

The digital acquisition path yields a `Sample` of door open and reserved. `logic.h` validates the primary value against 0–1 boolean and evaluates `s.value > 0.5f` only for valid samples. No actuator is implemented; alerts are telemetry flags only.

```mermaid
sequenceDiagram
  participant Sensor
  participant ESP32
  participant Broker
  Sensor->>ESP32: door open / reserved
  ESP32->>ESP32: validate and decide
  ESP32->>Broker: TLS MQTT telemetry JSON
```

Wi-Fi and broker reconnection use a backoff from one to sixty seconds. The firmware has no inbound command handler. MQTT broker provisioning, persistence, alert delivery, and visualization are outside the codebase. SNTP supplies wall time before TLS certificate validation.
