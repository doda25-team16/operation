# Peer Review Submission Team 16

## Assignment 1
### Links
- operation: https://github.com/doda25-team16/operation/tree/a1
- backend: https://github.com/doda25-team16/model-service/tree/a1
- frontend: https://github.com/doda25-team16/app/tree/a1
- lib: https://github.com/doda25-team16/lib-version/tree/a1
### Comments
- We did not integrate F4: Multi-Architecture Containers to work with F8: Automated container image releases
- No implementation for F11: Lib-version pre-releases

The following can be validated in their respective repositories:
- `lib`: F1 & F2
- `backend` and `frontend`: F3
- To validate F5 of multi-staging, you can build the docker file in [v0.01](https://github.com/doda25-team16/model-service/tree/v0.0.1) (pre-multi-staging) and then build our latest Dockerfile in [backend a1](https://github.com/doda25-team16/operation/tree/a1) and then compare image SIZES with `docker images`
- For F6 and F7 (Docker Compose + Flexible Port Configuration), you can learn more in the [README.md](https://github.com/doda25-team16/operation)
- F8 and F9 are just automated releases, they can be found in https://github.com/orgs/doda25-team16/packages and https://github.com/doda25-team16/model-service/releases
- F10 contains instructions for running models and mounts in the [README.md](https://github.com/doda25-team16/model-service/blob/main/README.md)

## Assignment 2
### Links
- backend: https://github.com/doda25-team16/model-service/tree/a2 (should be in operations but oops :))
### Comments
- Relevant files are all contained in `root` and `./ansible`
- We did not fully implement steps 15-23.
- Steps 1 through 14 are merged in the respective repositories.
- Beware: we have put our VM/cluster config files in the `model-service` repository. We plan to migrate it to the `operation` repo.
- Specific run instructions can be found in the `README.md` in the `operations` [repo](https://github.com/doda25-team16/operation?tab=readme-ov-file#running-the-application-cluster-for-assignment-2).

## Assignment 3
### Links
- operation: https://github.com/doda25-team16/operation/tree/a3-v1
- backend: https://github.com/doda25-team16/model-service/tree/a3-v1
- frontend: https://github.com/doda25-team16/app/tree/a3-v1
- lib: https://github.com/doda25-team16/lib-version/tree/a3-v1

### Comments
- Running info can be found in `operation/README.md`
- For this assignment, we moved all provisioning code from `model-service` to `operation` repo, where it can be found now.
- The only part of A3 fully finished is the migration from docker compose to kubernetes.
- Other steps, so Helm, Monitoring, Alerting and Grafana have some start made, but are not fully functional yet. Because we spent the time finalizing A2 in the mean time.

## Assignment 4
### Links
- operation: https://github.com/doda25-team16/operation/tree/a4-v2
- backend: https://github.com/doda25-team16/model-service/tree/a4-v2
- frontend: https://github.com/doda25-team16/app/tree/a4-v2
- lib: https://github.com/doda25-team16/lib-version/tree/a4-v2

### Comments
- Running info can be found in `operation/README.md`
- Of A4 we have implemented the `Documentation` and `Traffic management` parts. These can both be found in the `operation` repo where it would be expected.
- One `Additional Use Case` has been implemented, namely traffic rate limiting for Istio also in `operation`.
- `Continious Experimentation` has not been implemented yet.
