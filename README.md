# DHIS2 with PostGIS on Kubernetes

Welcome to the DHIS2 Kubernetes deployment repository! This repository contains all the configurations you need to set up and manage DHIS2 on Kubernetes, backed by a PostgreSQL database with the PostGIS extension for geospatial data support. This setup offers a cloud-native, scalable, and secure environment for running DHIS2.

## About DHIS2

[DHIS2](https://dhis2.org) (District Health Information Software 2) is an open-source platform designed to capture, manage, and analyze health data at both individual and aggregate levels. Originally developed by the Health Information Systems Program (HISP) at the University of Oslo, DHIS2 is now used by governments, NGOs, and other organizations across 70+ countries, making it one of the world’s largest health information management systems.

### Core Functionalities

- **Data Collection and Aggregation**: Allows users to capture and consolidate health-related data from various sources, including health facilities, mobile devices, and community reports.
- **Real-Time Data Analysis**: Provides powerful analytics tools for creating charts, pivot tables, dashboards, and GIS maps, helping stakeholders make data-driven decisions.
- **Geospatial Analysis**: With support for PostGIS, DHIS2 enables geolocation-based tracking, which is vital for managing resources, tracking disease outbreaks, and planning healthcare coverage.
- **Metadata and Hierarchies**: Supports flexible metadata hierarchies, making it adaptable to different administrative levels (e.g., national, regional, district).
- **Event and Case Tracking**: Tracks individual health events, enabling better patient and case management. This is especially useful for infectious disease surveillance, immunization programs, and chronic disease management.
- **Program Management**: Facilitates the management of health programs by enabling configuration of program stages, workflows, and reports tailored to specific health objectives.
- **Interoperability**: Compatible with standards like HL7 FHIR and integrates with other health information systems, enabling data exchange between different healthcare systems.

### Common Use Cases

- **Health Program Management**: DHIS2 is used to manage immunization, maternal and child health, HIV, TB, and other health programs.
- **Disease Surveillance**: Real-time data collection and geospatial analysis are used to track infectious disease outbreaks, manage vaccinations, and monitor chronic health conditions.
- **Resource Planning and Allocation**: Public health organizations leverage DHIS2’s analytics to allocate resources efficiently based on population health needs.
- **Reporting for Funders and Stakeholders**: DHIS2 provides comprehensive reporting tools, enabling NGOs and government agencies to report on health outcomes and compliance to funders.

### Accessibility in DHIS2

DHIS2 is built with accessibility in mind, aiming to provide usability across a wide range of contexts:
- **Multi-language Support**: Users can choose their preferred language, making DHIS2 adaptable for global use.
- **Offline Data Capture**: Allows health workers in remote areas to collect data offline and sync it when connectivity is restored.
- **WCAG Compliance**: Designed following web accessibility standards, DHIS2 is usable for people with disabilities, ensuring inclusivity.

## About Cloud Native PostgreSQL (CNPG) with PostGIS

For geospatial data handling, this deployment uses **CNPG** (Cloud Native PostgreSQL) with the **PostGIS** extension. PostGIS extends PostgreSQL to store geographic data types, making it essential for DHIS2’s mapping and spatial functionalities.

### Key Features of CNPG with PostGIS

- **Kubernetes-native Management**: CNPG is designed to run seamlessly within Kubernetes, providing better scalability, resilience, and easier management through native Kubernetes capabilities.
- **High Availability**: CNPG is configured for replication and failover, ensuring continuous availability and reliability.
- **Geospatial Data Support with PostGIS**: PostGIS enables the storage, retrieval, and analysis of spatial data, necessary for DHIS2’s geolocation features and GIS mapping.

## Repository Structure

```plaintext
.
├── README.md                     # Documentation (this file)
├── Taskfile.yaml                 # Automates setup and deployment tasks
├── infrastructure/               # GCP resources configurations
│   ├── api.yaml                  # API Gateway setup
│   ├── bucket.yaml               # Storage bucket configuration
│   └── ...                       # Other GCP resource configurations
└── k8s/                          # Kubernetes configurations
    ├── cert-manager/             # Manages TLS certificates for secure communication
    ├── cnpg-system/              # Configures PostgreSQL + PostGIS
    ├── cnrm-system/              # Google Cloud Platform resources managed by Kubernetes
    ├── configconnector-operator-system/ # Config Connector Operator for GCP
    ├── dhis2/                    # DHIS2 application deployment files
    ├── flux-system/              # GitOps setup with Flux for automated syncing
    └── istio-ingress/            # Ingress and traffic management using Istio
```
## Components

### cert-manager
Handles TLS certificates to secure communication both within the cluster and externally by managing and renewing SSL/TLS certificates automatically.

### cnpg-system
This directory includes the configurations to set up Cloud Native PostgreSQL with PostGIS, providing the geospatial database layer that DHIS2 requires for mapping and spatial analysis.

### cnrm-system
Manages Google Cloud Platform (GCP) resources through Kubernetes configurations, allowing a seamless Kubernetes-native approach to provisioning and managing GCP resources.

### configconnector-operator-system
Deploys the Config Connector operator, which synchronizes GCP resources with Kubernetes configurations, ensuring cloud infrastructure aligns with Kubernetes changes.

### dhis2
This directory contains the deployment configurations for DHIS2, including settings for connecting to the CNPG database and scaling the application within Kubernetes.

### flux-system
Uses Flux to implement GitOps, enabling automatic application of any configuration changes from this Git repository to the Kubernetes cluster.

### istio-ingress
Configures Istio as the Ingress controller for managing external access to services within the cluster. Istio also uses mTLS to secure service-to-service communication.

## Getting Started

### Prerequisites

- A Kubernetes cluster, accessible with `kubectl`
- Persistent storage for data (e.g., dynamic provisioner or PersistentVolumes)
- Google Cloud Platform account with Config Connector permissions
- Flux installed for GitOps configuration management
- Domain name and SSL/TLS setup if external access is required

### Deployment Instructions

1. Clone this repository and navigate to the project directory.
2. Apply Infrastructure Configurations to set up GCP resources and other required infrastructure.
3. Deploy DHIS2 on Kubernetes.
4. Enable GitOps with Flux to ensure continuous synchronization with the Git repository.

## Configuration Overview

- **Database Connection**: Environment variables (`DB_USERNAME`, `DB_PASSWORD`, `DB_HOST`) are managed through Kubernetes secrets, ensuring secure database connectivity for DHIS2.
- **ConfigMaps and Secrets**: Store non-sensitive configuration data and sensitive information (like database credentials) securely.
- **Istio Ingress**: Ensures secure communication and traffic routing with mutual TLS (mTLS) and routing rules for DHIS2 services.

## Backup and Restore

Automated backups are managed using Kubernetes CronJobs, while restore scripts in `k8s/dhis2/postgis/restore` allow recovery from backups or object storage.

## Flux GitOps Setup

Flux enables continuous synchronization between this Git repository and the cluster, ensuring any configuration updates are automatically deployed.

To manually sync Flux, apply the Flux sync configuration.
