# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2025-01-30

### Changed
- Installation of the Data Provider agent
- Data Provider secured access to Governance Authority to request security credentials (certificates) 
- API gateway, secured with certificates, for Data Provider communications with the Governance Authority for catalogue publishing and search
- Central Governance Authority catalogue search functionality, including a UI
- Catalogue Service Description tool, including a UI, to prepare datasets, infrastructure or combined bundled service - offering to be then signed and published securely to the Governance Authority central catalogue
- Basic service usage contract set-up with a Consumer DataSpace participant, requesting a Data Provider published service offering from the central catalogue
- Provisioning functionality for the Consumer access to a contracted dataset (direct download)
- Provisioning functionality for the Consumer access to a contracted VM infrastructure + dataset bundle (indirect access)
