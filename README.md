# ILM Keycloak Optimized

> This repository is part of the commercial open-source project ILM. You can find more information about the project at the [ILM](https://github.com/OmniTrustILM/ilm) repository, including the contribution guide.

This repository builds an optimized Keycloak container image for ILM, based on the official Keycloak image.

This image targets Keycloak `26.8.1` using the official
`quay.io/keycloak/keycloak:26.8.1-0` base image. The corresponding ILM image
version is `26.8.1-0`.

The image enables Keycloak's experimental `cimd` feature, so MCP clients can identify themselves with
[Client ID Metadata Documents](https://www.keycloak.org/securing-apps/mcp-authz-server). Every realm then
advertises this support in its metadata, but accepts such client IDs only when its client policies use the
`client-id-metadata-document` executor. Clients that prefer it, such as Claude, try it instead of dynamic client
registration, so a realm that relies on dynamic registration for these clients also needs that policy.

## Container image

The image is published as `hub.omnitrustregistry.com/ilm/keycloak-optimized`.
The ILM Helm chart uses `hub.omnitrustregistry.com/ilm/keycloak-optimized:26.8.1-0`.

## CI and publishing

Pull requests use the shared ILM [container test workflow](https://github.com/OmniTrustILM/.github/blob/main/.github/workflows/containers-test.yml).
Pushes to `main` and release tags use the shared [container publishing workflow](https://github.com/OmniTrustILM/.github/blob/main/.github/workflows/containers-build-and-push.yml).
These workflows build `linux/amd64` and `linux/arm64` images and use the central ILM vulnerability policy. Publishing signs the images with Cosign and updates the registry README.

Publishing requires the `CONTAINER_REGISTRY_USERNAME`, `CONTAINER_REGISTRY_PASSWORD`,
`COSIGN_PRIVATE_KEY`, and `COSIGN_PASSWORD` secrets. PR builds require no registry or signing secrets.

Publish the `26.8.1-0` release tag and verify that the image is available in the ILM registry before rolling out the matching Helm chart.

For more information on optimized Keycloak builds, refer to:
- [Configuring Keycloak](https://www.keycloak.org/server/configuration)
- [Running Keycloak in a container](https://www.keycloak.org/server/containers)
