---
layout: post
title: "How to set up Cassandra test cluster using Docker"
tags: [cassandra, docker, test] 
---

In this post I'll walk you through set up of the test [Apache
Cassandra](http://cassandra.apache.org/) cluster in a Docker environment. This
is useful if you want to experiment with this NoSql database or need it as part
of your development environment.

First of all, you need to have [Docker](https://www.docker.com/) installed on
your system. 

If you use it from Mac OS X, like me, then please make sure your VirtualBox VM
instance has enough memory for running Cassandra cluster.

1. Open "VirtualBox" app
2. Find the VM, in my case it was "default"
3. Open "Settings", then "System"
4. Make sure it has at least 8GB available

Now we are ready to begin the fun part.


## Single instance

Let's run single Cassandra instance and then check that it is up using 
`docker ps` command:

{% highlight no %}
$ docker run --name cassandra1 -m 2g -d cassandra:3.0.4 
c8d489f00f1778535c7806c92ca17707e9876a872f28b97cd4a05eb265b7af38

$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                         NAMES
c8d489f00f17        cassandra:3.0.4     "/docker-entrypoint.s"   10 seconds ago      Up 10 seconds       7000-7001/tcp, 7199/tcp, 9042/tcp, 9160/tcp   cassandra1
{% endhighlight %}

Note that there is `-m 2g` option for `docker run` command, it gives 2GB RAM
to this container. If you are curious run `docker logs cassandra1` to see log
message produced during Cassandra start up.

Connection to Cassandra instance with `cqlsh` is not difficult. Just need find
out current container IP address.

{% highlight no %}
{% raw %}
$ docker inspect --format='{{ .NetworkSettings.IPAddress }}' cassandra1
172.17.0.2

$ docker run -it --link cassandra1 --rm cassandra:3.0.4 \
  sh -c 'exec cqlsh 172.17.0.2'
Connected to Test Cluster at 172.17.0.2:9042.
[cqlsh 5.0.1 | Cassandra 3.0.4 | CQL spec 3.4.0 | Native protocol v4]
Use HELP for help.
cqlsh>
{% endraw %}
{% endhighlight %}


## Cluster

To form the cluster we need to run second Cassandra container `cassandra2` and
connect it to `cassandra1`.

{% highlight no %}
{% raw %}
$ docker run --name cassandra2 -m 2g -d \
  -e CASSANDRA_SEEDS="$(docker inspect --format='{{ .NetworkSettings.IPAddress }}' cassandra1)" \ 
  cassandra:3.0.4
{% endraw %}
{% endhighlight %}

Ok, now we have a cluster with two instances. Let's verify that using
`nodetool status` command which need to be executed inside the container.

{% highlight no %}
{% raw %}
$ docker exec -i -t cassandra1 sh -c 'nodetool status'
Datacenter: datacenter1
=======================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
--  Address     Load       Tokens       Owns (effective)  Host ID                               Rack
UN  172.17.0.3  101.84 KB  256          100.0%            fb3d0aef-d70e-4637-8bdf-7e861acfcea6  rack1
UN  172.17.0.2  107.36 KB  256          100.0%            ccd1000e-1b9a-422a-a2b4-d291da9c0585  rack1
{% endraw %}
{% endhighlight %}

More instances can be added to this cluster using the same approach. Before
running other instances make sure that "nodetool" returns "Status=Up" for all
existing cluster nodes.

I hope it was easy!

