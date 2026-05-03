# Trino

Trino is an open-source, distributed SQL query engine built for lightning-fast analytics over large, heterogeneous data sets. Originally forked from Presto (which emerged at Facebook), it supports ANSI-compliant SQL across a wide range of storage systems from data lakes (S3, HDFS, Iceberg) to relational and NoSQL databases (MySQL, PostgreSQL, Cassandra, MongoDB, Elasticsearch, Kafka, and more). Architecturally, Trino employs a massively parallel processing (MPP) model with a dedicated coordinator that plans queries and multiple worker nodes that execute them in parallel, allowing it to scale from gigabytes to petabytes of data.

**URL:** https://raw.githubusercontent.com/api-evangelist/trino/refs/heads/main/apis.yml

## Tags

Analytics, Big Data, Distributed SQL, MySQL, NoSQL, Queries, SQL

## APIs

### Trino Client REST API

The Trino Client REST API enables clients to submit SQL queries to a Trino coordinator and retrieve results. Clients POST SQL to /v1/statement, then poll GET on the returned nextUri, and may DELETE nextUri to cancel.

**Human URL:** https://trino.io/docs/current/develop/client-protocol.html

**Base URL:** http://localhost:8080

#### Properties

| Type | URL |
|------|-----|
| Documentation | https://trino.io/docs/current/develop/client-protocol.html |
| OpenAPI | [openapi/trino-client-api-openapi.yml](openapi/trino-client-api-openapi.yml) |

#### Tags

Analytics, Big Data, Queries, SQL

## Artifacts

| Artifact | Path |
|----------|------|
| OpenAPI Spec | [openapi/trino-client-api-openapi.yml](openapi/trino-client-api-openapi.yml) |
| JSON Schema | [json-schema/trino-query-results-schema.json](json-schema/trino-query-results-schema.json) |
| JSON Structure | [json-structure/trino-query-results-structure.json](json-structure/trino-query-results-structure.json) |
| JSON-LD Context | [json-ld/trino-context.jsonld](json-ld/trino-context.jsonld) |
| Spectral Rules | [rules/trino-rules.yml](rules/trino-rules.yml) |
| Vocabulary | [vocabulary/trino-vocabulary.yml](vocabulary/trino-vocabulary.yml) |

## Examples

| Example | Path |
|---------|------|
| Submit Statement | [examples/trino-submit-statement-example.json](examples/trino-submit-statement-example.json) |
| Get Cluster Info | [examples/trino-get-cluster-info-example.json](examples/trino-get-cluster-info-example.json) |
| Cancel Query | [examples/trino-cancel-query-example.json](examples/trino-cancel-query-example.json) |

## Capabilities

| Capability | Description |
|-----------|-------------|
| [sql-analytics](capabilities/sql-analytics.yaml) | Unified SQL analytics workflow combining query submission, result polling, and cluster management |

### Shared Definitions

| Shared Definition | API |
|-------------------|-----|
| [trino-client-api](capabilities/shared/trino-client-api.yaml) | Trino Client REST API |

## SDKs and Clients

- [Python Client](https://github.com/trinodb/trino-python-client)
- [Go Client](https://github.com/trinodb/trino-go-client)
- [JavaScript Client](https://github.com/trinodb/trino-js-client)
- [Helm Charts](https://github.com/trinodb/charts)
- [Admin Tool](https://github.com/trinodb/trino-admin)

## Common Properties

- [GitHub Organization](https://github.com/trinodb)
- [Website](https://trino.io/)
- [Documentation](https://trino.io/docs/current/)
- [Security](https://trino.io/docs/current/security.html)
- [Connectors](https://trino.io/docs/current/connector.html)
- [Glossary](https://trino.io/docs/current/glossary.html)
- [ChangeLog](https://trino.io/docs/current/release.html)
- [Getting Started](https://trino.io/download)
- [Events](https://trino.io/community#events)
- [Blog](https://trino.io/blog/)
- [Discussions](https://trino.io/community)

## Maintainers

**FN:** Kin Lane  
**Email:** kin@apievangelist.com
