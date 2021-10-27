# Step Templates

This folder contains all step templates that are available.

Learn more in the [official documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#step-templates).

## Overview

- [Change Pipeline Run Number](#change-pipeline-run-number)
- [Replace Tokens](#replace-tokens)

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
- template: steps/change-pipeline-run-number.yml
  parameters:
    pipelineNumber: $(Artifact.Version)-$(Build.BuildNumber)
```

### Replace Tokens

Replace tokens allows you to easily replace all tokens in one or more files and requires the following [extension](https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens) to be installed.

**Example** - Automatically replace tokens in all files in a folder:

```yaml
- template: steps/replace-tokens.yml
  parameters:
    directory: '$(Pipeline.Workspace)/package_azure/artifact/arm'
```

**Example** - Automatically replace tokens in a specific file in a folder:

```yaml
- template: steps/replace-tokens.yml
  parameters:
    directory: '$(Pipeline.Workspace)/package_azure/artifact/arm'
    targetFiles: 'template.parameters.json'
```
