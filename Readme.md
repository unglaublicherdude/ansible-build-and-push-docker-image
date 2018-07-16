## Build and Push Docker Images

This role lets you easy build (lokal) and push a docker image. Just prepare an Image (Dockerfile) in a directory within your playbook and configure the role with parameters.

It will skip te build, if the image already exists locally. So it is important, that you change the Version if you want to build a new image.

Don't change the vars in this role directly. Use role-vars (see example).

This role has the python dependency:
docker-py

It will fail, if the dependency is not installed.

If you don't want to push your image into a registry, use the transfer function of this playbook. The image will be exported (docker save), transfered to the server and imported (docker load). It will even tag it on the server as it would be in a registry.

## Changelog

 * v2.0.4 (16.07.2018)
   * Added build argument passing to Dockerfile.
 * v2.0.3 (10.04.2018)
   * Added the ability to transfer an image pulled from a registry - without build.
 * v2.0.2 (10.04.2018)
   * Just some readme fixes.
 * v2.0.1 (10.04.2018)
   * Check if version actually contains dots.
 * v2.0.0 (06.04.2018)
   * You can now also transfer an image to a server without going through a registry.

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

### Transfer without push to a registry

```
    - {
      role: build-and-push-docker-image,
      build_and_push_image_name: "registryhost.tld/image",
      build_and_push_docker_file_path: "./files/docker/image",
      build_and_push_image_version: "1.0.0",
      build_and_push_push: false,
      build_and_push_transfer: true,
      build_and_push_transfer_tmp_image_dir: /tmp/docker/images # on the server
    }
```

### Transfer without build and push

```
    - {
      role: build-and-push-docker-image,
      build_and_push_image_name: "jwilder/whoami",
      build_and_push_image_build: false,
      build_and_push_push: false,
      build_and_push_transfer: true,
      build_and_push_transfer_tmp_image_dir: /tmp/docker/images,
      build_and_push_image_version: "latest"
    }
```
### Handing build arguments to the Dockerfile

```
    - {
      role: build-and-push-docker-image,
      build_and_push_image_name: "registryhost.tld/image",
      build_and_push_docker_file_path: "./files/docker/image",
      build_and_push_image_version: "1.0.0",
      build_and_push_args: 
      {
        some_password_from_the_vault: "{{some_password_from_the_vault}}",
        myfancypath: "{{myfancypath}}"
      }      
    }
```