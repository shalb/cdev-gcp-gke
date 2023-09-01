# GCP-GKE

Cluster.dev uses [stack templates](https://docs.cluster.dev/stack-templates-overview/) to generate users' projects in a desired cloud. GCP-GKE is a stack template that creates and provisions Kubernetes clusters in GCP cloud by means of Google Kubernetes Engine (GKE).

In this repository you will find all information and samples necessary to start an EKS cluster on AWS with Cluster.dev. 

The resources to be created:

* VPC
* GKE Kubernetes cluster with addons:
  * cert-manager
  * ingress-nginx
  * external-secrets

## Prerequisites

1. Terraform version 13+
2. GCP account and project.
3. GCloud CLI installed and configured with your GCP account.
4. kubectl installed.
5. [Cluster.dev client installed](https://docs.cluster.dev/get-started-install/).

## Quick Start

1. Create GCO bucket for terraform backend
```
gcloud projects create PROJECT_ID
gcloud config set project PROJECT_ID
gsutil mb gs://gke-demo-state
```

2. Clone example project:
    ```
    git clone https://github.com/shalb/cdev-gcp-gke.git
    cd examples/
    ```
   Update project.yaml
   ```
name: demo-project
kind: Project
backend: default
variables:
  organization: my-organization
  project: PROJECT_ID
  region: us-west1
  state_bucket_name: gke-demo-state
  state_bucket_prefix: demo
```

3. Edit variables in the example's files, if necessary.
4. Run `cdev plan`
5. Run `cdev apply`

