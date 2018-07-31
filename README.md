```
gcloud components install docker-credential-gcr
docker-credential-gcr gcr-login
```

```
# Builds to a container image registry.
$ mvn compile jib:build
# Builds to a Docker daemon.
$ mvn compile jib:dockerBuild
```