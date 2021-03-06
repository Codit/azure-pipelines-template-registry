# Azure Pipeline Template Registry

A registry of YAML templates for Azure Pipelines.

![Codit logo](./media/logo.png)

## Using the template registry

You can easily use the template registry in your own pipelines:

```yaml
resources:
  repositories:
  - type: github
    # Name of this GitHub repository
    name: Codit/azure-pipelines-template-registry
    # Name of repository to refer to in your pipeline
    repository: template-registry
    # Name of your GitHub service connection in Azure DevOps
    # See https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops&tabs=yaml#github-service-connection
    endpoint: Codit

steps:
  # Specify the path to the template to use suffixed
  # with '@' and the name of your repository ID above
- template: steps/v1/replace-tokens.yml@template-registry
  parameters:
    directory: '$(Pipeline.Workspace)/package_azure/artifact/arm'
```

Learn more about adding repository resources in the [official documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=example%2Cparameter-schema#type).

## Registry structure

The repo is structured as follows:

- `stages` - Contains all templates for stages
- `jobs` - Contains all templates for jobs
- `steps` - Contains all templates for steps

Every folder can should have sub-folders that is used for versioning and optionally provide more structure.

For example, `steps/v1/helm/`provides all steps related to Helm for v1.

## Contributing

Learn how to contribute new templates in our [contribution guide](CONTRIBUTING.md).

## License

This is licensed under The MIT License (MIT). Which means that you can use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the web application. But you always need to state that Codit is the original author of this web application.

Read the full license [here](https://github.com/Codit/azure-pipelines-template-registry/blob/main/LICENSE).
