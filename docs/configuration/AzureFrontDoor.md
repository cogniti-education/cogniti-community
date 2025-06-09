# Azure Front Door Configuration

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Configuration Steps](#configuration-steps)
  - [1. Locate Azure Front Door](#1-locate-azure-front-door)
  - [2. Configure Custom Domain](#2-configure-custom-domain)
  - [3. Configure SSL Certificate](#3-configure-ssl-certificate)
    - [Option A: Azure Managed Certificate](#option-a-azure-managed-certificate-recommended)
    - [Option B: Bring Your Own Certificate](#option-b-bring-your-own-certificate)
  - [4. Update DNS Records](#4-update-dns-records)
  - [5. Verify Configuration](#5-verify-configuration)
- [Important Notes](#important-notes)
- [Troubleshooting](#troubleshooting)
- [Next Steps](#next-steps)
- [Support](#support)

## Overview

After deploying Cogniti through Azure Marketplace, you'll need to configure Azure Front Door with a custom domain and SSL certificate for production use.

## Prerequisites

- Completed Azure Marketplace deployment
- Access to the managed resource group
- Custom domain ownership
- SSL certificate (or ability to create one)

## Configuration Steps

### 1. Locate Azure Front Door

After deployment completes:

1. Navigate to the **Azure Portal**
2. Go to your **Managed Resource Group**
3. Locate the Azure Front Door resource named `afd-<uniqueString>`
    - The unique string is automatically generated during deployment

### 2. Configure Custom Domain

1. In the Azure Front Door resource, navigate to **Domains** in the left menu
2. Click **+ Add** to add a custom domain
3. Enter your custom domain details:
    - **Domain name**: Your custom domain (e.g., `app.yourdomain.com`)
    - **DNS management**: Choose your DNS provider
4. Follow the validation process:
    - Add the required CNAME record to your DNS provider
    - Wait for domain validation (typically 5-30 minutes)

### 3. Configure SSL Certificate

You have several options for SSL certificates:

#### Option A: Azure Managed Certificate (Recommended)

1. In the custom domain configuration
2. Select **AFD managed** for certificate management
3. Azure will automatically provision and renew the certificate

#### Option B: Bring Your Own Certificate

1. Upload your certificate to **Azure Key Vault**
2. In the custom domain configuration:
    - Select **Use my own certificate**
    - Choose the certificate from Key Vault
    - Grant Azure Front Door access to Key Vault if prompted

### 4. Update DNS Records

Add the following DNS record at your DNS provider:

```shell
Type: CNAME
Name: <your-subdomain>
Value: <afd-name>.azurefd.net
TTL: 3600
```

### 5. Verify Configuration

1. Wait for DNS propagation (5-30 minutes)
2. Test your custom domain:

```bash
curl https://your-custom-domain.com
```

3. Verify SSL certificate:

```bash
openssl s_client -connect your-custom-domain.com:443
```

## Important Notes

- **Redirect URL Update**: After configuring the custom domain, update your identity provider's redirect URL to:

```shell
https://your-custom-domain.com/login/oauth2/auth/{provider_name}
```

- **Propagation Time**: DNS changes can take up to 48 hours to fully propagate
- **Certificate Renewal**: Azure managed certificates auto-renew; custom certificates require manual renewal

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Domain validation fails | Verify CNAME record is correctly configured |
| SSL certificate errors | Check certificate validity and Key Vault permissions |
| 404 errors | Ensure routing rules are properly configured |

## Next Steps

- Configure WAF policies for enhanced security
- Set up monitoring and alerts
- Configure caching rules for better performance

## Support

For issues specific to Azure Front Door configuration, consult the [Azure Front Door documentation](https://docs.microsoft.com/azure/frontdoor/).

Raise [an issue](https://github.com/cogniti-education/cogniti-community/issues)
