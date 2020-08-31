# Antora Build for Apicurio Registry Documentation

This folder contains the configuration and scripts used when building the Apicurio Registry 
documentation for publishing to the Apicurio project web site.  We are using a tool called
Antora to build the asciidoc based documentation into a publishable site.  However, due to
some issues with building on various platforms (I'm looking at you, Windows) and also some
conflicts between what Antora builds and what our GitHub Pages (jekyll) project site expects,
we have created a non-trivial (but automated) process for building the docs.

## Docker Image

In order to avoid platform inconsistencies with the build, we have created a Docker image to
perform it.  The `Dockerfile` is located in this directory, along with a `build.sh` script
that becomes the main entrypoint/cmd for the docker image.

## Build the Docker Image

To build the docker image, do this:

```docker build -t="apicurio/apicurio-docs-builder" --rm .```

Then you can push the new image like this:

```docker push apicurio/apicurio-docs-builder:latest```

Only do this if you need to change the build script or some other aspect of the Antora build
process.  See the `build.sh` file for details about how Antora is used to build the docs.

## Building the Documentation

The Docker image should be used to build the documentation.  It has several (optional) inputs
and one required volume-mount to store the output.  You can run the build something like this:

```
docker pull apicurio/apicurio-docs-builder:latest
docker run '-v=C:\Users\ewittman\git\apicurio\apicurio.github.io\registry\docs:/antora-dist' -it apicurio/apicurio-docs-builder:latest
```

This will run the build and output the result in the provided local directory (in the above
example it would write the result to the appropriate Apicurio project site directory).

The following ENV variables can be set to customize what gets built:

* **ENV PLAYBOOK** local-antora-playbook.yml

Feel free to override the default values to customize the build.
