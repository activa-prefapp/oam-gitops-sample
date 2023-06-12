# OAM gitops sample

[![Docker Build and Publish](https://github.com/activa-prefapp/oam-gitops-sample/actions/workflows/image-app.yml/badge.svg?branch=gitops-test)](https://github.com/activa-prefapp/oam-gitops-sample/actions/workflows/image-app.yml)

Repository for configuration, evaluation and development of gitops integrations with kubevela.

The purpose of this repository is to serve as an example and basis for a complete integration using github actions and gitops configurations in a kubernetes cluster.

_If you have access to the repository, you will be able to make modifications to the files in the __app/ directory__ in the __gitops-test branch__, you will be able to see the changes made when the update flow finishes in the url: https://oam-gitops-sample.activa.napptive.dev/_

_Remember that this is an example repository. To replicate this deployment in your cluster you will have to fulfill all the requirements and make the necessary modifications to namespace, secrets, host, certificates and other information._

## Repository structure and composition

```
oam-gitops-sample/
├── .github/
│   └── workflows/
├── app/
│   ├── Dockerfile
│   └── index.html
├── infrastructure/
│   ├── deployment-app.yaml
│   ├── ingres.yaml
│   └── service.yaml
```
- __.github/workflows/__ actions from github manifest for automatic build of application image and version change in manifest in infrastructure/.

- __app/__ dockerfile and html index of the test application.

- __infrastructure/__ manifest of the application to be deployed using gitops configurations in kubernetes cluster. You can define any type of manifest in this path, including an [OAM manifest](https://kubevela.io/docs/quick-start).

## infrastructure requirements

- Kubernetes cluster
- KubeVela
- FluxCD

This example has been deployed on the EKS cluster using some of the components configured in the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository.  

For the automatic deployment of the application in your cluster it will be necessary to configure and deploy a manifest as the one uploaded in the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository in the [gitops-application](https://github.com/activa-prefapp/oam-applications/tree/main/gitops-application) section, which will be in charge of monitoring the changes in the components declared in this repository and perform the modifications in the cluster.

For this example, some components have been configured as a deployable application. You can find all the necessary information for this deployment, configurations and requirements in the [aws-web-service](https://github.com/activa-prefapp/oam-applications/tree/main/applications/aws-web-service) directory of the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository.


## Kubernetes secret requirements

If your repository is private as well as your image registry, in order for these deployments to be automated correctly you need to have some secrets configured in the kubernetes cluster.

You need a secret to access the repository that will be used by the KubeVela FluxCD plugin to deploy the infrastructure defined in your git repository.

In this example the repository is located on GitHub, as you can see in the [original documentation](https://fluxcd.io/flux/components/source/gitrepositories/#basic-access-authentication), FluxCD is waiting for a basic authentication secret. As specified in the "[Bearer token authentication](https://fluxcd.io/flux/components/source/gitrepositories/#bearer-token-authentication)" section, in the note after the first paragraph, in this case the password field must store the github token with sufficient accesses and not the usual password.

The configuration of the secrets used for this deployment can be found in the organization's documentation repository, in the documentation requirements/gitops-config section.

## Deployment of KubeVela GitOps application

Once all the requirements defined above are met you can deploy a GitOps application with FluxCD in KubeVela as per [official documentation](https://kubevela.io/docs/end-user/gitops/fluxcd#preparing-the-configuration-repository). In this example the application that will be in charge of deploying the components in the infrastructure directory is defined in the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository in [oam-applications/gitops-application](https://github.com/activa-prefapp/oam-applications/tree/main/gitops-application).

To perform the deployment download the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository, go to the specified directory and deploy the application with the command:

```
vela up -f infra-gitops-app.yaml
```

You can check status with the command:

```
vela status infra-gitops -n gitops
```

Additionally you can check the status of the application with the [VelaUX](https://kubevela.io/docs/installation/standalone#3-install-velaux) dashboard if you have previously installed the addon.

## Uninstall

You can undo the changes and remove the application deployment with the following command:

```
vela delete infra-gitops -n gitops
```