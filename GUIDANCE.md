# Kibana on Wodby

What Wodby sets up for this Kibana service. It runs the official Elastic image, configured through environment variables set by the manifest.

## Linked Elasticsearch

The `elasticsearch` link is required. It sets:

| Variable | Meaning |
| --- | --- |
| `ELASTICSEARCH_HOSTS` | the linked Elasticsearch service over plain HTTP |
| `ELASTICSEARCH_USERNAME` | the built-in `kibana_system` user |
| `ELASTICSEARCH_PASSWORD` | the password Wodby generated for that user on the Elasticsearch service |

The Elasticsearch service sets the `kibana_system` password itself after it starts. Nothing has to be configured by hand, and the enrollment-token setup is switched off (`INTERACTIVESETUP_ENABLED`). Kibana stays unready until Elasticsearch is up and that password is set.

## Signing in

- The web interface is on port `5601`, the service's main HTTP endpoint. `SERVER_PUBLICBASEURL` is set to the service's URL in the environment.
- People sign in with Elasticsearch users. The built-in superuser is `elastic`; its password is the `password` token of the linked Elasticsearch service. `kibana_system` is a service account and cannot be used to sign in.

## What is generated

Three encryption keys are generated once and kept stable across deployments: `XPACK_SECURITY_ENCRYPTIONKEY`, `XPACK_ENCRYPTEDSAVEDOBJECTS_ENCRYPTIONKEY` and `XPACK_REPORTING_ENCRYPTIONKEY`. Replacing them signs users out and makes encrypted saved objects, such as connector secrets, unreadable.

Telemetry is off (`TELEMETRY_ENABLED`).

## Changing configuration

Kibana settings are environment variables on this service in the image's form: the setting name in upper case with dots replaced by underscores. Do not mount a `kibana.yml`.

## Data

Kibana has no volume. Dashboards, data views and other saved objects are stored in the linked Elasticsearch, on its volume.

## Check the result

- `GET http://<app service name>:5601/api/status` answers `200` when Kibana is ready.
- "Kibana server is not ready yet" means Elasticsearch is unreachable or the `kibana_system` password is not set yet: check the Elasticsearch service first.
