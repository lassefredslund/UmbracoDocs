---
description: >-
  Details an integration available for Dynamics, built and maintained by Umbraco HQ.
---

# Dynamics

This integration provides a form picker and rendering component for forms managed within a Microsoft Dynamics 365 Marketing instance.

## Package Links

- [NuGet install](https://www.nuget.org/packages/Umbraco.Cms.Integrations.Crm.Dynamics)
- [Source code](https://github.com/umbraco/Umbraco.Cms.Integrations/tree/main/src/Umbraco.Cms.Integrations.Crm.Dynamics)
- [Umbraco marketplace listing](https://marketplace.umbraco.com/package/umbraco.cms.integrations.crm.dynamics)

## Prerequisites

Required minimum versions of Umbraco CMS:

- 8.4.0 for version 8
- 10.1.0 for version 10

## Authentication

The package uses the OAuth protocol for authentication.

## Additional Configuration

To connect to your Dynamics 365 instance, the following configuration is required:

For Umbraco 8:

```xml
  <appSettings>
    ...
    <add key="Umbraco.Cms.Integrations.Crm.Dynamics.HostUrl" value="https://[INSTANCE]/api.crm4.dynamics.com/" />
    <add key="Umbraco.Cms.Integrations.Crm.Dynamics.ApiPath" value="api/data/v9.2/" />
    ...
  </appSettings>
```

For Umbraco 10+:

```json
  "Umbraco": {
    "Integrations": {
        "Crm": {
          "Dynamics": {
            "Settings": {
              "HostUrl": "https://[INSTANCE].crm4.dynamics.com/",
              "ApiPath": "api/data/v9.2/"
            }
          }
        }
      }
    }
```

The above settings are for demonstration purpose. They might change depending on your personalized instance Web API.

## Backoffice usage

To use the form picker, a new Data Type needs to be created based on the Dynamics Form Picker property editor.

The settings in `Web.config`/`appsettings.json` will be used for sending authorization and data requests to the Dynamics API, through the _0Auth Proxy for Umbraco Integrations_ or directly.

The _Connect_ button prompts the user with the Microsoft authorization window, which after a successful authentication will send the authorization code back.

The retrieved access token will be saved into the database and used for future requests.

_Revoke_ action will remove the access token from the database and the authorization process will need to be repeated.

## Front-end rendering

A strongly typed model will be generated by the property value converter. An HTML helper is available to render the form on the front-end.

Ensure your template has a reference to the following using statement:

```csharp
@using Umbraco.Cms.Integrations.Crm.Dynamics.Helpers;
```

Assuming a property based on the created Data Type, with alias `dynamicsForm` has been created, render the form using:

```csharp
@Html.RenderDynamicsForm(Model.DynamicsForm)
```

The selected form is embedded either through an _iframe_, or by using scripts.