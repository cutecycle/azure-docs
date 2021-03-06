---
title: API support in Azure Static Web Apps with Azure Functions
description: Learn what API features Azure Static Web Apps supports
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic:  conceptual
ms.date: 05/18/2020
ms.author: cshoe
---

# API support in Azure Static Web Apps with Azure Functions

Azure Static Web Apps provides serverless API endpoints via [Azure Functions](../azure-functions/functions-overview.md). By using Azure Functions, APIs dynamically scale based on demand, and include the following features:

- **Integrated security** with direct access to user [authentication and role-based authorization](user-information.md) data.
- **Seamless routing** that makes the _api_ route available to the web app securely without requiring custom CORS rules.

Azure Static Web Apps APIs are supported by two possible configurations:

- **Managed functions**:  By default, the API of a static web app is an Azure Functions application managed and deployed by Azure Static Web Apps associated with some restrictions.

- **Bring your own functions**: Optionally, you can [provide an existing Azure Functions application](functions-bring-your-own.md) of any plan type, which is accompanied by all the features of Azure Functions. With this configuration, you're responsible to handle a separate deployment for the Functions app.

The following table contrasts the differences between using managed and existing functions.

| Feature | Managed Functions | Bring your own Functions |
| --- | --- | --- |
| Access to Azure Functions triggers | Http only | [All](../azure-functions/functions-triggers-bindings.md#supported-bindings) |
| Supported runtimes | Node.js<br>.NET<br>Python | [All](../azure-functions/supported-languages.md#languages-by-runtime-version) |
| Supported Azure Functions [hosting plans](../azure-functions/functions-scale.md) | Consumption | Consumption<br>Premium<br>Dedicated |
| [Integrated security](user-information.md) with direct access to user authentication and role-based authorization data | ✔ | ✔ |
| [Routing integration](./configuration.md?#routes) that makes the _api_ route available to the web app securely without requiring custom CORS rules. | ✔ | ✔ |
| [Durable Functions](../azure-functions/durable/durable-functions-overview.md) programming model | | ✔ |
| [Managed identity](../app-service/overview-managed-identity.md) | | ✔ |
| [Azure App Service Authentication and Authorization](../app-service/configure-authentication-provider-aad.md) token management | | ✔ |
| API functions available outside Azure Static Web Apps |  | ✔ |

## Configuration

API endpoints are available to the web app through the _api_ route. While this route is fixed, you have control over the folder and project where you locate the associated Azure Functions app. You can change this location by [editing the workflow YAML file](github-actions-workflow.md#build-and-deploy) located in your repository's _.github/workflows_ folder.

## Troubleshooting and logs

Logs are only available if you add [Application Insights](monitor.md) to your static web app.

## Constraints

- The API route prefix must be _api_.

- Route rules for API functions only support [redirects](configuration.md#defining-routes) and [securing routes with roles](configuration.md#securing-routes-with-roles).

- Some application settings are managed by the service, therefore the following prefixes are reserved by the runtime:

  - *APPSETTING\_, AZUREBLOBSTORAGE\_, AZUREFILESSTORAGE\_, AZURE_FUNCTION\_, CONTAINER\_, DIAGNOSTICS\_, DOCKER\_, FUNCTIONS\_, IDENTITY\_, MACHINEKEY\_, MAINSITE\_, MSDEPLOY\_, SCMSITE\_, SCM\_, WEBSITES\_, WEBSITE\_, WEBSOCKET\_, AzureWeb*

- Managed identity and Azure Key Vault references require the [Standard plan](plans.md).

Managed functions and bring your own functions each come with a different set of constraints.

- For managed functions:
  - Triggers are limited to [HTTP](../azure-functions/functions-bindings-http-webhook.md).

- For bring you own functions:
  - The Azure Functions app must either be in Node.js 12, .NET Core 3.1, or Python 3.8.
  - You are responsible to manage deployment of the Functions app.

## Next steps

> [!div class="nextstepaction"]
> [Add an API](add-api.md)
