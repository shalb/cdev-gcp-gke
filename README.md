# GCP-GKE

Cluster.dev uses [stack templates](https://docs.cluster.dev/stack-templates-overview/) to generate users' projects in a desired cloud. GCP-GKE is a stack template that creates and provisions Kubernetes clusters in GCP cloud by means of Google Kubernetes Engine (GKE).

In this repository you will find all information and samples necessary to start an GKE cluster on GPC with Cluster.dev. 

The resources to be created:

* VPC
* GKE Kubernetes cluster with addons:
  * cert-manager
  * ingress-nginx
  * external-secrets

## Prerequisites

1. Terraform version >= 1.4
2. GCP account and project.
3. GCloud CLI installed and configured with your GCP account.
4. kubectl installed.
5. [Cluster.dev client installed](https://docs.cluster.dev/get-started-install/).

## Quick Start

1. Create GCO bucket for terraform backend
    ```
    gcloud projects create cdev-demo
    gcloud config set project cdev-demo
    gsutil mb gs://gke-demo-state
    ```

1.1 Create project if nessacerary
    ```
    gcloud projects create cdev-demo
    gcloud config set project cdev-demo
    ```

1.2 Authorize cdev/terraform to interact with GCP via SDK
    ```
    gcloud auth application-default login
    ```
1.3 Go to and enable the [Kubernetes Engine API](https://console.cloud.google.com/apis/api/container.googleapis.com/overview)

2. Clone example project:
    ```
    git clone https://github.com/shalb/cdev-gcp-gke.git
    cd examples/
    ```
3. Update project.yaml
    ```
    name: demo-project
    kind: Project
    backend: default
    variables:
      organization: my-organization
      project: cdev-demo
      region: us-west1
      state_bucket_name: gke-demo-state
      state_bucket_prefix: demo
    ```

4. Edit variables in the example's files, if necessary.
5. Run `cdev plan`
6. Run `cdev apply`
7. Connect to GKE cluster
    ```
    gcloud components install gke-gcloud-auth-plugin
    gcloud container clusters get-credentials demo-env-cluster --zone us-west1-a --project cdev-demo
    ```
