# Trino (trino)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
