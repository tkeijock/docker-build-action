# Docker Build Action

This repository provides a reusable GitHub Actions workflow that builds and pushes Docker images to Docker Hub.

Instead of copying pipelines into multiple repositories, you can treat this workflow as a function:
call it from any other repository and pass only the required inputs.

The goal is to encapsulate the Docker build logic once and reuse it everywhere.

🧩 How It Works
---
This repository exposes a reusable workflow that other repositories can call:

- It builds a Docker image from the Dockerfile located in the target repository

- It tags the image

- It pushes the image to Docker Hub

Your projects (Repo B, Repo C, etc.) only need to reference this workflow and provide the inputs.

🔧 Inputs
---
1) ```inputs.docker_context ```:
 Specifies where the Dockerfile is located in the calling repository.

Each repository may have a different project structure and the Dockerfiles may not always be in the root folder

This reusable workflow does not assume where the Dockerfile is located.

Example :   ```docker_context: ./app```
 
 2) ```inputs.docker_repo```:
    
Defines the Docker Hub repository name where the built image will be pushed.

Example : ```docker_repo: my-repo-1```

🔑Secrets Required in the Calling Repository
---
Even though this repository ( repo A) performs the build and push, GitHub uses the security context of the repository that invokes the workflow.

Therefore, the caller repository (Repo B, Repo C, etc.) must define:

```secrets.DOCKER_USERNAME```

```secrets.DOCKER_PASSWORD```


📎 Example Usage (from another repository)
---

```
jobs:
  build:
    uses: your-user/docker-build-action/.github/workflows/build-push-docker.yml@main
    with:
      docker_context: ./folder1
      docker_repo: my-repo1
    secrets:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
```



