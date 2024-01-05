This repository is an example of how an organization could use ArgoCD/Openshift Gitops to control the infrastructure of multiple clusters from a single repository. All new clusters created should used this repository to install internal infrastructure.

## Technologies

This repo makes use of the following:
- [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)/[Openshift Gitops](https://docs.openshift.com/container-platform/latest/cicd/gitops/understanding-openshift-gitops.html) - Cluster level service used to deploy the application
- [Kustomize](https://kustomize.io/) - Kubernetes native configuration management
- [Helm](https://helm.sh/) - Kubernetes package manager used for tempesting

## Folder Breakdown

``` sh
  clusters
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

  - Inside every sub directory of `clusters`, you'll discover a Kustomize Template designed for shared resources within the environment.
  - A list of `yaml` files defining the various resources required to deploy various infrastructure components to that specific cluster

## Add components

In order to add resources managed by gitops you will need to

  1. add a new yaml file defining the resources you want deployed.  

***Example to install the open-cluster-management operator to the utility cluster***
  
  `clusters/utility/advanced-cluster-management.yaml`

  ```
apiVersion: v1
kind: Namespace
metadata:
  name: open-cluster-management
  annotations:
    openshift.io/display-name: "Advanced Cluster Management for Kubernetes"
  labels:
    openshift.io/cluster-monitoring: 'true'
---
apiVersion: operators.coreos.com/v1
kind: OperatorGroup
metadata:
  name: advanced-cluster-management-group
  namespace: open-cluster-management
spec:
  targetNamespaces:
  - open-cluster-management
---
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: advanced-cluster-management
  namespace: open-cluster-management
spec:
  channel: release-2.9 
  installPlanApproval: Automatic
  name: advanced-cluster-management
  source: redhat-operators
  sourceNamespace: openshift-marketplace
  startingCSV: advanced-cluster-management.v2.9.1 # Replace with the specific version you need
  ``` 

  2. update the `kustomization.yaml` file to reference the new file you created

  ```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

resources:
  - advanced-cluster-management.yaml
  ```

## Remove components

In order to remove components that are managed via gitops you will need to 

1. remove the reference to the file from the `clusters` corresponding sub directory `kustomization.yaml` file.

***It is also recommended that you remove the file from the repository***



