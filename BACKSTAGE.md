# Backstage Integration Guide

This document explains how to integrate the Home Loan Check service with Backstage.

## Overview

The Home Loan Check service is a web-based application that helps users analyze home loan options and make informed decisions. This guide will help you register and configure this service in your Backstage instance.

## Prerequisites

- A running Backstage instance
- Access to configure the Backstage catalog
- The service should be deployed and accessible via a URL (for production use)

## Files Included

### `catalog-info.yaml`

This is the main Backstage entity descriptor file that defines the service component. It includes:

- **Component Metadata**: Name, description, tags, and annotations
- **Component Spec**: Type (website), lifecycle, owner, system, and domain
- **Location Reference**: Points to this catalog-info.yaml file

## Configuration Steps

### 1. Update the catalog-info.yaml File

Before registering the component, update the following fields in `catalog-info.yaml`:

#### Required Updates:

- **owner**: Change `platform-team` to your actual team name (e.g., `finance-team`, `product-team`)
- **system**: Change `financial-services` to your actual system name
- **domain**: Change `finance` to your actual domain name

#### Optional Updates:

- **annotations.backstage.io/view-url**: Add the URL where your service is deployed
  ```yaml
  backstage.io/view-url: https://your-domain.com/home-loan-check
  ```

- **annotations.backstage.io/edit-url**: Add the URL to your source code repository
  ```yaml
  backstage.io/edit-url: https://github.com/your-org/home_loan_check
  ```

- **annotations.github.com/project-slug**: Add your GitHub organization and repository
  ```yaml
  github.com/project-slug: your-org/home_loan_check
  ```

- **tags**: Add or modify tags to better categorize your service
  ```yaml
  tags:
    - web
    - financial
    - loan-analysis
    - calculator
    - frontend
  ```

### 2. Register the Component in Backstage

There are several ways to register the component:

#### Option A: Using the Backstage UI

1. Log in to your Backstage instance
2. Navigate to **Catalog** → **Register Existing Component**
3. Select **URL** as the registration method
4. Enter the URL to your `catalog-info.yaml` file:
   - If hosted on GitHub: `https://raw.githubusercontent.com/your-org/home_loan_check/main/catalog-info.yaml`
   - If hosted elsewhere: Provide the direct URL to the file
5. Click **Analyze** and then **Import**

#### Option B: Using the Backstage Configuration File

Add the location to your Backstage `app-config.yaml`:

```yaml
catalog:
  locations:
    - type: url
      target: https://raw.githubusercontent.com/your-org/home_loan_check/main/catalog-info.yaml
```

Then restart your Backstage instance.

#### Option C: Using the Backstage API

You can also register the component programmatically using the Backstage API:

```bash
curl -X POST http://your-backstage-instance/api/catalog/locations \
  -H "Content-Type: application/json" \
  -d '{
    "type": "url",
    "target": "https://raw.githubusercontent.com/your-org/home_loan_check/main/catalog-info.yaml"
  }'
```

### 3. Verify Registration

1. Navigate to the **Catalog** in Backstage
2. Search for "Home Loan Check" or "home-loan-check"
3. Click on the component to view its details
4. Verify that all metadata is correctly displayed

## Component Details

### Component Type
- **Type**: `website`
- This indicates that the component is a web-based application

### Lifecycle
- **Lifecycle**: `production`
- Other options: `experimental`, `development`, `deprecated`

### Ownership
- Ensure the `owner` field points to an existing team/user in your Backstage instance
- If the team doesn't exist, create it first in Backstage

### TechDocs Integration

The component is configured to use TechDocs for documentation:

```yaml
annotations:
  backstage.io/techdocs-ref: dir:.
```

To enable TechDocs:

1. Ensure you have a `mkdocs.yml` file in the repository root
2. Add documentation files in a `docs/` directory
3. Configure TechDocs in your Backstage instance

## Customization

### Adding Additional Metadata

You can extend the `catalog-info.yaml` file with additional information:

```yaml
metadata:
  links:
    - url: https://your-service-docs.com
      title: Documentation
      icon: description
    - url: https://your-dashboard.com
      title: Monitoring Dashboard
      icon: dashboard
```

### Adding Relations

You can define relationships to other components:

```yaml
spec:
  dependsOn:
    - component:another-service
  providesApis:
    - api:home-loan-api
```

## Troubleshooting

### Component Not Appearing

1. Check that the `catalog-info.yaml` file is accessible at the specified URL
2. Verify the YAML syntax is correct (use a YAML validator)
3. Check Backstage logs for any parsing errors
4. Ensure the location is registered in your Backstage configuration

### Metadata Not Displaying

1. Verify that all referenced entities (owner, system, domain) exist in Backstage
2. Check that annotations are correctly formatted
3. Review Backstage catalog logs for warnings

### TechDocs Not Loading

1. Ensure `mkdocs.yml` exists in the repository
2. Verify TechDocs is configured in your Backstage instance
3. Check that the `backstage.io/techdocs-ref` annotation is correct

## Best Practices

1. **Keep catalog-info.yaml in version control**: This ensures changes are tracked and reviewed
2. **Use meaningful tags**: Tags help with discoverability and filtering
3. **Keep metadata up-to-date**: Update the component information as the service evolves
4. **Link related resources**: Add links to documentation, dashboards, and other relevant resources
5. **Define relationships**: Use `dependsOn` and `providesApis` to show service dependencies

## Additional Resources

- [Backstage Documentation](https://backstage.io/docs)
- [Catalog Model](https://backstage.io/docs/features/software-catalog/descriptor-format)
- [TechDocs](https://backstage.io/docs/features/techdocs/techdocs-overview)

## Support

For issues or questions:
1. Check the Backstage documentation
2. Review the component's catalog-info.yaml file
3. Contact your Backstage administrator
