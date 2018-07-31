# JIB test

This repository will show a small example how to use JIB from google to build your images.

First of you need to configure the jib-maven-plugin. As you see below there is so many flags and switches to add but you usually only need to supply the to/image and container/mainClass as these are required to specify where to put the result and what class you want to run inside of the image.
```
<configuration>
  <from>
    <image>openjdk:alpine</image>
  </from>
  <to>
    <image>localhost:5000/my-image:built-with-jib</image>
    <credHelper>osxkeychain</credHelper>
  </to>
  <container>
    <jvmFlags>
      <jvmFlag>-Xms512m</jvmFlag>
      <jvmFlag>-Xdebug</jvmFlag>
      <jvmFlag>-Xmy:flag=jib-rules</jvmFlag>
    </jvmFlags>
    <mainClass>mypackage.MyApp</mainClass>
    <args>
      <arg>some</arg>
      <arg>args</arg>
    </args>
    <ports>
      <port>1000</port>
      <port>2000-2003/udp</port>
    </ports>
    <format>OCI</format>
  </container>
</configuration>
```

Before you run the jib:build command you might want to setup your credentials for the container repository. Below you can see an example of how to setup the gcloud google container registry credentials.
```
gcloud components install docker-credential-gcr
docker-credential-gcr gcr-login
```

After the credentials are setup you can run the jib:build below in order to build the image and push it directly to the container registry. If you rather build the image locally to your docker installation you can run jib:dockerBuild.
```
# Builds to a container image registry.
$ mvn compile jib:build
# Builds to a Docker daemon.
$ mvn compile jib:dockerBuild
```

If you have special credentials for your repository you might want to try the setup below in order to specify your credentials. For the google container repository this step is not required but I thought I mention it.
```
<settings>
  <servers>
    <server>
      <id>MY_REGISTRY</id>
      <username>MY_USERNAME</username>
      <password>{MY_SECRET}</password>
    </server>
  </servers>
</settings>
```

Lastly I add this little command so you can see which image you have in our setup and clean up with prune when your done. Notice that that command will remove all images in your environment so DO NOT RUN in production.
```
docker images
docker system prune -a
```