---
layout: post
title: "How to install Scala and sbt to Ubuntu 12.04 LTS"
tags: [linux, ubuntu, scala, sbt] 
---
[Scala](http://scala-lang.org) 
is a really nice JVM based programming language. This post
explains how to install it to Ubuntu 12.04 LTS system. If you don't
just play with Scala console but want to start a project, then you need
[Simple Build Tool (sbt)](http://www.scala-sbt.org/)
installed as well.

First of all let's get root priviledges, update packages index and
install some basic must-have things:

{% highlight shell %}
sudo su -
apt-get update
apt-get -y install curl git vim
{% endhighlight %}

Install Open JDK 7 and Scala:

{% highlight shell %}
apt-get install -y openjdk-7-jdk
apt-get install -y scala
{% endhighlight %}

Test from the command line:

{% highlight shell %}
$ java -version
java version "1.7.0_25"
OpenJDK Runtime Environment (IcedTea 2.3.10) (7u25-2.3.10-1ubuntu0.12.04.2) 
OpenJDK 64-Bit Server VM (build 23.7-b01, mixed mode)

$ scala
Welcome to Scala version 2.9.1 (OpenJDK 64-Bit Server VM, Java 1.7.0_25).
Type in expressions to have them evaluated.
Type :help for more information.

scala>
{% endhighlight %}

Now you have Scala version 2.9.1, it's not latest but for each project
you'll have a possibility to install required version through `sbt`. So
let's go ahead and install it. 

Download and install sbt-0.13:
{% highlight shell %}
wget http://repo.scala-sbt.org/scalasbt/sbt-native-packages/org/scala-sbt/sbt/0.13.0/sbt.deb --no-verbose
dpkg -i sbt.deb
{% endhighlight %}

Now you have `sbt` available from the command line. It's time to return to
your normal user login because sbt creates separate scala instances
for each username. Next ask sbt to show scala console: `sbt console`. It
will force sbt to download and deploy scala-2.10.2 which is default for
sbt-0.13.

That's it, enjoy your day with Scala and sbt!
