# Azure Pipeline Template Registry

A registry of YAML templates for Azure Pipelines.

![Codit logo](./media/logo.png)

## Using the template registry

You can easily use all of the templates available in this repo by using a [`repository` resource](https://docs.microsoft.com/en-us/azure/devops/pipelines/yaml-schema?view=azure-devops&tabs=schema%2Cparameter-schema#repository-resource).

```yaml
```

## Registry structure

The repo is structured as follows:

- `stages` - Contains all templates for stages
- `jobs` - Contains all templates for jobs
- `steps` - Contains all templates for steps

Every folder can have sub-folders to provide more structure, for example `steps/helm/` that provides all steps related to Helm.

## Contributing

Learn how to contribute new templates in our [contribution guide](CONTRIBUTING.md).

## License

This is licensed under The MIT License (MIT). Which means that you can use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the web application. But you always need to state that Codit is the original author of this web application.

Read the full license [here](https://github.com/Codit/azure-pipelines-template-registry/blob/main/LICENSE).
