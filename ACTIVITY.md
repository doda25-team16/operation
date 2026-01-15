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

- Ignas: https://github.com/doda25-team16/model-service/pull/11/files
  This week I had less contribution in pure code, as it depended on previous steps. I have made a start for steps 20 and up, and helped with other features. When the previous steps are running, I will finish the remaining TODOs in the code, as now it cant be properly tested.

- Johnny: https://github.com/doda25-team16/model-service/pull/9
  This week, I worked on the initial set up from #1 - #7, which involved setting the basic Vagrant and Ansible provisioning stuff. Instantiated the preliminaries for Vagrantfiles, ctrl, general and node files for the rest of the team to work on.

- Uddhav: https://github.com/doda25-team16/model-service/pull/12
  This week I worked on steps 12 and 13 for A2, which involved enabling kubelet and controller initialization. I was also assigned steps 18 and 19, which I will get to in the coming days once step 17 is complete.


- Job: https://github.com/doda25-team16/lib-version/pull/2  
       Implemented automated versioning and release management for the lib-version library. Added support for stable releases, prereleases, and version metadata packaging to ensure the library can         be published reliably and consumed across multiple environments.

  https://github.com/doda25-team16/model-service/tree/step22-kubernetes-dashboard 
      Integrated the Kubernetes Dashboard into the provisioning workflow. Added automated Helm installation, deployed the Dashboard via its Helm charm. could not do the other steps at the moment due to previous parts not being finished.

### Week Q2.4 (Dec 1+)

- Caio: https://github.com/doda25-team16/app/pull/8 I worked on the monitoring setup for assignment 3. Thus, I added a /metrics to our frontend which collected different information in the form of a histogram, gauge and counter.

- Ignas: I worked on implementing the last steps of A2, which was not possible earlier due to missing earlier steps and node configurations.
  Implemented MetalLB, Ingress, IPconfigs, L2advertisements and improved other configurations (https://github.com/doda25-team16/model-service/pull/21). I also moved everything from `model-service` to `operation` which had to do with our infra provisioning (https://github.com/doda25-team16/operation/pull/49). And I made a start for Alerting in A3 (https://github.com/doda25-team16/operation/pull/50)

- Yuting: https://github.com/doda25-team16/model-service/pull/16
  I worked on steps 15 - 16 of A2, which installed Flannel and Helm, and added configuration in the ctrl file. After that, I started working on the Grafana part of A3, which will be done in the next week.

- Johnny: https://github.com/doda25-team16/operation/pull/48

  Had the leftover task from A1 on Automated Multi-Architecture Container release which I did. For A3, I did the migration over from Docker-Compose -> Kubernetes. Implemented the core components such as Services, Deployments and Ingress in separate template files. Additionally, added ConfigMap to App (to customize model URL + ports) and Secrets (not used) to the program.

- Uddhav: https://github.com/doda25-team16/model-service/pull/19
  I worked on the leftover tasks from A2, specifically steps 17, 18 and 19. The steps involved generating and running join commands, and the necessary changes have been made to the ctrl and node yaml files

- Job: https://github.com/doda25-team16/operation/pull/53 and https://github.com/doda25-team16/operation/pull/52, Made a working helm chart that runs the project and implemented steps 22 and 23 from a2.

### Week Q2.5 (Dec 8+)

- Caio: https://github.com/doda25-team16/operation/pull/61 I worked on the traffic management section of A4. This involved adding the istio configurations for a canary release. Thus, I applied what was previously seen in class for istio and a canary release to our project with some modifications to make it work for our case. I also gave each version of sms-app and model a different weight (90 and 10).
- Ignas: Implemented Rate limiting as an additional usecase from A4 https://github.com/doda25-team16/operation/pull/63. Also Continued working on finishing alerting from A3.
- Uddhav: https://github.com/doda25-team16/operation/pull/64 - I worked on parts of A2 and A3 to make the 'Kubernetes Usage' part of A3 fall in accordance with the 'Excellent' rubric. In particular I made sure all VMs mount /mnt/shared into the VM.
- Job: https://github.com/doda25-team16/operation/pull/65 and https://github.com/doda25-team16/operation/pull/66. Worked on making the documentation and creating an extension proposal.
- Johnny: https://github.com/doda25-team16/operation/pull/67 Continued off of Caio and Ignas's work on Traffic Management + Istio. None of their work was included in the Helm Chart so I did the migration over + added configurable values for stable & canary splits + made ingress gateway selector customizable and able to accept multiple ingress gateway pods.

### Week Q2.6 (Dec 15+)

- Johnny: https://github.com/doda25-team16/operation/pull/69 Small changes this week, spent a lot of time on debugging why our Helm Installation wasn't working after our latest push to main. Some hard-coded paths were pushed onto main (related to storage). Also made some small tweaks to provisioning and deployments.

- Yuting: https://github.com/doda25-team16/operation/pull/73 I continued with the Grafana Dashboard task this week. This involved creating a dashboard and connect to Prometheus automatically. I spent a lot of time debugging the issues of vagrant provision on my host machine. The nodes couldn't be joined automatically and provision didn't work. After this task, I will continue with improving A3 and A4.

- Caio: https://github.com/doda25-team16/operation/pull/70 This week I read through some of the feedback recieved for assignment 1. I saw that we where missing the .env file for that assignment and I decided to implement it into our project. I also then updated the readme accordingly and was able to get it all working.

- Job: https://github.com/doda25-team16/lib-version/pull/4 and https://github.com/doda25-team16/app/pull/10: This week, I went back to see what we had missed from previous weeks and looked into how we could reuse our lib-version in other parts of our code.

- Uddhav: https://github.com/doda25-team16/operation/pull/68 I reworked on the Traffic Management part of A4 this week so that I can move forward with the Continuous Experimentation part. The TM does work now, but it could still use some changes which i am currently working on now.

- Ignas: https://github.com/doda25-team16/operation/pull/72 This week I implemented Alerting for Assignment 3, which was still missing. This required also some updates to the playbooks to install Prometheus corrctly. I also took the time to make all the Ansible playbooks user independent (where user is a variable), and architecture independent and idempotent. Now it works for both AMD64 and ARM64 architectures, and re-running playbooks does not give errors.

### Week Q2.7 (Jan 5+)

- Yuting: https://github.com/doda25-team16/operation/pull/76 This week I updated the Grafana Dashboard to display the three metrics we created in the sms app. And I fixed the problem of duplicated Grafana deployments. The Grafana dashboard JSON file is stored locally so that both manual and automatic installation are possible.

- Caio: https://github.com/doda25-team16/operation/pull/88 This week I worked on fixing some of the previous issues we had on A2. What I did was add the second public ssh key we needed to the project. I also updated the script slightly to accout for this second ssh key

- Ignas: https://github.com/doda25-team16/operation/pull/89 This week I fixed multiple issues in our cluster provisioning. I replaced inline shell scripts with helm modules where possible, fixed running playbooks on the ctrl node, fixed the IP range and some other idempotancy improvements.

- Johnny: https://github.com/doda25-team16/operation/pull/92 I extended upon our pre-existing rate-limiting implementation and brought it up to Excellent from Good by implementing a Global rate-limiting and also a user-based rate limiting based on per-user headers.

- Job: https://github.com/doda25-team16/app/pull/12 I updated the version on the app repo to bump up the grade after testing, the lib-version.
- Uddhav: https://github.com/doda25-team16/app/pull/11 For the continuous experimentation part I made a second version of the app(made a minimal UI change (button color to yellow)) to create a new application version. The change is intentionally small to isolate its impact during canary deployment.
  https://github.com/doda25-team16/operation/pull/74 After facing a lot of errors and bugs in the Traffic Management part, I was able to fix it, and I also added Sticky Sessions so my continuous experimentation part goes smoothly. (Note: this PR was made during the break but since the activity.md file goes from Dec 15+ to Jan 5+, so I am pasting that week's contribution here)
