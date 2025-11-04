# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.3.5] - 2025-11-04

- Fixed ClusterIssuer for SDTooling-API-BE, EDC-Connector-Adapter and XFSC-AdvSearch-BE ingresses (SIMPL-15891)
- Moved infrastructure-crossplane and tofu controller to dependencies (SIMPL-19845)

## [2.3.4] - 2025-10-31

- Replaced HashiCorp Vault by OpenBao

## [2.3.3] - 2025-10-30

- Update monitoring stack to version 0.1.20

## [2.3.2] - 2025-10-28

- Updated infrastructure-be component to 1.0.11 (fixing bug SIMPL-19937)

## [2.3.1] - 2025-10-14

- Updated simpl_catalogue_client: 1.2.7, xfsc_advsearch_be: 1.14.2, sd_ui: targetRevision: 1.2.4

## [2.3.0] - 2025-10-10

- Updated many components to implement Consumer version 2.3.0.
- Add component Redis.
- Add component Tier2-Proxy.

### Simpl Cloud gateway (Tier 1)

#### 2.4.0 (2025-09-08)

#### Added

- SIMPL-14971

#### Changed

- SIMPL-15701

### Users Roles

#### 2.4.0 (2025-09-08)

#### Added

- SIMPL-14971

#### Fixed

- SIMPL-16771

### SIMPL FE

#### 2.4.0 (2025-09-08)

#### Added

- SIMPL-15662
- SIMPL-15665
- SIMPL-12865
- SIMPL-15656
- SIMPL-15655

#### Fixed

- SIMPL-15726
- SIMPL-16121
- SIMPL-15909
- SIMPL-15909

### TLS Gateway (Tier 2)

#### 2.4.0 (2025-09-08)

#### Added

- SIMPL-14971

#### Fixed

- SIMPL-10191

### Tier 2 Proxy

#### 1.0.1 (2025-08-06)

#### Fixed

- Fixed base docker image

### Authentication Provider

#### 2.4.2 (2025-09-26)

#### Fixed
- Identity Attributes of local copy get creationTimestamp and updateTimestamp from authority synchronization flow
- SIMPL-13018

#### 2.4.1 (2025-09-18)
No changes.

#### 2.4.0 (2025-09-08)

#### Added

- Added unique constraint on private_key.keypair_id column
- SIMPL-14971
- SIMPL-12990

### sd-creator-backend

#### 1.15.2 (2025-09-15)

#### Fixed

- SIMPL-16767

#### Changed

- simpl-data1-common upgraded to 1.4.1 to enable request uri tracing in

#### 1.15.1 (2025-09-09)

No changes.

#### 1.15.0 (2025-09-04)

#### Added

- SIMPL-17313
- SIMPL-15584
- SIMPL-15289

#### Changed

- SIMPL-17453
- SIMPL-8416
- SIMPL-2775

### sd-creator-frontend

#### 1.2.3 (2025-09-02)

#### Fixed

- SIMPL-17423

### sdtooling-validation-api-be

#### 1.14.0 (2025-09-04)

#### Added

- SIMPL-17313

### xsfc-advsearch-be

#### 1.14.1 (2025-09-04)

No changes.

#### 1.14.0 (2025-09-20)

#### Added

- SIMPL-17313

#### Fixed

- SIMPL-17434

### edc connector adapter

#### 1.6.0 (2025-09-04)

#### Added

- SIMPL-17313

#### Fixed

- SIMPL-8258

### simpl-edc

#### 1.0.11 (2025-09-05)

#### Changed

- SIMPL-14812 fix sonar issues

### simpl-catalogue-client

#### 1.2.6 (2025-09-30)
No changes.

#### 1.2.5 (2025-08-04)

#### Added

- SIMPL-14811

#### Changed

- SIMPL-14790

#### Fixed

- SIMPL-10725

### simpl-files

#### 1.1.0 (2025-08-06)

#### Changed

- SIMPL-16125

### infrastructure-be

#### 1.0.7 (2025-09-03)

#### Added
- SIMPL-16167
- SIMPL-16422
- SIMPL-16420
- SIMPL-16169
- SIMPL-16369
- SIMPL-16588
- SIMPL-16765
- SIMPL-16167
- SIMPL-16165

#### Changed
- SIMPL-16167
- SIMPL-17095
- SIMPL-15623
- SIMPL-16167

#### Fixed

- SIMPL-16165

### infrastructure-fe

#### 1.0.0 (2025-09-04)

#### Added
- SIMPL-17022
- SIMPL-17091
- SIMPL-16761
- SIMPL-16179

#### Changed

- SIMPL-17091
- SIMPL-17022
- SIMPL-17025
- SIMPL-16179

### infrastructure-crossplane

#### 1.2.3 (2025-09-16)

No changes.

### Filebeat

#### 0.1.18 (2025-09-04)

No changes.

### Contract Manager

#### 2.0.9 (2025-10-02)

No changes.

#### 2.0.6 (2025-09-03)

#### Changed
- SIMPL-15093
