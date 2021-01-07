# Pass a env var into a build context of kaniko

If you would like to pass a env var into the build context of kaniko you have to declare the `ARG` in the docker file and use `--build-arg key=value`.
