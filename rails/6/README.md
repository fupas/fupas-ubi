# RAIL 6.0 & Ruby 2.5 container image
This container image includes Ruby 2.5 as a [S2I](https://github.com/openshift/source-to-image) base image for your RAILS 6.0 applications.

```bash
docker build -t ubi8/rails:6.0 -f Dockerfile.rhel8 .
```

or

```bash
oc create -f build-image.yaml -n openshift
```

## Reference

* https://github.com/sclorg/mariadb-container/ 
* https://github.com/sclorg/postgresql-container 

