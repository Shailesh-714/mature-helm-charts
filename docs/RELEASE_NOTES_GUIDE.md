# Release Notes Guide

This guide explains how to add professional release notes to your Helm charts.

## Adding Release Notes to Chart.yaml

Add an `annotations` section to your `Chart.yaml` with ArtifactHub-compatible change entries:

```yaml
annotations:
  # ArtifactHub compatible changelog
  artifacthub.io/changes: |
    - kind: added
      description: "Support for PostgreSQL 16"
    - kind: changed
      description: "Updated default resource limits for better performance"
    - kind: deprecated
      description: "The `legacy.enabled` value is deprecated and will be removed in v1.0.0"
    - kind: removed
      description: "Removed support for Kubernetes versions < 1.24"
    - kind: fixed
      description: "Fixed ingress configuration for Kubernetes 1.27+"
    - kind: security
      description: "Updated Redis to 7.2.3 to address CVE-2023-XXXXX"
```

## Change Types

- **added**: New features or functionality
- **changed**: Changes to existing functionality
- **deprecated**: Features that will be removed in future versions
- **removed**: Features that have been removed
- **fixed**: Bug fixes
- **security**: Security-related updates

## Best Practices

1. **Be Specific**: Describe what changed, not just that something changed
2. **Include Version Numbers**: When updating dependencies, include version numbers
3. **Reference Issues**: Link to GitHub issues when applicable
4. **Breaking Changes**: Clearly mark breaking changes
5. **Migration Path**: For breaking changes, provide migration instructions

## Example Complete Annotation

```yaml
annotations:
  artifacthub.io/changes: |
    - kind: added
      description: "Added support for horizontal pod autoscaling"
    - kind: added
      description: "New `monitoring.enabled` value to deploy Prometheus ServiceMonitor"
    - kind: changed
      description: "Updated PostgreSQL chart dependency from 13.x to 15.x"
    - kind: changed
      description: "Increased default memory limits from 256Mi to 512Mi"
    - kind: deprecated
      description: "The `replicaCount` value is deprecated in favor of `autoscaling.minReplicas`"
    - kind: fixed
      description: "Fixed NetworkPolicy to allow traffic from ingress controller"
    - kind: security
      description: "Updated base image to address CVE-2023-45678"
  
  # Optional: Add links to detailed documentation
  artifacthub.io/links: |
    - name: Changelog
      url: https://github.com/juspay/hyperswitch-helm/blob/main/CHANGELOG.md
    - name: Upgrade Guide
      url: https://github.com/juspay/hyperswitch-helm/blob/main/charts/incubator/hyperswitch-app/UPGRADE.md
```

## Upgrade Notes

For major version changes or breaking changes, create an `UPGRADE.md` file in your chart directory with detailed migration instructions.

## Automation

The release workflow will automatically:
1. Extract these annotations from your Chart.yaml
2. Format them into professional release notes
3. Include installation instructions
4. Add links to upgrade documentation if present