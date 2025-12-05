# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.4.1] - 2025-12-02
- Fixed SIMPL-21567 bug.

## [2.4.0] - 2025-11-15
- Updated many components to implement Consumer version 2.4.0.


### Simpl Cloud gateway (Tier 1)

#### 2.5.0 (2025-09-29)

#### Added
- Added new routes for Security Attributes Provider

#### Fixed
- Https constraints applied in Content Security Policy only when https origins are present


### Users Roles

#### 2.5.1 (2025-10-16)

#### Fixed
- Identity Attributes validation now handles correctly identity attributes not assigned to participant, not assignable to roles and disabled.

#### 2.5.0 (2025-09-29)

#### Fixed
- SIMPL-12860
- SIMPL-16081


### SIMPL FE

#### 2.5.0 (2025-09-29)

#### Added
- SIMPL-14573
- SIMPL-16741
- SIMPL-16738
- SIMPL-16739
- SIMPL-16740

#### Fixed
- SIMPL-16738


### TLS Gateway (Tier 2)

#### 2.5.0 (2025-09-29)

#### Added
- Added new routes for Security Attributes Provider

#### Fixed
- SIMPL-14604


### Tier 2 Proxy

#### 1.0.1 (2025-08-06)

#### Fixed
- Fixed base docker image


### Authentication Provider

#### 2.5.2 (2025-10-17)

#### Fixed
- Removed bitnami legacy image from helm chart

#### 2.5.1 (2025-10-07)

#### Fixed
- Attempt identity attributes update after storing the ephemeral proof
- Avoid storing already expired ephemeral proofs

#### 2.5.0 (2025-09-29)

#### Added
- SIMPL-17522
- SIMPL-17529
- SIMPL-17530
- SIMPL-17492
- SIMPL-17517
- SIMPL-17516

#### Fixed
- SIMPL-16621


### sd-creator-backend

#### 1.16.1 (2025-11-05)

#### Fixed
- SIMPL-20138

#### 1.16.0 (2025-09-26)

#### Added
- SIMPL-15622
- SIMPL-16767

#### Fixed
- fixed request logging issue

#### Changed
- SIMPL-16767


### sd-creator-frontend

#### 1.4.1 (2025-11-03)

#### Fixed
- SIMPL-20114


### sdtooling-validation-api-be

#### 1.14.0 (2025-09-04)

#### Added
- SIMPL-17313


### xsfc-advsearch-be

#### 1.15.0 (2025-09-26)

#### Added
- SIMPL-14978

#### Fixed
- fixed request logging issue

#### Changed
- simpl-data1-common updated to 1.5.0
- SIMPL-17497


### edc connector adapter

#### 1.7.0 (2025-09-26)
No changes.


### simpl-edc

#### 1.0.11 (2025-09-05)

#### Changed
- SIMPL-14812 fix sonar issues


### simpl-catalogue-client

#### 2.0.0 (2025-09-29)
No changes.


### simpl-files

#### 1.1.0 (2025-08-06)

#### Changed
- SIMPL-16125


### infrastructure-be

#### 1.0.12 (2025-11-04)

#### Added
- SIMPL-17406
- SIMPL-18457
- SIMPL-18321
- SIMPL-17398
- SIMPL-18996
- SIMPL-18326
- SIMPL-17427
- SIMPL-18269
- SIMPL-18022
- SIMPL-16171
- SIMPL-17408
- SIMPL-17996
- SIMPL-16167

#### Fixed
- SIMPL-20223
- SIMPL-14405
- SIMPL-13986
- SIMPL-14134
- SIMPL-13370

#### Changed
- SIMPL-18698
- SIMPL-17949
- SIMPL-13096
- SIMPL-17350
- SIMPL-17946
- SIMPL-16167
- SIMPL-14767
- SIMPL-15538


### infrastructure-fe

#### 1.1.0 (2025-10-21)

#### Added
- SIMPL-18325
- SIMPL-17399
- SIMPL-18320
- SIMPL-18319
- SIMPL-18313
- SIMPL-18315
- SIMPL-17405
- SIMPL-18230

#### Fixed
- Fix details label
- SIMPL-17405
- fixed cloudProvisionerTemplateId for not call when is ovh
- SIMPL-18320

#### Changed
- SIMPL-18325
- SIMPL-17405
- SIMPL-18315


### infrastructure-crossplane

#### 2.1.1 (2025-09-24)

#### Fixed
- SIMPL-17566


### Filebeat

#### 0.1.19 (2025-09-26)

#### Fixed
- SIMPL-18667 Fix cluster health alert

#### Changed
- SIMPL-18665 Create ILM policy for filebeat


### Contract Manager

#### 2.0.9 (2025-10-02)
No changes.