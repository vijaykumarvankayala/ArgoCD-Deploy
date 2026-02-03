# ArgoCD deployment repo - Java microservices

This repository holds:
- source templates for Java microservices (example Dockerfiles)
- Kubernetes manifests (kustomize base + overlays)
- GitLab CI pipeline to build, push images and update k8s overlays
- ArgoCD Application manifests to deploy the overlay to a cluster

How it works:
1. CI builds Docker images for each service and pushes to `$CI_REGISTRY`.
2. CI updates k8s overlay (`k8s/overlays/production/kustomization.yaml`) with the new image tags and commits the change back to the repo.
3. ArgoCD watches this repo path (`k8s/overlays/production`) and will sync the updated manifests to the cluster.

Pre-requisites:
- GitLab project (create and push this repo)
- GitLab Runner capable of docker:dind (or an equivalent build environment)
- Kubernetes cluster
- ArgoCD installed in the cluster (or ready to install)
- (Optional) Use GitLab Container Registry or an external registry (set credentials in CI variables)

Replace placeholders in `.gitlab-ci.yml` and ArgoCD manifests with your real values before use.
