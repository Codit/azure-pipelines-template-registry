# Step Templates

This folder contains all step templates that are available.

Learn more in the [official documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#step-templates).

## Overview

- [Replace Tokens](#replace-tokens)

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
