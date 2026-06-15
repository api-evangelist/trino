# Trino (trino)

Trino is an open-source, distributed SQL query engine built for lightning-fast analytics over large, heterogeneous data sets. Originally forked from Presto (which emerged at Facebook), it supports ANSI-compliant SQL across a wide range of storage systems from data lakes (S3, HDFS, Iceberg) to relational and NoSQL databases (MySQL, PostgreSQL, Cassandra, MongoDB, Elasticsearch, Kafka, and more).

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/trino/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/trino/refs/heads/main/apis.yml)

## Tags

- Analytics
- Big Data
- Distributed SQL
- MySQL
- NoSQL
- Queries
- SQL

## Timestamps

- **Created:** 2025-06-05
- **Modified:** 2026-05-19

## APIs

### Trino Client REST API

The Trino Client REST API enables clients to submit SQL queries to a Trino coordinator and retrieve results. Clients POST SQL to /v1/statement, then poll GET on the returned nextUri, and may DELETE nextUri to cancel. Used by all Trino client drivers (Python, Go, Java, JavaScript) and data tools like dbt, Grafana, and Apache Superset.

- **Human URL:** [https://trino.io/docs/current/develop/client-protocol.html](https://trino.io/docs/current/develop/client-protocol.html)
- **Base URL:** `http://localhost:8080`

#### Tags

- Analytics
- Big Data
- Queries
- SQL

#### Properties

- [Documentation](https://trino.io/docs/current/develop/client-protocol.html)
- [OpenAPI](openapi/trino-client-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/trino-client-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/trino-client-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [GitHub Organization](https://github.com/trinodb)
- [Python S D K](https://github.com/trinodb/trino-python-client)
- [Go S D K](https://github.com/trinodb/trino-go-client)
- [Java Script S D K](https://github.com/trinodb/trino-js-client)
- [Helm Chart](https://github.com/trinodb/charts)
- [C L I](https://github.com/trinodb/trino-admin)
- [J S O N L D Context](json-ld/trino-context.jsonld)
- [JSON Schema](json-schema/trino-query-results-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [Vocabulary](vocabulary/trino-vocabulary.yml)
- [Spectral Rules](rules/trino-rules.yml)
- [Website](https://trino.io/)
- [Documentation](https://trino.io/docs/current/)
- [Security](https://trino.io/docs/current/security.html)
- [Integrations](https://trino.io/docs/current/connector.html)
- [Glossary](https://trino.io/docs/current/glossary.html)
- [Changelog](https://trino.io/docs/current/release.html)
- [Getting Started](https://trino.io/download)
- [Events](https://trino.io/community#events)
- [Blog](https://trino.io/blog/)
- [Discussions](https://trino.io/community)
- [Features](undefined)
- [Integrations](undefined)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
