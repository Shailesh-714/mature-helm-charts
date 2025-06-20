# hyperswitch-istio

![Version: 0.1.1](https://img.shields.io/badge/Version-0.1.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.16.0](https://img.shields.io/badge/AppVersion-1.16.0-informational?style=flat-square)

A Helm chart for Kubernetes

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| image.version | string | `"v1o107o0"` |  |
| ingress.annotations | object | `{}` |  |
| ingress.className | string | `"alb"` |  |
| ingress.enabled | bool | `true` |  |
| ingress.hosts.paths[0].name | string | `"istio-ingressgateway"` |  |
| ingress.hosts.paths[0].path | string | `"/"` |  |
| ingress.hosts.paths[0].pathType | string | `"Prefix"` |  |
| ingress.hosts.paths[0].port | int | `80` |  |
| ingress.hosts.paths[1].name | string | `"istio-ingressgateway"` |  |
| ingress.hosts.paths[1].path | string | `"/healthz/ready"` |  |
| ingress.hosts.paths[1].pathType | string | `"Prefix"` |  |
| ingress.hosts.paths[1].port | int | `15021` |  |
| ingress.tls | list | `[]` |  |
| livenessProbe.httpGet.path | string | `"/"` |  |
| livenessProbe.httpGet.port | string | `"http"` |  |
| readinessProbe.httpGet.path | string | `"/"` |  |
| readinessProbe.httpGet.port | string | `"http"` |  |
| replicaCount | int | `1` |  |
| service.port | int | `80` |  |
| service.type | string | `"ClusterIP"` |  |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.14.2](https://github.com/norwoodj/helm-docs/releases/v1.14.2)
