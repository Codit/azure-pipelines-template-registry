# Step Templates

This folder contains all step templates that are available.

- **General**
  - [Change Pipeline Run Number](#change-pipeline-run-number)
  - [Replace Tokens](#replace-tokens)
- **Helm**
  - [Install Tool](#install-helm-tool)
  - [Add Helm Repository (Public)](#add-helm-repository-public)
  - [Add Helm Repository (Azure Container Registry)](#add-helm-repository-azure-container-registry)
  - [Update Helm Repository](#update-helm-repository)
  - [List Available Helm Charts in Repository](#list-available-helm-charts-in-repository)
  - [Lint Chart](#lint-helm-chart)
  - [Package Helm Chart](#package-helm-chart)
  - [Push Helm Chart](#push-helm-chart)

Learn more in the [official documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#step-templates).

## General

### Change Pipeline Run Number

Change Pipeline Run Number allows you to change the number of a pipeline run to align with your needs. This is typically useful with deployment pipelines where you want to use the version of the artifact that is being deployed with an additional incremental version.

For example - If your package pipeline generates a `1.10.0` you can use this step to change the run number of the deployment pipeline to `1.10.0-1` based on that version

**Example** - Changing the pipeline number to include the version of a container image tag:

```yaml
# Define to use default revision increment
name: $(Rev:rrr) # This is not used since we define it during the first stage

# Define a resource of the package pipeline triggering this deployment
resources:
  pipelines:
  - pipeline: package_container
    source: 'Contoso - Package Container Image'
    trigger:
     enabled: true
     branches:
      include:
        - main

# Define an Artifact.Version variable that takes the artifact version from the package pipeline
variables:
- name: Artifact.Version
  value: $(resources.pipeline.package_container.runName)

# Use the template to change the pipeline version
steps:
- template: steps/v1/change-pipeline-run-number.yml
  parameters:
    pipelineRunNumber: $(Artifact.Version)-$(Build.BuildNumber)
```

### Replace Tokens

Replace tokens allows you to easily replace all tokens in one or more files and requires the following [extension](https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens) to be installed.

**Example** - Automatically replace tokens in all files in a folder:

```yaml
- template: steps/v1/replace-tokens.yml
  parameters:
    directory: '$(Pipeline.Workspace)/package_azure/artifact/arm'
```

**Example** - Automatically replace tokens in a specific file in a folder:

```yaml
- template: steps/v1/replace-tokens.yml
  parameters:
    directory: '$(Pipeline.Workspace)/package_azure/artifact/arm'
    targetFiles: 'template.parameters.json'
```

## Helm

### Install Helm Tool

Install Tool allows you to easily install Helm in your pipeline.

**Example** - Install Helm v3.7.0:

```yaml
- template: steps/v1/helm/install-tool.yml
  parameters:
    helmVersion: '3.7.0'
```

### Add Helm Repository (Public)

Setup Public Repository allows you to add a given public Helm repository to your local configuration so you can interact with it.

**Example** - Add the KEDA Helm repository:

```yaml
- template: steps/v1/helm/setup-public-repo.yml
  parameters:
    repositoryName: 'kedacore'
    repositoryUrl: 'https://kedacore.github.io/charts'
```

### Add Helm Repository (Azure Container Registry)

Setup Azure Container Registry Repository allows you to add a given public Helm repository to your local configuration so you can interact with it.

**Example** - Add an Azure Container Registry repository:

```yaml
- template: steps/v1/helm/setup-acr-repo.yml
  parameters:
    # Name of the Azure Pipelines service connection to your Azure subscription
    # Learn more about Azure Service connections on https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops&tabs=yaml#azure-resource-manager-service-connection
    serviceConnectionName: 'my-azure-service-connection'
    
    # Name of your Azure Container Registry
    repositoryName: 'codito'
```

### Update Helm Repository

Update Helm Repository allows you to update a Helm repository so you can use the latest Helm charts in your pipeline.

**Example** - Update Helm repositories:

```yaml
- template: steps/v1/helm/update-repo.yml
```

### List Available Helm Charts in Repository

List Available Helm Charts allows you to get an overview of all the Helm charts that are available in a given Helm repository.

**Example** - List all available Helm charts in a repository:

```yaml
- template: steps/v1/helm/list-available-charts.yml
  parameters:
    repositoryName: 'example-repository'
```

### Lint Helm Chart

Lint Chart allows you to easily lint a Helm chart to verify the chart is valid.

**Example** - Lint a chart without a configuration file:

```yaml
- template: steps/v1/helm/lint-chart.yml
  parameters:
    chartName: 'example-chart'
```

**Example** - Lint a chart with a configuration file:

```yaml
- template: steps/v1/helm/lint-chart.yml
  parameters:
    chartName: 'example-chart'
    valuesFile: 'example-chart-config.yml'
```

### Package Helm Chart

Package Helm allows you to easily package a Helm chart and make it available to be deployed.

**Example** - Package 'example-chart' which is located in `charts/example-chart`:

```yaml
- template: steps/v1/helm/package-chart.yml
  parameters:
    chartName: 'example-chart'
    chartVersion: '1.1.0'
    appVersion: '1.0.0'
    destination: '$(Build.ArtifactStagingDirectory)/charts/'
    workingDir: charts
```

### Push Helm Chart

Easily push a packaged Helm chart to your Azure Container Registry so that it can be deployed.

**Example** - Push v1.1.0 of the example-chart Helm chart to Azure Container Registry:

```yaml
- template: steps/v1/helm/push-chart-to-acr.yml
  parameters:
    chartName: 'example-chart'
    chartVersion: '1.1.0'
    chartLocation: '$(Build.ArtifactStagingDirectory)/charts/'
    registryName: 'codito'

    # Name of the Azure Pipelines service connection to your Azure subscription
    # Learn more about Azure Service connections on https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops&tabs=yaml#azure-resource-manager-service-connection
    registrySubscription: 'my-azure-service-connection'
```
