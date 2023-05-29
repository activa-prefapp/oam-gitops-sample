# OAM gitops sample
Repository for configuration, evaluation and development of gitops integrations with kubevela.

The purpose of this repository is to serve as an example and basis for a complete integration using gitops and github actions configurations in a kubernetes cluster.

__If you have access to the repository, you will be able to make modifications to the files in the app/ directory, you will be able to see the changes made when the update flow finishes in the url: https://oam-gitops-sample.activa.napptive.dev/__

__Remember that this is an example repository. To replicate this deployment in your cluster you will need to comply with all the requirements and make the necessary modifications to namespace, secrets, host, certificates, and other information.__




## Repository structure and composition

- app/ dockerfile and html index of the test application

- infrastructure/ manifest of the application to be deployed using gitops configurations on kubernetes cluster

- github/workflows github manifest actions for automatic build of application image and version change in manifest on infrastructure/

## Infrastructure requirements

- Kubernetes cluster
- KubeVela
- FluxCD

This example has been deployed on the EKS cluster using some of the components configured in the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository.  

For the automatic deployment of the application in your cluster it will be necessary to configure and deploy a manifest as the one uploaded in the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository in the [gitops-application]() section, which will be in charge of monitoring the changes in the components declared in this repository and perform the modifications in the cluster.

For this example, another oam application has been configured as a deployable application. The manifest declared in the infrastructure/ directory is a modification of the one already configured in the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository. You can find all the necessary information for this deployment, configurations and requirements in the [aws-web-service](https://github.com/activa-prefapp/oam-applications/tree/main/applications/aws-web-service) directory of the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository.


## Kubernetes secret requirements

If your repository is private as well as your image registry, in order for these deployments to be automated correctly you need to have some secrets configured in the kubernetes cluster.

You need a secret to access the repository that will be used by the KubeVela FluxCD plugin to deploy the infrastructure defined in your git repository.

In this example the repository is located on GitHub, as you can see in the [original documentation](https://fluxcd.io/flux/components/source/gitrepositories/#basic-access-authentication), FluxCD is waiting for a basic authentication secret. As specified in the "[Bearer token authentication](https://fluxcd.io/flux/components/source/gitrepositories/#bearer-token-authentication)" section, in the note after the first paragraph, in this case the password field must store the github token with sufficient accesses and not the usual password.

The configuration of the secrets used for this deployment can be found in the organization's documentation repository, in the documentation requirements/gitops-config section.

## Deployment of KubeVela GitOps application

Once all the requirements defined above are met you can deploy a GitOps application with FluxCD in KubeVela as per [official documentation](https://kubevela.io/docs/end-user/gitops/fluxcd#preparing-the-configuration-repository). In this example the application that will be in charge of deploying the components in the infrastructure directory is defined in the [oam-applications](https://github.com/activa-prefapp/oam-applications) repository in [oam-applications/gitops-application]().

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