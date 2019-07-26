RAIL 6.0 & Ruby 2.5 container image
=================
This container image includes Ruby 2.5 as a [S2I](https://github.com/openshift/source-to-image) base image for your RAILS 6.0 applications.

docker build -t ubi8/rails:6.0 -f Dockerfile.local .

oc create -f build-image.yaml -n openshift