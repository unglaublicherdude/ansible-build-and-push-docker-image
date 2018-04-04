## PT-ATA-KEYS

This role lets you easy build (lokal) and push a docker image.
Just prepare an Image (Dockerfile) in a directory within your playbook and configure the role with parameters.

It will skip te build, if the image already exists locally. So it is important, that you change the Version if you want to build a new image.

Don't change the vars in this role directly. Use role-vars (see example).

This role has the python dependency:
docker-py

It will fail, if the dependency is not installed.

## Installation

```ansible-galaxy install git+https://github.com/unglaublicherdude/ansible-build-and-push-docker-image.git```

## Example

### Simple build and push

```
    - {
      role: build-and-push-docker-image,
      build_and_push_image_name: "registryhost.tld/image",
      build_and_push_docker_file_path: "./files/docker/image",
      build_and_push_image_version: "1.0.0",
    }
```

This will build a prometheus image. Tag it with the following tags
 * 1.0.0
 * 1.0
 * 1
 * latest
and push it to the given registry path.

### Just build the images

You don't want the images to get Pushed?

```
    - {
      role: build-and-push-docker-image,
      build_and_push_image_name: "registryhost.tld/image",
      build_and_push_docker_file_path: "./files/docker/image",
      build_and_push_image_version: "1.0.0",
      build_and_push_push: false
    }
```

### Just need a latest

```
    - {
      role: build-and-push-docker-image,
      build_and_push_image_name: "registryhost.tld/image",
      build_and_push_docker_file_path: "./files/docker/image",
      build_and_push_image_version: "1.0.0",
      build_and_push_push: false,
      build_and_push_tag_versions: false
    }
```
The explizit Version is always build. It is needed for the version comarison.