
# .kick.yml - the container config file

The <kbd>.kick.yml</kbd> file is the central configuration file for 
the container. It specifies the docker image to start. And is responsible
for configuring the container. Inside the container it is evaluated by the
<kbd>kick</kbd> command.

<kbd>.kick.yml</kbd> defines commands to be executed at build, dev, production
and test time. You can run these commands by running <kbd>kick [command]</kbd>.
This has the advantage to test your scripts incremental inside the container
instead of rebuilding your container over and over again.


## Defining commands and build stages in <kbd>.kick.yml</kbd>

```yaml
version: 1
from: infracamp/kickstart-flavor-php:7.4

# Install packages via apt
packages: [wget, curl]

command:
  build:
    - "composer update"
    - "npm install"
    - "npm build"
    
  dev:
    - npm build --watch
    
  run:
  
  test:
    - phpunit
```

Inside the container you can now execute e.g. the tests with <kbd>kick test</kbd>.

## The Dockerfile <small></small>

Keeping the container configuration inside <kbd>.kick.yml</kbd> helps reducing your
<kbd>Dockerfile</kbd> to only a few lines:

```dockerfile
FROM nfra/kickstart-flavor-bare:1.0
ENV DEV_CONTAINER_NAME="your-project-name"

ADD / /opt
RUN ["bash", "-c",  "chown -R user /opt"]
RUN ["/kickstart/run/entrypoint.sh", "build"]

ENTRYPOINT ["/kickstart/run/entrypoint.sh", "standalone"]
```
*Make sure to adjust `FROM` and `DEV_CONTAINER_NAME` to the settings in
 <kbd>.kick.yml</kbd>*


## Run additional services in stack

To run additional services, just create a docker-compose like stack
file named `.kick-stack.yml` in your projects root directory.

<kbd>kickstart</kbd> will automatically create a new network with
the name of your current project. Make sure to adjust the names
in your stack file:


***`.kick-stack.yml`***
```yaml
version: "3"
services:
  # This service will be available as: project_name_some_service
  some_service:
    image: some/image
    networks:
      - project_name  ## <- Adjust these to your projects name

networks:
  project_name: ## <- Adjust these to your projects name
    external: true
```


## Integration into CI/CD pipeline

This example shows how to integrate in <kbd>.gitlab-cl.yml</kbd>:

```yaml
image: docker:stable
stages:
  - build

services:
  - docker:dind

before_script:
  - apk update && apk add bash curl
  - curl -o kickstart 'https://raw.githubusercontent.com/nfra-project/nfra-kickstart/master/dist/kickstart.sh' && chmod +x kickstart

latest:
  stage: build
  script:
    - ./kickstart :test
    - ./kickstart ci-build
  only:
    - master
```

This will execute the unit-tests defined in <kbd>command > test</kbd>-section 
and then build the container running <kbd>command > build</kdb> command.

The command <kbd>kickstart.sh ci-build</kbd> will build the container using
Dockerfile and push the newly created container to gitlab's registry.