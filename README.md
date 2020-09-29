# jenkins-x-kubernetes
Kubernetes Build Pack extends the Classic Build Pack to add opinionated CI+CD for Kubernetes Environments with GitOps based promotion

To change the Jx source to point to this repository, run the following command:

```bash
jx edit buildpack \
-u https://github.com/favedom-dev/jenkins-x-kubernetes \
-r master \
-b
```

When you import a new project, Jenkins X will use the build packs from this repository, i.e.

```bash
jx import 
```

The buildpack has the following structure:

- Dockerfile – Contains the build of the docker image.
- charts – Contains Helm charts and Kubernetes files that describe how to deploy the project to staging and production environments.
- pipeline.yaml – Contains the steps for building, testing and delivering the project.
- preview – Contains Helm charts and Kubernetes files that describe how to deploy the project to a preview environment.
- skaffold.yaml – Contains the steps for building and storing the docker image.