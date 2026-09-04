# Kibana service for Kubernetes on Wodby

Run Kibana as a reusable Kubernetes application service with Wodby.

This repository defines the Wodby service manifests and operational
configuration for Kibana.

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Wodby stacks using this service

- [Elasticsearch application stack](https://github.com/wodby/stack-elasticsearch)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `kibana` |
| Type | Application service |
| Versions | `9.5` by default |
| Workloads | `main` (Deployment, primary) |
| Containers | `kibana` using `docker.elastic.co/kibana/kibana` |
| Endpoints | `kibana`: HTTP 5601 (main) |
| Service links | Elasticsearch (`elasticsearch`), required |
| Application build | Not buildable from application source |
| Helm | chart `oci://registry-1.docker.io/wodby/stateless`; version `0.2.0` |

## Use this service

Use this service through [Elasticsearch application stack](https://github.com/wodby/stack-elasticsearch), or reference `kibana` from a
custom Wodby stack.

A service is a reusable component and does not deploy by itself. The stack
defines its links, settings, versions, resources, and relationship to the rest
of the application.

## Authentication

The required Elasticsearch link supplies Kibana's `kibana_system` credentials.
The linked Elasticsearch service initializes that internal account before
Kibana is deployed. Sign in to the Kibana UI as `elastic` using the linked
Elasticsearch service's generated `password` token.

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Keep service, workload, container, endpoint, link, volume, config, and
derivative names stable unless dependent stacks and app-level overrides are
updated at the same time.

Validate the manifests with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the [managed services index](https://github.com/wodby/services).
