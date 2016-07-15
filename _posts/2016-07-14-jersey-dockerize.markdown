---
layout: post
title: "How to Dockerize Java RESTful API Application"
tags: [docker, java, restful, api, dockerize]
---

The goal of this post is to show how to get a Java RESTful API application
(based on Jersey framework) into a Docker container. This guide assumes you
have [Docker](https://www.docker.com/),
[Java](https://www.oracle.com/java/index.html) and
[Maven](https://maven.apache.org/) installed.


## Create Jersey application

We'll use Maven archetype to create demo project using Jersey RESTful
framework. This project uses Grizzly HTTP server, so this is lightweight
alternative to running full container.

{% highlight no %}
mvn archetype:generate \
  -DarchetypeArtifactId=jersey-quickstart-grizzly2 \
  -DarchetypeGroupId=org.glassfish.jersey.archetypes -DinteractiveMode=false \
  -DgroupId=com.dekses -DartifactId=jersey-docker-demo \
  -Dpackage=com.dekses.jersey.docker.demo -DarchetypeVersion=2.22.2
{% endhighlight %}

After that we need to install dependencies and try to run this demo app.

{% highlight no %}
$ cd jersey-docker-demo

$ mvn install
...

$ mvn exec:java
...
Jul 14, 2016 5:07:03 PM org.glassfish.grizzly.http.server.NetworkListener start
INFO: Started listener bound to [localhost:8080]
Jul 14, 2016 5:07:03 PM org.glassfish.grizzly.http.server.HttpServer start
INFO: [HttpServer] Started.
Jersey app started with WADL available at
http://localhost:8080/myapp/application.wadl
Hit enter to stop it...
{% endhighlight %}

Let's test how it works by sending GET request with `curl`. If everything works
fine server should respond with "Got it!" text message.

{% highlight no %}
$ curl http://localhost:8080/myapp/myresource
Got it!
{% endhighlight %}


## Create Dockerfile

File called `Dockerfile` works as an instruction for building and running your Docker
image. For our demo project it will:

* define base image (with maven and jdk on-board)
* create application directory
* install dependencies using Maven
* expose local container port 8080
* run application

Full Dockerfile content (also available on
[github](https://github.com/dekses/jersey-docker-demo/blob/master/Dockerfile)).

{% highlight docker %}
# fetch basic image
FROM maven:3.3.9-jdk-8

# application placed into /opt/app
RUN mkdir -p /opt/app
WORKDIR /opt/app

# selectively add the POM file and
# install dependencies
COPY pom.xml /opt/app/
RUN mvn install

# rest of the project
COPY src /opt/app/src
RUN mvn package

# local application port
EXPOSE 8080

# execute it
CMD ["mvn", "exec:java"]
{% endhighlight %}


## Update server address

Another step is necessary before we can start building and running Docker
containers. By default Jersey application listens for incoming request on the
"localhost". This is defined by `BASE_URI` in
jersey-docker-demo/src/main/java/com/dekses/jersey/docker/demo/Main.java file
on line 16.

{% highlight java %}
// Base URI the Grizzly HTTP server will listen on
public static final String BASE_URI = "http://localhost:8080/myapp/";
{% endhighlight %}

This is not going to work well in Docker container because network interface
created inside container and available for external requests is different.
That's why we need to instruct Jersey to listen on all available addresses, to
do that replace "localhost" with "0.0.0.0".

{% highlight java %}
// Base URI the Grizzly HTTP server will listen on
public static final String BASE_URI = "http://0.0.0.0:8080/myapp/";
{% endhighlight %}


## Build docker image

This done with `docker build` command. Run it in your project directory.
Additional `-t` flag allows to tag your image so it's easier to find and run
later.

{% highlight no %}
docker build --tag=myapp .
{% endhighlight %}


## Run the image

Running your image with `-t -i` options allows executed process to run
attached to your terminal. This is development mode options, they allow you to
see output from your image immediately.

Option `-p` publishes or maps host system port 18080 to container port 8080,
otherwise you can not access your API service from the host system.

Image name is `myapp` assigned during build phase.

{% highlight no %}
docker run -p 18080:8080 -t -i myapp
{% endhighlight %}

Okay we can now test docker container with the same curl command but directed
to port 18080 instead of 8080 for local Java app.

{% highlight no %}
$ curl http://localhost:18080/myapp/myresource
Got it!
{% endhighlight %}

Hope this tutorial helped you get up and running a simple Java API as a Docker
container. Full source code is uploaded to Github 
[jersey-docker-demo](https://github.com/dekses/jersey-docker-demo) project.
