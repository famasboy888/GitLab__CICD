# GitLab CICD

### 1) Push or store code in GitLab repository.

<p align="left">
  <img width="40%" height="40%" src="https://github.com/famasboy888/GitLab__CICD/assets/23441168/b1e53716-037d-46ea-9602-095b97720f9c">
</p>

### 2) Create the CI/CD Pipeline.

#### 2.1) Create a file called `.gitlab-ci.yml` in the root directory

<p align="left">
  <img width="40%" height="40%" src="https://github.com/famasboy888/GitLab__CICD/assets/23441168/dd145081-c034-43a0-93c2-a9ada1be6f58">
</p>

#### 2.2) Create test job:

```yaml
run_tests:                                               <== Name of the step
    image: python:3.9-slim-buster                        <== Replaces the default docker image executor e.g. Ruby
    before_script:                                       <== You can run pre-defined commands or scripts here
        - apt-get update && apt-get install make -y      <== this is the predefined command to install 'make'
    script:                                              <== this is the main script command
        - make test
```

#### 2.3) Create build job:

- We need to first create an New Access Token from DockerHub.io
- Copy the username and secret temporarily.
- Go back to GitLab > Settings > CI/CD > Variables > Add Variable

Now we will edit the Pipeline yaml file

```bash
variables:                                                      <== Pipeline level variable
    IMG_NAME: famasboy888/demo-app
    IMG_TAG: 1.0.0

run_tests:
  ...
  ...

build_image:                                                  
    image: docker:26.1.0-alpine3.19                            <== base image
    services:                                                  <== we can run daemon service
        - docker:26.1.0-dind-alpine3.19                        <== daemon image
    variables:                                                 <== Job level variable
        DOCKER_TLS_CERTDIR: "/certs"                           <== Connection between the base image and service daemon
    before_script:
        - docker login -u $DOCKERHUB_USER -p $DOCKERHUB_SECRET    <== login using credentials
    script:
        - docker build -t $IMG_NAME:$IMG_TAG .
        - docker push $IMG_NAME:$IMG_TAG
```

#### 2.4) We notice that the jobs are ran in parallel. We don't want that so we use `stages` key:

```yaml
variables:
    ...

stages:                  <== define stages and order of the jobs
    - test
    - build

run_tests:
    stage: test          <== specify the stage
    ...

build_image:
    stage: build         <== specify the stage
    ...
```

You will have a sequenced job after:

<p align="left">
  <img width="40%" height="40%" src="https://github.com/famasboy888/GitLab__CICD/assets/23441168/f96132a5-c10a-477e-b2a7-a486506e1092">
</p>
