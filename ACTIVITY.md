### Week Q2.1 (Nov 10+)

No work

### Week Q2.2 (Nov 17+)

- Caio: https://github.com/doda25-team16/app/pull/3
I have worked on F5, F4 and F3 for the frontend of the project. This envolved creating the dockerfile for the front end, making it work for multiple architectures and making it have multiple stages. I have also reviewed a few pull requests created by my peers.
- Ignas: I have worked on F3, F4, F5 for the backend. This meant containerizing the backend, making it multi architecture and multi stage. Also I set up the backend repo, and ran the initial model setup.
    - https://github.com/doda25-team16/model-service/pull/1 
    - https://github.com/doda25-team16/model-service/pull/2
    - https://github.com/doda25-team16/model-service/pull/7
- Yuting: https://github.com/doda25-team16/operation/pull/18

    I have worked on F6, and F7 of A1. For F6, I enabled the possibility of flexible ports on which each service runs. And for F7, I created the necessary configuration file and documentation for running and operating application.

- Johnny: https://github.com/doda25-team16/model-service/pull/8
I have worked on integrating F8: Automated Container Release (both model-service and app) and F10: No hard-coded models. 
  - For F8, I've implemented the workflow that uses the pom.xml as the single source of truth and the workflow will validate that the pom.xml's version = git tag in order to execute the action of releasing the containers into the registry. 
  - For F10, I directly modified serve_model.py and implemented the logic of checking for the validity of a provided volume mount and model file in which the model will use. In any case, it will either use the provided model or it will download a given model from our release. Should not throw any errors.
  - Also reviewed 2 pull requests (F5.2 & F6)

- Uddhav: https://github.com/doda25-team16/model-service/pull/4
I worked on F9, where I automated the training and release pipeline. I created a workflow to train the model, packaged the artifact and used GitHub CLI 'gh' to create releases. I had to debug permission issues with 'GH_TOKEN'

- Job: https://github.com/doda25-team16/lib-version/commits/feature/version-aware-library/
Worked on integrating F1 and F2 for the version-aware library. Features include automatically package and version the library and release it to the package registry. Workflow is also compatible with snapshots.

### Week Q2.3 (Nov 24+)

- Caio: https://github.com/doda25-team16/model-service/pull/10
Worked on steps 8, 9 10 and 11 of A2. These envolved ensuring all nodes are reachable by name, adding the kubernetes repository, installing k8s tools and configuring containerd.

- Yuting: https://github.com/doda25-team16/model-service/pull/15
  I have worked on A2 step 14: setup and connect kubectl to control the configured workloads. I am still working on A2 step 15 and 16, which will be done in the next week.
