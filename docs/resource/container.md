# Container

**Status**: [Experimental][DocumentStatus]

**type:** `container`

**Description:** A container instance.

<!-- semconv container -->
| Attribute  | Type | Description  | Examples  | Requirement Level |
|---|---|---|---|---|
| `container.name` | string | Container name used by container runtime. | `opentelemetry-autoconf` | Recommended |
| `container.id` | string | Container ID. Usually a UUID, as for example used to [identify Docker containers](https://docs.docker.com/engine/reference/run/#container-identification). The UUID might be abbreviated. | `a3bf90e006b2` | Recommended |
| `container.runtime` | string | The container runtime managing this container. | `docker`; `containerd`; `rkt` | Recommended |
| `container.image.name` | string | Name of the image the container was built on. | `gcr.io/opentelemetry/operator` | Recommended |
| `container.image.tags` | string[] | Container image tags. An example can be found in [Docker Image Inspect](https://docs.docker.com/engine/api/v1.43/#tag/Image/operation/ImageInspect). Should be only the `<tag>` section of the full name for example from `registry.example.com/my-org/my-image:<tag>`. | `[v1.27.1, 3.5.7-0]` | Recommended |
| `container.image.id` | string | Runtime specific image identifier. Usually a hash algorithm followed by a UUID. [1] | `sha256:19c92d0a00d1b66d897bceaa7319bee0dd38a10a851c60bcec9474aa3f01e50f` | Recommended |
| `container.image.repo_digests` | string[] | Repo digests of the container image as provided by the container runtime. [2] | `[example@sha256:afcc7f1ac1b49db317a7196c902e61c6c3c4607d63599ee1a82d702d249a0ccb, internal.registry.example.com:5000/example@sha256:b69959407d21e8a062e0416bf13405bb2b71ed7a84dde4158ebafacfa06f5578]` | Recommended |
| `container.command` | string | The command used to run the container (i.e. the command name). [3] | `otelcontribcol` | Opt-In |
| `container.command_line` | string | The full command run by the container as a single string representing the full command. [2] | `otelcontribcol --config config.yaml` | Opt-In |
| `container.command_args` | string[] | All the command arguments (including the command/executable itself) run by the container. [2] | `[otelcontribcol, --config, config.yaml]` | Opt-In |
| `container.labels.<key>` | string | Container labels, `<key>` being the label name, the value being the label value. | `container.labels.app=nginx` | Recommended |

**[1]:** Docker defines a sha256 of the image id; `container.image.id` corresponds to the `Image` field from the Docker container inspect [API](https://docs.docker.com/engine/api/v1.43/#tag/Container/operation/ContainerInspect) endpoint.
K8s defines a link to the container registry repository with digest `"imageID": "registry.azurecr.io /namespace/service/dockerfile@sha256:bdeabd40c3a8a492eaf9e8e44d0ebbb84bac7ee25ac0cf8a7159d25f62555625"`.
The ID is assinged by the container runtime and can vary in different environments. Consider using `oci.manifest.digest` if it is important to identify the same image in different environments/runtimes.

**[2]:** [Docker](https://docs.docker.com/engine/api/v1.43/#tag/Image/operation/ImageInspect) and [CRI](https://github.com/kubernetes/cri-api/blob/c75ef5b473bbe2d0a4fc92f82235efd665ea8e9f/pkg/apis/runtime/v1/api.proto#L1237-L1238) report those under the `RepoDigests` field.

**[3]:** If using embedded credentials or sensitive data, it is recommended to remove them to prevent potential leakage.
<!-- endsemconv -->

## Open Container Initiative (OCI)

The [Open Container Initiative](https://opencontainers.org/) defines open industry standards around container formats and runtimes.

### OCI Image Manifest

This section refers to the [specification](https://github.com/opencontainers/image-spec/blob/main/manifest.md)
that defines an OCI Image manifest.

**Status**: [Experimental][DocumentStatus]

**type:** `oci`

**Description:** Attributes of an OCI image manifest.

<!-- semconv oci.manifest -->
| Attribute  | Type | Description  | Examples  | Requirement Level |
|---|---|---|---|---|
| `oci.manifest.digest` | string | The digest of the OCI image manifest. For container images specifically is the digest by which the container image is known. [1] | `sha256:e4ca62c0d62f3e886e684806dfe9d4e0cda60d54986898173c1083856cfda0f4` | Recommended |

**[1]:** Follows [OCI Image Manifest Specification](https://github.com/opencontainers/image-spec/blob/main/manifest.md), and specifically the [Digest property](https://github.com/opencontainers/image-spec/blob/main/descriptor.md#digests).
An example can be found in [Example Image Manifest](https://docs.docker.com/registry/spec/manifest-v2-2/#example-image-manifest).
<!-- endsemconv -->

[DocumentStatus]: https://github.com/open-telemetry/opentelemetry-specification/tree/v1.22.0/specification/document-status.md
