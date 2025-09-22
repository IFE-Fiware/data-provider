# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.1.3] - 2025-09-22

- bitnamilegacy related fixes

## [2.1.2] - 2025-08-21
- Update simpl-edc package to version 1.0.10.

## [2.1.1] - 2025-07-22
- Update infrastructure-crossplane package to version 1.2.1.

## [2.1.0] - 2025-07-17
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

#### 1.5.2 (2025-05-14)

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

#### 1.12.2 (2025-07-03)

#### Changed
- SIMPL-14932
- 
#### 1.12.1 (2025-06-30)

#### Changed
- SIMPL-13505

#### 1.12.0 (2025-06-19)

#### Added
- SIMPL-13521 Added ArgoCD manifests

#### Changed
- error responses aligned to belgif problem model
- aligned to simpl-data1-common to version 1.1.0 to use belgif Problem
- SIMPL-2766 Deleted unused settings


### sdtooling-validation-api-be

#### 1.10.0 (2025-05-29)

#### Added
- SIMPL-12098
- SIMPL-12291
- SIMPL-12999


### xsfc-advsearch-be

#### 1.11.1 (2025-06-30)

#### Changed
- SIMPL-13505

#### 1.11.0 (2025-06-19)

#### Added
- SIMPL-13521 Added ArgoCD manifests.

#### Changed
- SIMPL-14205
- error responses aligned to belgif problem model
- spring upgraded from 3.4.4 to 3.4.5 to fix tomcat security issue on


### edc connector adapter

#### 1.3.0 (2025-06-19)

#### Added
- added new key in values.yaml to specify a different service name in open
- SIMPL-13521 Added ArgoCD manifests.

#### Changed
- error responses aligned to belgif problem model
- aligned to simpl-data1-common version 1.1.0 to support belgif problem

#### Fixed
- fixed RegistationControlle register() error handling for missing


### simpl-edc

#### 1.0.7 (2025-07-04)

#### Added
- SIMPL-14638 added logger

#### Changed
- SIMPL-14638 changed auth provider url
- SIMPL-14638 solved sonar issues
- SIMPL-14638 removed creds
- SIMPL-14638 update SIMPL-EDC with new version of the simpl-http library
- SIMPL-14638 updated connector-core to 1.1.5


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

#### 1.0.4 (2025-06-16)

#### Fixed
- Fixed container port issue

#### 1.0.3 (2025-06-16)

#### Added
- SIMPL-13370

#### Changed
- SIMPL-14134
- SIMPL-13510
- SIMPL-13370
- Changed authentication and project version upgrade
- SIMPL-13625
- SIMPL-12943
- SIMPL-12946
- SIMPL-12943

#### Fixed
- SIMPL-14134
- SIMPL-13370
- SIMPL-13625

#### 1.0.2 (2025-06-02)

#### Changed
- Changed authentication and project version upgrade


### infrastructure-fe

#### 0.2.6 (2025-06-18)
No changes.

#### 0.2.5 (2025-06-18)

#### Added
- SIMPL-1289
- SIMPL-11343

#### Changed
- SIMPL-1289
- SIMPL-13370

#### Fixed
- SIMPL-1289

#### 0.2.4 (2025-05-27)

#### Added
- SIMPL-12091

#### Changed
- SIMPL-12091


### infrastructure-crossplane

#### 1.2.0 (2025-06-24)

#### Added
- SIMPL-12229


### Filebeat

#### 0.1.15 (2025-06-05)

#### Changed
- Edited dashboard for heartbeat
- SIMPL-13099
- SIMPL-12666 Removed unused fields
- Changed configuration because of change in business pods names.
- SIMPL-12666 Remove unused fields
