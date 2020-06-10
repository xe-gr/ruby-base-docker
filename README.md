## xe.gr Ruby base image

The base Ruby docker image we use for ruby projects.
View this image in [docker hub](https://hub.docker.com/r/xegr/ruby-base).

### building

Run the `build` script in the root directory of the repo.
To update the image version change the `IMAGE_VERSION` variable of the build script.

### pushing to registry

Push the image to docker registry. Assuming we built version `2.6.6-1` run:

```
docker push "xegr/ruby-base:2.6.6-1"
```

### upgrading ruby

Change the `FROM` directive at the beggining of the Dockerfile and also update the `RUBY_VERSION`
variable in the build script.
