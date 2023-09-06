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

## Before you begin

1.  [Create or select a Google Cloud project.](https://console.cloud.google.com/project)
    ```
    gcloud projects create cdev-demo
    gcloud config set project cdev-demo
    ```

2.  [Enable billing for your project.](https://support.google.com/cloud/answer/6293499#enable-billing)

3.  [Enable the Google Kubernetes Engine API.](https://console.cloud.google.com/flows/enableapi?apiid=container,cloudresourcemanager.googleapis.com)

4. Enable Secret Manager
   ```
   gcloud services enable secretmanager.googleapis.com
   ```


## Quick Start
1. Clone example project:
    ```
    git clone https://github.com/shalb/cdev-gcp-gke.git
    cd examples/
    ```
2. Update project.yaml
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
3. Create GCO bucket for terraform backend
    ```
    gcloud projects create cdev-demo
    gcloud config set project cdev-demo
    gsutil mb gs://gke-demo-state
    ```
4. Edit variables in the example's files, if necessary.
5. Run `cdev plan`
6. Run `cdev apply`
7. Authorize cdev/terraform to interact with GCP via SDK
    ```
    gcloud auth application-default login
    ```
7. Connect to GKE cluster
    ```
    gcloud components install gke-gcloud-auth-plugin
    gcloud container clusters get-credentials demo-env-cluster --zone us-west1-a --project cdev-demo
    ```
8. Reset ArgoCD admin password
   install argocd cli 
    ```
    argocd account bcrypt --password somepass
    $2a$10$uMKkVIexTYNKLQ2F5yVE1OaKQIKEhFdd96qOYNMfjCjFLE2F9Af/u%

    kubectl -n argocd patch secret argocd-secret \
      -p '{"stringData": {
        "admin.password": "$2a$10$uMKkVIexTYNKLQ2F5yVE1OaKQIKEhFdd96qOYNMfjCjFLE2F9Af/u%",
        "admin.passwordMtime": "'$(date +%FT%T%Z)'"
      }}'
   ```


