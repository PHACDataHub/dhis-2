# Kubernetes Cluster Configuration for DHIS2 with PostGIS

This repository contains the necessary Kubernetes configurations to set up and manage the DHIS2 application with a CNPG PostGIS database within a Kubernetes cluster. Below are the components and their respective purposes within our infrastructure.

## Components Overview

### cert-manager
Manages TLS certificates to ensure secure communication within the cluster and for inbound traffic.

### cnpg-system
Houses the configurations for Containerized PostgreSQL with PostGIS, providing spatial database capabilities for DHIS2.

### cnrm-system
Manages Google Cloud Platform (GCP) resources via Kubernetes configurations.

### configconnector-operator-system
Operates the Config Connector to keep GCP resources and Kubernetes configurations in sync.

### dhis2
Contains deployment configurations for the DHIS2 application, designed to connect with the PostGIS database.

### flux-system
Utilizes Flux to implement GitOps, automatically applying changes from a Git repository to the cluster.

### istio-ingress
Configures Istio as an Ingress controller to manage external access to services in the cluster.

## Flux Configuration

Flux is used to keep our Kubernetes cluster state synchronized with the configuration defined in our git repository. The `sync.yaml` file is a key part of this setup.

### sync.yaml Overview
The `sync.yaml` file contains the Flux GitRepository and Kustomization resources, defining where Flux should look for the configuration files and how they should be applied to the cluster.

### Applying sync.yaml
To apply the `sync.yaml` file and enable Flux syncing:

```bash
kubectl apply -f path/to/sync.yaml


### Deployment Process

1. **DHIS2 Kubernetes Deployment**: We deploy DHIS2 as a set of replicated pods within the Kubernetes cluster to ensure high availability. The deployment configuration specifies the DHIS2 Docker image and sets the necessary environment variables for database connectivity.

   - **Database Connectivity**: Environment variables `DB_USERNAME` and `DB_PASSWORD` are set through Kubernetes secrets to maintain security. These credentials are used by DHIS2 to connect to the CNPG PostGIS database.

2. **CNPG Database Setup**: The CNPG system provides a PostgreSQL database with the PostGIS extension. It includes its own set of Kubernetes objects like Deployments, Services, and PersistentVolumeClaims to ensure data persistence.

   - **Integration with DHIS2**: DHIS2 connects to the CNPG database using a JDBC URL specified in the environment variables or a configuration file. This URL includes the service name of the CNPG database as the hostname, which Kubernetes resolves internally.

3. **ConfigMaps and Secrets**: Configuration Maps (ConfigMaps) and Secrets store configuration data and sensitive information respectively. They are mounted into DHIS2 pods as environment variables or files.

   - **ConfigMaps**: May include configurations like system settings or overrides.
   - **Secrets**: Store sensitive information like database credentials, used by both DHIS2 and CNPG deployments.

# cnpg-system


The `cnpg-system` directory contains the necessary configurations for deploying a containerized PostgreSQL database with the PostGIS extension, which enables spatial and geographic data support. This setup is tailored for Kubernetes environments, allowing for scalable and managed database services.

## Prerequisites

Before deploying the CNPG system, the following prerequisites must be met:

- A Kubernetes cluster set up and accessible.
- PersistentVolumes (PV) provisioned or a dynamic storage provisioner configured.
- Necessary RBAC roles and RoleBindings set up for resource access within the cluster.

## Deployment

The deployment process involves several Kubernetes manifests that set up the CNPG system:

- **PersistentVolumeClaims (PVCs)**: For data storage and persistence.
- **Deployments**: To run the PostgreSQL with PostGIS containers.
- **Services**: To expose the database within the cluster.
