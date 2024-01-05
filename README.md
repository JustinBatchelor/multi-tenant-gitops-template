This repository is an example of how an organization could use ArgoCD/Openshift Gitops to control the infrastructure of multiple clusters from a single repository. All new clusters created should used this repository to install internal infrastructure.

## Technologies

This repo makes use of the following:
- [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)/[Openshift Gitops](https://docs.openshift.com/container-platform/latest/cicd/gitops/understanding-openshift-gitops.html) - Cluster level service used to deploy the application
- [Kustomize](https://kustomize.io/) - Kubernetes native configuration management
- [Helm](https://helm.sh/) - Kubernetes package manager used for tempesting

## Folder Breakdown

``` sh
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
```

  - Inside every `env` folder, you'll discover a Kustomize Template designed for shared resources within the environment.



