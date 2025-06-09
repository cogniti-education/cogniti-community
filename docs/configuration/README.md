# Azure Marketplace Configuration

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Configuration Steps](#configuration-steps)
  - [1. Marketplace Deployment](#1-marketplace-deployment)
  - [2. Deployment Configuration](#2-deployment-configuration)
    - [Project Details](#project-details)
    - [Instance Details](#instance-details)
    - [Application Settings](#application-settings)
    - [Identity Provider Configuration](#identity-provider-configuration)
    - [Cogniti Administrator](#cogniti-administrator)
  - [3. Post-Deployment Verification](#3-post-deployment-verification)
- [Next Steps](#next-steps)

## Overview

This guide provides instructions for configuring and deploying Cogniti through Azure Marketplace.

## Prerequisites

- Active Azure subscription
- Azure CLI installed (optional)
- Required permissions to create resources

## Configuration Steps

### 1. Marketplace Deployment

1. Navigate to [Azure Marketplace](https://azuremarketplace.microsoft.com/)
2. Search for **Cogniti**
3. Click **Create**
4. Follow the deployment wizard

### 2. Deployment Configuration

#### Project Details

| Parameter | Description | Example |
|-----------|-------------|---------|
| Subscription | Azure subscription to manage deployed resources and costs | My Subscription |
| Resource Group | Organize and manage all your resources | Create new or select existing |

#### Instance Details

| Parameter | Description | Example |
|-----------|-------------|---------|
| Location | Azure region for deployment | Australia East |
| Application Name | Name for your managed application | - |
| Managed Resource Group | Holds all resources required by the managed application | Consumer has limited access |

#### Application Settings

| Parameter | Description | Options |
|-----------|-------------|---------|
| Container Apps SKU | Choose workload size | Small, Medium, Large, Consumption |
| Enable Purge Protection | Prevent accidental deletion of Key Vault and App Configuration Store | Enable/Disable |

> **Note**: For Container Apps pricing, see [Azure Container Apps Pricing](https://azure.microsoft.com/en-us/pricing/details/container-apps/)

#### Identity Provider Configuration

Configure the initial identity provider for user authentication:

| Parameter | Description | Example |
|-----------|-------------|---------|
| Provider Name | Identity provider name | Microsoft |
| Authority URL | OAuth authority endpoint | - |
| Client ID | Application client ID | - |
| Client Secret | Application client secret | - |
| Email Claim | Email claim field | preferred_username |

> **Important**: Configure the identity provider redirect URL to: `https://{cogniti-app-url}/login/oauth2/auth/{provider_name}`

#### Cogniti Administrator

| Parameter | Description | Example |
|-----------|-------------|---------|
| Administrator Email | Email for initial admin access | <admin@cogniti.ai> |

> **Note**: Email must be registered with the configured identity provider

### 3. Post-Deployment Verification

```bash
# Verify deployment
az resource list --resource-group <your-resource-group>

# Access application
https://<app-name>.azurewebsites.net
```

## Next Steps

- [Configure Azure Front Door](./AzureFrontDoor.md)
- [Application Configuration](./AppConfig.md)
