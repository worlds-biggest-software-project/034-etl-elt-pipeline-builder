# Standards & API Reference

> Project: ETL/ELT Pipeline Builder · Generated: 2026-05-04

## Industry Standards & Specifications

### ISO Standards

#### ISO 8000 — Data Quality
- **URL:** https://www.iso.org/standard/50798.html (overview); https://eccma.org/what-is-iso-8000/
- **Relevance:** The international standard for Data Quality and Master Data. Defines requirements for standard exchange of master data among systems, covering syntax, provenance, completion, accuracy, and certification — directly applicable to validating data passing through ETL pipelines before loading to destinations.

#### ISO/IEC 11179 — Metadata Registries (MDR)
- **URL:** https://www.iso.org/standard/78914.html
- **Relevance:** Defines how metadata elements should be standardised and registered to make data understandable and shareable across disparate systems. Part 7 (ISO/IEC 11179-7:2019) covers metamodels for dataset registration, relevant to schema management in pipeline connectors.

#### ISO/IEC 23751:2022 — Data Sharing Agreement (DSA) Framework
- **URL:** https://www.iso.org/standard/76834.html
- **Relevance:** Establishes building blocks for Data Sharing Agreements in cloud and distributed platform environments, including Data Level Objectives (DLOs) and Data Qualitative Objectives (DQOs). Relevant to cross-organisation ETL pipelines where formal data-sharing contracts govern what may be extracted, transformed, or loaded.

---

### W3C & IETF Standards

#### RFC 8259 — JSON Data Interchange Format
- **URL:** https://www.rfc-editor.org/rfc/rfc8259
- **Relevance:** The IETF standard defining the JSON format (also ECMA-404). JSON is the dominant wire format for connector message passing (Singer spec, Airbyte protocol, REST APIs) and configuration files across virtually all modern ETL/ELT tooling.

#### JSON Schema (Draft 2020-12)
- **URL:** https://json-schema.org/specification
- **Relevance:** Provides vocabulary for structural validation of JSON documents. Used extensively by pipeline tools to validate connector configuration, source/destination schemas, and message payloads. OpenLineage's core spec is itself formalised as a JSON Schema document.

#### W3C PROV — Provenance Data Model
- **URL:** https://www.w3.org/TR/prov-dm/ (data model); https://www.w3.org/TR/prov-overview/
- **Relevance:** The W3C family of provenance specifications (PROV-DM, PROV-N, PROV-O) provides a domain-agnostic, interoperable model for expressing data provenance — who created data, how it was transformed, and where it came from. Forms the conceptual foundation for data-lineage features in ETL platforms and underpins GDPR audit-trail obligations.

#### RFC 6749 — OAuth 2.0 Authorization Framework
- **URL:** https://www.rfc-editor.org/rfc/rfc6749
- **Relevance:** The foundational IETF standard for delegated authorisation, widely used by connector authentication flows (Airbyte, Fivetran, Matillion, Azure Data Factory). Bearer-token flows from this RFC are the de facto mechanism for securing REST API calls between pipeline orchestrators and source/destination systems.

#### RFC 9700 — OAuth 2.0 Security Best Current Practice
- **URL:** https://datatracker.ietf.org/doc/rfc9700/
- **Relevance:** Published January 2025, this updates and extends the OAuth 2.0 threat model with modern guidance on token security, redirect-URI validation, and PKCE requirements — directly relevant to secure connector credential management in ETL platforms.

#### RFC 8446 — Transport Layer Security (TLS) 1.3
- **URL:** https://datatracker.ietf.org/doc/html/rfc8446
- **Relevance:** Defines TLS 1.3, the current minimum-recommended encryption standard for data-in-transit. All pipeline connections between extractors, transformation engines, and loaders must use TLS 1.3 (or 1.2 as a fallback) to satisfy enterprise security and GDPR Article 32 requirements.

---

### Data Model & API Specifications

#### OpenAPI Specification 3.1
- **URL:** https://spec.openapis.org/oas/v3.1.0 (spec); https://www.openapis.org/
- **Relevance:** The vendor-neutral standard for describing RESTful APIs in YAML or JSON. ETL platform management APIs (Fivetran, Airbyte, Matillion, Stitch) are documented via OpenAPI definitions, enabling client code generation, interactive documentation, and contract testing between orchestration layers.

#### Apache Avro
- **URL:** https://avro.apache.org/docs/ (docs); https://avro.apache.org/docs/1.11.1/specification/
- **Relevance:** A row-oriented data serialisation framework using JSON schemas for type definition and compact binary encoding. Common in Kafka-based ETL pipelines for schema-governed event streaming. Avro schemas travel with the data, enabling schema evolution without code generation.

#### Apache Arrow — Columnar Format
- **URL:** https://arrow.apache.org/docs/format/Columnar.html
- **Relevance:** Defines an in-memory columnar data format for efficient CPU-native analytics. Used as the interchange format between pipeline stages within the same process or across language runtimes (Python, Java, Rust). Arrow IPC format enables zero-copy data sharing between transformation steps.

#### Apache Parquet
- **URL:** https://parquet.apache.org/ (overview); https://github.com/apache/parquet-format (spec)
- **Relevance:** The dominant columnar storage file format for data lakes (S3, ADLS, GCS). ETL pipeline loaders targeting cloud data lakes or lakehouses must write Parquet-compatible output. Parquet's support for compression codecs (snappy, zstd, gzip) and nested schemas via Dremel encoding makes it the standard for analytical workload destinations.

#### Protocol Buffers (Protobuf)
- **URL:** https://protobuf.dev/
- **Relevance:** Google's language-neutral binary serialisation format, used for efficient on-the-wire data exchange in high-throughput ETL pipelines and gRPC-based connector APIs. Complements Arrow (in-memory) and Parquet (storage) in the data serialisation stack.

---

### Security & Authentication Standards

#### GDPR — Data Lineage & Processing Records
- **URL:** https://gdpr-info.eu/ (Articles 30, 32, 20)
- **Relevance:** GDPR Article 30 requires Records of Processing Activities documenting data flows. Article 32 mandates appropriate technical measures (encryption, pseudonymisation). Article 20 grants data portability rights. ETL pipelines handling EU personal data must maintain column-level lineage, encryption-at-rest and in-transit, and support subject-access and erasure requests across the data estate.

---

### Pipeline & Connector Protocol Standards

#### Singer Specification
- **URL:** https://www.singer.io/; https://hub.meltano.com/singer/spec/; https://github.com/singer-io/getting-started/blob/master/docs/SPEC.md
- **Relevance:** The open standard (v0.3.0) for composable ETL connectors: Taps (extractors) communicate with Targets (loaders) via three JSON message types (SCHEMA, RECORD, STATE) over stdout. Hundreds of community-maintained connectors are built to this spec, and it underpins Meltano and is compatible with Airbyte's protocol.

#### Airbyte Protocol
- **URL:** https://docs.airbyte.com/platform/understanding-airbyte/airbyte-protocol
- **Relevance:** Describes the standard components, message formats (JSON over IPC), and interaction patterns for declaring an ELT pipeline. Defines Catalog (schema discovery), ConfiguredCatalog (sync configuration), and AirbyteMessage types. Compatible with Singer taps, enabling cross-ecosystem connector reuse.

#### dbt Model Contracts
- **URL:** https://docs.getdbt.com/docs/mesh/govern/model-contracts
- **Relevance:** dbt contracts enforce column names, data types, and constraints on transformation models as a pre-build preflight check — effectively a typed schema contract for the transformation layer. Relevant to ELT pipelines where dbt performs in-warehouse transformations and upstream/downstream teams need guarantees on model schemas.

#### OpenLineage
- **URL:** https://openlineage.io/; https://github.com/OpenLineage/OpenLineage; https://github.com/OpenLineage/OpenLineage/blob/main/spec/OpenLineage.md
- **Relevance:** An open standard (formalised as JSON Schema) for collecting and emitting lineage metadata about datasets, jobs, and pipeline runs. Supported by Apache Airflow, dbt, Spark, Flink, and major cloud platforms. Enables a pipeline builder to emit standardised lineage events consumable by governance tools (Marquez, DataHub, OpenMetadata) without vendor lock-in.

---

## Similar Products — Developer Documentation & APIs

### Airbyte

- **Description:** Open-source ELT platform with a large connector catalogue (700+). Offers a self-hosted Community edition and a managed Cloud service with a declarative low-code CDK for connector development.
- **API Documentation:** https://reference.airbyte.com/ (REST reference); https://docs.airbyte.com/developers/api-documentation
- **SDKs/Libraries:** Python CDK — `pip install airbyte-cdk` (https://pypi.org/project/airbyte-cdk/); Python API SDK — https://github.com/airbytehq/airbyte-api-python-sdk
- **Developer Guide:** https://docs.airbyte.com/platform/connector-development; https://docs.airbyte.com/platform/connector-development/config-based/low-code-cdk-overview
- **Standards:** REST (OpenAPI-documented), JSON over IPC (Airbyte Protocol), Singer-compatible
- **Authentication:** Bearer token (short-lived access tokens, 60-minute TTL); tokens generated via the Airbyte UI and passed in the `Authorization: Bearer` header

### Fivetran

- **Description:** Fully managed ELT service with automated schema migration and an extensive library of pre-built connectors. Targets cloud data warehouses (Snowflake, BigQuery, Redshift) with minimal configuration.
- **API Documentation:** https://fivetran.com/docs/rest-api; https://fivetran.com/docs/rest-api/api-reference
- **SDKs/Libraries:** Postman collection (https://fivetran.com/docs/rest-api/api-tools); no official language SDK — API is consumed directly via HTTP
- **Developer Guide:** https://fivetran.com/docs/rest-api/getting-started; https://fivetran.com/docs/rest-api/powered-by-fivetran
- **Standards:** REST (OpenAPI-generated reference); JSON payloads; HTTPS required
- **Authentication:** HTTP Basic Authentication using Base64-encoded `api_key:api_secret` in the `Authorization` header; Scoped API keys and System keys supported

### dbt (dbt Labs)

- **Description:** The industry-standard SQL-first transformation tool for data warehouses. dbt Core is open-source; dbt Cloud adds managed scheduling, CI, the Semantic Layer, and collaborative features for teams.
- **API Documentation:** https://docs.getdbt.com/dbt-cloud/api-v2 (Admin API); https://docs.getdbt.com/docs/dbt-cloud-apis/sl-api-overview (Semantic Layer API); https://docs.getdbt.com/docs/dbt-cloud-apis/overview
- **SDKs/Libraries:** Python Semantic Layer SDK — https://github.com/dbt-labs/semantic-layer-sdk-python (`pip install dbt-sl-sdk`); dbt-cloud-cli — https://pypi.org/project/dbt-cloud-cli/
- **Developer Guide:** https://docs.getdbt.com/docs/dbt-cloud-apis/overview; https://docs.getdbt.com/blog/making-dbt-cloud-api-calls-using-dbt-cloud-cli
- **Standards:** REST (Admin API, Discovery API); GraphQL (Semantic Layer); dbt Model Contracts (YAML schema enforcement)
- **Authentication:** API tokens (personal or service account) passed as Bearer tokens in the `Authorization` header

### Apache NiFi

- **Description:** Open-source Apache project providing a visual dataflow programming environment for routing, transforming, and mediating data between systems. Excels at complex routing logic and real-time streaming pipelines.
- **API Documentation:** https://nifi.apache.org/docs/nifi-docs/rest-api/index.html (v1.28.0); https://nifi.apache.org/docs/nifi-registry-docs/rest-api/index.html
- **SDKs/Libraries:** No official language SDK; REST API consumed directly; NiFi Python API available for processor development in Python
- **Developer Guide:** https://nifi.apache.org/nifi-docs/developer-guide.html; https://nifi.apache.org/documentation/
- **Standards:** REST (Swagger-documented); HTTPS; Processor naming conventions (Get\<Service\>, Put\<Service\>)
- **Authentication:** HTTP Bearer token (access tokens) in the `Authorization` header; TLS mutual authentication supported for node-to-node communication

### Meltano

- **Description:** Open-source DataOps platform built on the Singer specification. Provides a CLI and project framework for managing Singer taps/targets and dbt transformations, with a plugin hub of 600+ connectors.
- **API Documentation:** https://sdk.meltano.com/ (Singer SDK docs); https://hub.meltano.com/singer/spec/ (Singer spec)
- **SDKs/Libraries:** Meltano Singer SDK — https://sdk.meltano.com/en/latest/ (`pip install meltano-sdk`); supports Tap, Target, RESTStream, GraphQLStream base classes
- **Developer Guide:** https://sdk.meltano.com/en/latest/dev_guide.html; https://hub.meltano.com/singer/docs/
- **Standards:** Singer Specification (v0.3.0); REST and GraphQL stream types; JSON schema validation of catalog entries
- **Authentication:** Configurable per connector; typically Bearer token, Basic auth, or OAuth 2.0 depending on the tap/target being used

### Stitch (Talend)

- **Description:** Cloud ETL service (now part of Qlik/Talend) aimed at developers. Provides a Connect API for embedding ETL capabilities and an Import API for pushing arbitrary data into the pipeline.
- **API Documentation:** https://www.stitchdata.com/docs/developers/stitch-connect/api; https://www.stitchdata.com/docs/developers/import-api/api
- **SDKs/Libraries:** JavaScript Connect Card library for embedding source-connection flows; no official language SDK
- **Developer Guide:** https://www.stitchdata.com/docs/developers/stitch-connect/guides/stitch-partner-authentication-guide; https://www.stitchdata.com/platform/embedding/
- **Standards:** REST; JSON; HTTPS
- **Authentication:** Bearer token authentication; access token associated with a single Stitch client account passed in the `Authorization: Bearer` header

### Matillion

- **Description:** Cloud-native ETL/ELT platform for Snowflake, Redshift, and BigQuery. Provides a visual pipeline designer and a Data Productivity Cloud API with both a legacy ETL API and a newer REST API using OAuth 2.0.
- **API Documentation:** https://docs.matillion.com/metl/docs/2916124/ (ETL API v1); https://docs.matillion.com/data-productivity-cloud/api/docs/intro/
- **SDKs/Libraries:** No official language SDK; API consumed via HTTP with standard REST clients
- **Developer Guide:** https://docs.matillion.com/data-productivity-cloud/api/docs/authentication/; https://docs.matillion.com/metl/docs/2863654/
- **Standards:** REST; OpenAPI; OAuth 2.0 (Data Productivity Cloud); HTTP Basic auth (legacy ETL API)
- **Authentication:** OAuth 2.0 Client Credentials (token endpoint: `https://id.core.matillion.com/oauth/dpc/token`); tokens valid for 30 minutes; legacy API uses HTTP Basic auth (username/password hash)

### Talend (Qlik Talend)

- **Description:** Enterprise data integration platform (now part of Qlik) offering open-source Talend Open Studio and commercial Talend Cloud. Provides visual job design, 900+ connectors, and a Component Kit for custom connector development.
- **API Documentation:** https://talend.qlik.dev/ (API portal); https://api.us.cloud.talend.com/tmc/swagger/swagger-ui.html (TMC Public API); https://help.qlik.com/talend/en-US/talend-data-catalog/8.1/Subsystems/UserGuide/Content/REST-API.htm
- **SDKs/Libraries:** Talend Component Kit (Java-based) — https://talend.github.io/component-runtime/; REST components (`tRESTClient`, `tExtractJSONFields`) built into Open Studio
- **Developer Guide:** https://talend.github.io/component-runtime/main/1.0.5/all-in-one.html; https://help.talend.com/en-us/studio-user-guide/7.3/api-integration-overview
- **Standards:** REST; OpenAPI (Swagger UI for TMC API and Component Kit); SOAP; HTTP Basic and Bearer token auth
- **Authentication:** Personal Access Tokens or Service Account tokens (new API portal); Basic auth for legacy deployments; OAuth 2.0 supported for certain components

### AWS Glue

- **Description:** Fully managed serverless ETL service on AWS. Provides a Data Catalog, visual ETL job authoring (AWS Glue Studio), and PySpark/Ray-based transformation runtimes with 70+ native connectors.
- **API Documentation:** https://docs.aws.amazon.com/glue/latest/dg/aws-glue-api.html; https://docs.aws.amazon.com/glue/latest/webapi/WebAPI_Welcome.html
- **SDKs/Libraries:** AWS SDK for Python (Boto3) — `pip install boto3`; code examples at https://docs.aws.amazon.com/code-library/latest/ug/python_3_glue_code_examples.html
- **Developer Guide:** https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-calling.html; https://docs.aws.amazon.com/glue/latest/dg/aws-glue-programming-python-setup.html
- **Standards:** AWS-proprietary API (JSON over HTTPS); IAM-based access control; AWS Glue Schema Registry supports Avro and JSON Schema
- **Authentication:** AWS IAM (AWS Signature Version 4); credentials configured via AWS CLI or environment variables; Boto3 uses the same credential chain

### Azure Data Factory (ADF)

- **Description:** Microsoft's fully managed, serverless cloud ETL service with 90+ built-in connectors. Supports code-free visual pipeline design and hybrid data integration between on-premises and cloud systems.
- **API Documentation:** https://learn.microsoft.com/en-us/azure/data-factory/quickstart-create-data-factory-rest-api; https://learn.microsoft.com/en-us/azure/data-factory/
- **SDKs/Libraries:** Azure SDK for Python (`pip install azure-mgmt-datafactory`); Azure SDK for .NET, Java, JavaScript; documentation at https://learn.microsoft.com/en-us/azure/data-factory/
- **Developer Guide:** https://learn.microsoft.com/en-us/azure/data-factory/connector-rest (REST connector); https://learn.microsoft.com/en-us/azure/data-factory/
- **Standards:** REST (Azure Resource Manager API); JSON pipeline definitions; OpenAPI-compatible; supports anonymous, Basic, Service Principal, OAuth 2.0, and Managed Identity authentication on connectors
- **Authentication:** Microsoft Entra ID (Azure AD) — Service Principal, System-assigned or User-assigned Managed Identity, or OAuth 2.0 Client Credentials for management plane; connector authentication varies per connector (Basic, OAuth 2.0, API key, Managed Identity)

---

## Notes

- **Singer vs. Airbyte protocol:** The Airbyte protocol is a superset of Singer. Existing Singer taps can be wrapped for Airbyte use via `tap-airbyte-wrapper` (https://github.com/MeltanoLabs/tap-airbyte-wrapper), giving access to 700+ Airbyte connectors from within the Singer/Meltano ecosystem.
- **Data format convergence:** The modern ETL/ELT stack converges on JSON for configuration and message passing (Singer, Airbyte, REST APIs), Apache Parquet for landed storage in data lakes, and Apache Arrow for in-process columnar exchange between transformation steps.
- **Lineage as a first-class concern:** OpenLineage is rapidly becoming the de facto standard for lineage emission. A pipeline builder should emit OpenLineage-compatible events natively to enable integration with governance platforms (Marquez, DataHub, OpenMetadata, AWS SageMaker) without custom adapters.
- **GDPR and compliance:** Any pipeline builder handling EU personal data must support column-level lineage tracking, data-at-rest encryption, TLS 1.3 for data-in-transit, and connectors that can honour erasure ("right to be forgotten") propagation across pipeline stages.
- **OAuth 2.0 remains the connector authentication standard:** All major ETL/ELT vendors have converged on OAuth 2.0 bearer tokens for API authentication. RFC 9700 (2025) best practices — including PKCE and short-lived token rotation — should be followed in connector credential management flows.
- **dbt as the transformation standard:** In modern ELT architectures, dbt is effectively the industry standard for the T in ELT, with its model contracts providing typed schema guarantees that bridge extraction and downstream consumption. A pipeline builder should natively support dbt project integration or expose dbt-compatible transformation hooks.
