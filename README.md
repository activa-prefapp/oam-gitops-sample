# OAM sample gitops
Repository for configuration, evaluation and development of gitops integrations with kubevela.

The goal of this repository is to serve as an example and a basis for a complete integration using gitops and github actions configurations on a kubernetes cluster.

## Repository structure and composition

- app/ dockerfile and html index of the test application

- oam-app/ application manifest to be deployed via gitops configurations on the kubernetes cluster

- github/workflows github manifest actions for automatic build of the application image and version change in the manifest in oam-app/

## Infrastructure requirements

- Kubernetes cluster
- KubeVela
- FluxCD

This example has been deployed on EKS cluster using some of the components configured in the [oam-applications]() repository.  

For the automatic deployment of the application in your cluster it will be necessary to configure and deploy a manifest as the one uploaded in the [oam-applications]() repository in the [gitops-application]() section, which will be in charge of monitoring the changes in the components declared in this repository and make the modifications in the cluster.

For this example, another oam application has been configured as a drop-down application. The manifest declared in the oam-app directory is a modification of the one already configured in the [oam-applications]() repository. You can find all the necessary information for this deployment, configurations and requirements in the [aws-web-service]() directory of the [oam-applications]() repository.


