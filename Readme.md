# GitLab CICD

### 1) Push or store code in GitLab repository.

<p align="left">
  <img width="40%" height="40%" src="https://github.com/famasboy888/GitLab__CICD/assets/23441168/b1e53716-037d-46ea-9602-095b97720f9c">
</p>

### 2) Create a file called `.gitlab-ci.yml` in the root directory.

<p align="left">
  <img width="40%" height="40%" src="https://github.com/famasboy888/GitLab__CICD/assets/23441168/dd145081-c034-43a0-93c2-a9ada1be6f58">
</p>

```yaml
run_tests:                                               <== Name of the step
    image: python:3.9-slim-buster                        <== Replaces the default docker image executor e.g. Ruby
    before_script:                                       <== You can run pre-defined commands or scripts here
        - apt-get update && apt-get install make -y      <== this is the predefined command to install 'make'
    script:                                              <== this is the main script command
        - make test
```
