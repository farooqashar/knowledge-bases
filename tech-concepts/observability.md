---
title: Observability and OpenTelemetry
tags: [devops, infrastructure, monitoring]
updated: 2026-10-27
---

# Observability and OpenTelemetry

Observability is being able to understand what a running system is doing, and why, from the data it emits. **OpenTelemetry (OTel)** is the open standard for producing and shipping that data, so you can instrument code once and send it to any backend.

## How it works

Systems emit three main kinds of **telemetry**:

- **Logs**: timestamped records of events ("payment failed for order 123").
- **Metrics**: numbers over time (requests per second, error rate, memory use). Cheap to store, good for dashboards and alerts.
- **Traces**: the path of one request through every service it touches, broken into timed **spans**. They show where time goes and where errors start.

OpenTelemetry provides:

1. **SDKs and auto-instrumentation** that capture telemetry from your code and common libraries.
2. A **context propagation** standard so a trace ID follows a request across services.
3. The **Collector**, an optional agent that receives, processes and forwards telemetry.
4. **OTLP**, the wire protocol, which tools like Grafana, Honeycomb, Datadog and Jaeger accept.

## Key terms

- **Span**: one timed operation inside a trace, with attributes and a parent.
- **Cardinality**: the number of distinct label combinations on a metric. High cardinality (a user ID as a label) gets expensive fast.
- **Sampling**: keeping only some traces to control cost.
- **SLO**: a target like "99.9% of requests succeed," used to decide what deserves an alert.

## Common pitfalls

- Logging everything and alerting on everything, so real problems drown in noise.
- Unstructured log lines that are hard to search. Log JSON with consistent field names.
- Putting user IDs or other unbounded values in metric labels.

## Related

- [CI/CD](ci-cd.md)
- [Serverless](serverless.md)
