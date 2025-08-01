## Description
<!-- Provide a brief description of the changes in this PR -->

## Type of Change
<!-- Mark the relevant option with an "x" -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update
- [ ] Dependency update

## Chart(s) Affected
<!-- List the charts that are modified in this PR -->
- [ ] hyperswitch-app
- [ ] hyperswitch-card-vault
- [ ] hyperswitch-istio
- [ ] hyperswitch-keymanager
- [ ] hyperswitch-monitoring
- [ ] hyperswitch-stack
- [ ] hyperswitch-ucs
- [ ] hyperswitch-web

## Checklist
<!-- Mark completed items with an "x" -->
- [ ] Chart version has been bumped in `Chart.yaml`
- [ ] Release notes have been added to `Chart.yaml` annotations (see [Release Notes Guide](docs/RELEASE_NOTES_GUIDE.md))
- [ ] Documentation has been updated (run `task update-readme`)
- [ ] For breaking changes: `UPGRADE.md` has been created/updated
- [ ] Values have been documented with comments
- [ ] Chart has been tested locally

## Release Notes
<!-- 
Summarize the changes for the release notes. This will be added to Chart.yaml annotations.
Example:
- kind: added
  description: "Added support for custom annotations on all resources"
- kind: fixed
  description: "Fixed ingress path configuration for nginx controller"
-->

```yaml
- kind: <added|changed|deprecated|removed|fixed|security>
  description: "Your change description here"
```

## Testing
<!-- Describe how you tested these changes -->
- [ ] Installed chart in local Kubernetes cluster
- [ ] Upgraded from previous version
- [ ] Verified all pods are running
- [ ] Tested specific functionality: <!-- describe what you tested -->

## Additional Notes
<!-- Any additional information that reviewers should know -->