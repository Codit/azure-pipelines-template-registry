# Contribution Guide

## Adding a new template

Adding a new template is easy:

1. Create a new file in the folder that aligns with the scope of the template (`stage`, `job` or `step`) with `.yml` extension.
2. Provide the template functionality that you want to contribute
3. Document the new template in the README of the folder ([docs](#documenting-a-template))

## Guidance

### Documenting a template

When documenting a template, make sure that people understand what the template does and how it can be used by providing samples.

If there are pre-requistes for the template, such as installing a 3rd party extension, make sure to document them.

### Using parameters

When using parameters, we recommend to specify which ones are required and use defaults where suitable.

If is recommended to use YAML comments to provide more context, but we prefer clear parameter names where possible.

Learn more in the [documentation](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/templates?view=azure-devops#passing-parameters).
