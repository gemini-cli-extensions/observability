# Google Cloud Observability Extension for Gemini CLI ☁️

This extension connects [Gemini CLI](https://github.com/google-gemini/gemini-cli)
to Cloud Observability APIs with an 
[Model Context Protocol (MCP)](https://modelcontextprotocol.io/) server to 
search for logs, view metrics, return traces and view
error reports. It acts as a local bridge, translating natural language commands
from your CLI into the appropriate API calls to help you **understand, manage,
and troubleshoot** your Google Cloud environment.

> To learn more about the underlying services, see the official documentation:
>
> -   [Cloud Logging](https://cloud.google.com/logging/docs)
> -   [Cloud Monitoring](https://cloud.google.com/monitoring/docs)
> -   [Cloud Trace](https://cloud.google.com/trace/docs)
> -   [Error Reporting](https://cloud.google.com/error-reporting/docs)

## 🚀 Getting Started

### Prerequisites

-   [Node.js](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm):
    version 20 or higher
-   [gcloud CLI](https://cloud.google.com/sdk/docs/install)

## ✨ Install the extension

### Gemini CLI and Gemini Code Assist

To integrate this extension with Gemini CLI or Gemini Code Assist, run the setup
command below. This will install the MCP server as a
[Gemini CLI extension](https://github.com/google-gemini/gemini-cli/blob/main/docs/extension.md)
for the current user, making it available for all your projects. Refer to 
[Gemini CLI extension management](https://github.com/google-gemini/gemini-cli/blob/main/docs/extension.md#extension-management)
for uninstall, disable, enable, and update instructions.

```shell
gemini extensions install https://github.com/gemini-cli-extensions/observability
```

After the initialization process, you can verify that the observability-mcp server is
configured correctly by running the following command:

```
gemini mcp list

> ✓ observability: npx -y @google-cloud/observability-mcp (stdio) - Connected
```

### For other AI clients

See github.com/googleapis/gcloud-mcp for instructions on using the Cloud Observability MCP
server with other AI clients.

### Authentication

You need to authenticate twice: once for your user account and once for the
application itself.

```shell
# Authenticate your user account to the gcloud CLI
gcloud auth login

# Set up Application Default Credentials for the server.
# This allows the MCP server to securely make Google Cloud API calls on your behalf.
gcloud auth application-default login
```

#### Setting the Quota Project

All API requests made by this server require a Google Cloud project for billing
and API quota management. This is known as the "quota project". This project
will likely already be set in the gcloud CLI. The project selected as the quota
project will need to have the APIs you wish to use in Observability **enabled**
or you will see errors when attempting to use their related tools (e.g. you need
the Cloud Logging API enabled in the quota project to use the list_log_entries
tool).

If you need to control which project is used for quotas, run the following
command
(https://cloud.google.com/sdk/gcloud/reference/auth/application-default/set-quota-project):

```shell
# Set the project to be used for API quotas and billing by ADC
gcloud auth application-default set-quota-project YOUR_QUOTA_PROJECT_ID
```

This ensures that all API usage from this server is attributed to the correct
project.

## Usage

Once the server is configured, you can ask your MCP client natural language
questions about your Google Cloud environment. Here are a few examples:

-   **"Show me all logs with a severity of ERROR."**
-   **"What is the average CPU utilization for my GCE instances over the last
    hour?"**
-   **"List all traces from the past 30 minutes."**
-   **"Are there any new stack traces in my logs in the last day?"**

Your MCP client will translate these questions into the appropriate tool calls
to fetch the data from Google Cloud.

## Tools Reference

The server exposes the following tools:

| Service             | Tool                      | Description                |
| ------------------- | ------------------------- | -------------------------- |
| **Logging**         | `list_log_entries`        | Lists log entries from a project. |
|                     | `list_log_names`          | Lists log names from a project. |
|                     | `list_buckets`            | Lists log buckets from a project. |
|                     | `list_views`              | Lists log views from a project. |
|                     | `list_sinks`              | Lists log sinks from a project. |
|                     | `list_log_scopes`         | Lists log scopes from a project. |
| **Monitoring**      | `list_metric_descriptors` | Lists metric descriptors for a project. |
|                     | `list_time_series`        | Lists time series data for a given metric. |
|                     | `list_alert_policies`     | Lists the alert policies in a project. |
| **Trace**           | `list_traces`             | Searches for traces in a project. |
|                     | `get_trace`               | Gets a specific trace in a project. |
| **Error Reporting** | `list_group_stats`        | Lists the error groups for a project. |

## 🛡️ Important Notes

This repository is currently in prerelease and may see breaking changes until
the first stable release (v1.0).

This repository is providing a solution, not an officially supported Google
product. It may break when the MCP specification, other SDKs, or when other
solutions and products change.

## 👥 Contributing

Please read our [Contributing Guide](../../CONTRIBUTING.md) to get started.

## 📝 License

This project is licensed under the Apache 2.0 License - see the
[LICENSE](../../LICENSE) file for details.
