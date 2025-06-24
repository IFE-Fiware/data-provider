# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.0] - 2025-06-27
- Updated many components to implement Data Provider version 2.1.0.
- Remove component simpl-cli


### Simpl Cloud gateway (Tier 1)

#### 2.0.0 (2025-06-03)

#### Added
- Added Configuration Properties section
- Added the API Documentation section (merge request)


### Users Roles

#### 2.0.0 (2025-06-03)

#### Added
- Drop table identity attribute
- Added Configuration Properties section
- Added API Documentation section
- Maven goal to automatically add openapi from simpl-api-iaa
- Search roles by multiple names

#### Changed
- Removed deprecated API
- Removed ingress spec from chart
- Removed keycloak.client-to-realm-role-migration properties
- SIMPL-11765 Remove version v0 APIs fom IAA components


### SIMPL FE

#### 2.0.1 (2025-06-05)

#### Changed
- Fixed the display of assigned identity attributes and those for the user

#### 2.0.0 (2025-06-03)

#### Added
- SIMPL-10530
- SIMPL-10533
- SIMPL-11766
- SIMPL-8228
- SIMPL-8227

#### Changed
- SIMPL-8338


### TLS Gateway (Tier 2)

#### 2.0.0 (2025-06-03)
No changes.


### Authentication Provider

#### 2.0.0 (2025-06-03)

#### Added
- Maven goal to automatically add openapi from simpl-api-iaa
- SIMPL-12367 Integrate the reviewed APIs into the Keycloak Authenticator extension
- Added Configuration Properties section
- Added the API Documentation section

#### Changed
- Removed microservice.users-roles.url property
- Removed deprecated API
- CredentialInitializerImpl
- Removed ingress spec from chart
- SIMPL-11765 Remove version v0 APIs fom IAA components


### sd-creator-backend

#### 1.11.0 (2025-05-29)

#### Added
- SIMPL-12916 Extend
- SIMPL-10562 Added Resource Address config.
- SIMPL-12916 Extend
- SIMPL-12999 Config

#### Changed
- SIMPL-11995

#### Fixed
- SIMPL-2775


### sdtooling-validation-api-be

#### 1.10.0 (2025-05-29)

#### Added
- SIMPL-12098
- SIMPL-12291
- SIMPL-12999


### xsfc-advsearch-be

#### 1.10.0 (2025-06-04)

#### Added
- SIMPL-12999

#### Changed
- SIMPL-12117


### edc connector adapter

#### 1.2.0 (2025-05-29)

#### Added
- SIMPL-12727
- SIMPL-12999


### simpl-edc

#### 1.0.6 (2025-05-30)

#### Added
- SIMPL-12095. Add opentelemetry to simpl-edc

#### Changed
- SIMPL-6470
- SIMPL-12190 Implement changelog for simpl-edc


### simpl-catalogue-client

#### 1.2.1 (2025-05-09)

#### Added
- Classes to elements to aid testing
- Unit tests to increase code coverage to 80%


### simpl-files

#### 1.0.0 (2025-05-08)

#### Changed
- Improved Dockerfile for use simpluser


### infrastructure-be

#### 1.0.2 (2025-06-02)

#### Changed
- Changed authentication and project version upgrade


### infrastructure-fe

#### 0.2.4 (2025-05-27)

#### Added
- SIMPL-12091

#### Changed
- SIMPL-12091


### infrastructure-crossplane

#### 1.1.0 (2025-06-05)

#### Added
- SIMPL-13363

#### Changed
- SIMPL-11706 disable SA token automount


### Filebeat

#### 0.1.15 (2025-06-05)

#### Changed
- Edited dashboard for heartbeat
- SIMPL-13099
- SIMPL-12666 Removed unused fields
- Changed configuration because of change in business pods names.
- SIMPL-12666 Remove unused fields
