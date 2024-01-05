This repository is an example of how an organization could use ArgoCD/Openshift Gitops to control the infrastructure of multiple clusters from a single repository. All new clusters created should used this repository to install internal infrastructure.

## Technologies

This repo makes use of the following:
- [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)/[Openshift Gitops](https://docs.openshift.com/container-platform/latest/cicd/gitops/understanding-openshift-gitops.html) - Cluster level service used to deploy the application
- [Kustomize](https://kustomize.io/) - Kubernetes native configuration management
- [Helm](https://helm.sh/) - Kubernetes package manager used for tempesting

## Folder Breakdown

``` sh
  cluster (1)
  ├── example-cluster-1
  │   ├── kustomization.yml
  env
  ├── bcp
  │   ├── kustomization.yml
  ├── dev2
  │   ├── kustomization.yml
  ├── prod
  │   ├── kustomization.yml
  ├── uat
  │   ├── kustomization.yml
  ├── utility
  │   ├── kustomization.yml
  manifest (3)
  ├── argo (3a)
  └── charts (3b)
```

  1. Within each `cluster` folder, you'll find a Kustomize Template that represents a distinct cluster. These templates inherit from an `env` template.
  2. Inside every `env` folder, you'll discover a Kustomize Template designed for shared resources within the environment.
  3. The Manifest folder contains templates for deploying various architectural features.
       - Contains a set of Kustomize Templates for deploying the Argo `Application` pointing to the appropriate Helm chart
       - Contains a set of Helm Charts that represent the different components installed onto the cluster

## Cluster Management

This section will talk about managing specific clusters using this repository.

### Adding a New Cluster

Once the inital install of the cluster has been completed use the following instructions for installation of the inital infrastructure

#### Create Cluster Folder

Create a new folder under the `/cluster` folder with an appropriately descriptive name. And a `kustomization.yml` file with the following content

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - <resource.yaml>
  - <resource1.yaml> ...

```

