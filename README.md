# Glim Helm Chart

## Disclaimer

This chart is heavily inspired by code found in this repository: [https://git.app.uib.no/caleno/helm-charts/-/tree/fb701bdd3f991abd6600967f6ce66e05325c34ac/stable/openldap](https://git.app.uib.no/caleno/helm-charts/-/tree/fb701bdd3f991abd6600967f6ce66e05325c34ac/stable/openldap).

## Prerequisites Details

* PV support on the underlying infrastructure

## Chart Details

This chart will do the following:

* Instantiate Glim services (REST and LDAP)

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
helm install --name my-release stable/glim
```

## Configuration

We use the docker images available at Docker Hub ([https://hub.docker.com/r/sologitops/glim](https://hub.docker.com/r/sologitops/glim)). Please consult [Glim's project readme](https://github.com/doncicuto/glim) for more information.

The following table lists the configurable parameters of the Glim chart and their default values.

| Parameter                          | Description                                                               | Default           |
| ---------------------------------- | ------------------------------------------------------------------------- | ------------------|
| `replicaCount`                     | Number of replicas                                                        | `1`               |
| `image.repository`                 | Container image repository                                                | `sologitops/glim` |
| `image.tag`                        | Container image tag                                                       | `0.5.0-alpha`          |
| `image.pullPolicy`                 | Container pull policy                                                     | `IfNotPresent`    |
| `extraLabels`                      | Labels to add to the Resources                                            | `{}`              |
| `existingSecret`                   | Use an existing secret for admin and config user passwords                | `""`              |
| `service.annotations`              | Annotations to add to the service                                         | `{}`              |
| `service.clusterIP`                | IP address to assign to the service                                       | `""`              |
| `service.ldapPort`                 | Service port for LDAP                                            | `1636`             |
| `service.apiPort`                 | Service port for REST API                                           | `1323`             |
| `service.type`                     | Service type                                                              | `ClusterIP`       |
| `env`                              | List of key value pairs as env variables to be sent to the docker image. See [https://github.com/doncicuto/glim](https://github.com/doncicuto/glim)) for available ones | `[see values.yaml]`  |
| `adminPassword`                    | Password for admin user. Unset to auto-generate the password              | None              |
| `searchPassword`                   | Password for search user. Unset to auto-generate the password             | None              |
| `apiSecret`                   | REST API secret for JWT tokens. Unset to auto-generate the password             | None              |
| `persistence.enabled`              | Whether to use PersistentVolumes or not                                   | `false`           |
| `persistence.storageClass`         | Storage class for PersistentVolumes.                                      | `<unset>`         |
| `persistence.accessMode`           | Access mode for PersistentVolumes                                         | `ReadWriteOnce`   |
| `persistence.size`                 | PersistentVolumeClaim storage size                                        | `100Mi`             |
| `persistence.existingClaim`                 | Existing PersistentVolumeClaim to be used size                                        | `<unset>`             |
| `resources`                        | Container resource requests and limits in yaml                            | `{}`              |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
helm install --name my-release -f values.yaml stable/glim
```

> **Tip**: You can use the default [values.yaml](values.yaml)

## Cleanup orphaned Persistent Volumes

Deleting the Deployment will not delete associated Persistent Volumes if persistence is enabled.

Do the following after deleting the chart release to clean up orphaned Persistent Volumes.

```bash
kubectl delete pvc -l release=${RELEASE-NAME}
```

## Custom Secret

`existingSecret` can be used to override the default secret.yaml provided
