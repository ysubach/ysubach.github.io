---
layout: post
title: "DC/OS: local deployment using Vagrant"
tags: [dcos, cloud, cluster, vagrant]
---

DC/OS (Datacenter Operating System) is an open source platform enabling enterprises
to easily build and run modern apps in production. It literally abstracts
datacenter into a single computer. DC/OS is suitable for various application
types and infrastructure agnostic by design. It runs on top of any bare-metal,
private or public clouds. Even local deployment on a single machine is
possible and quite easy (useful for testing, development and demo purposes). 


## Local deployment with Vagrant

Before start make sure you have latest versions of 
[VirtualBox](https://www.virtualbox.org/wiki/Downloads) and 
[Vagrant](https://www.vagrantup.com/downloads.html) installed on your system.

Everything is tested on Mac OS X but DC/OS Vagrant package have been reported
to work also on Windows (7, 10), Ubuntu (14, 15, 16), Fedora 23, Arch Linux.
Minimal requirement for RAM is 5GB.

1) Clone "dcos-vagrant" from GitHub:

{% highlight no %}
git clone git@github.com:dcos/dcos-vagrant.git
{% endhighlight %}

2) Download latest DC/OS package into dcos-vagrant folder:

{% highlight no %}
cd dcos-vagrant
curl -O https://downloads.dcos.io/dcos/EarlyAccess/dcos_generate_config.sh
{% endhighlight %}

3) Install Vagrant Host Manager Plugin, it manages the "/etc/hosts" on the VMs
and host to allow access by hostname.

{% highlight no %}
vagrant plugin install vagrant-hostmanager
{% endhighlight %}

4) Configure the DC/OS installer, it requires Vagrant config file and
environment variable which selects DC/OS configuration.

{% highlight no %}
cp VagrantConfig.yaml.example VagrantConfig.yaml
export DCOS_CONFIG_PATH=etc/config-1.7.yaml
{% endhighlight %}

5) Disable authentication. DC/OS cluster uses external OAuth by default but it is
more convenient to have it disabled for local testing.

{% highlight no %}
echo "oauth_enabled: 'false'" >> etc/config-1.7.yaml
{% endhighlight %}

6) Start minimal DC/OS cluster which has one master node and one agent node.
Depending on your Internet connection speed you'll need to wait some time
for download of OS images and setup of VirtualBox VMs.

{% highlight no %}
vagrant up m1 a1 boot
{% endhighlight %}

## Check main dashboard

After successful execution of last "vagrant up" command you can check that
your minimal cluster is up and running. This is simple. Just open following
URL in your browser:

{% highlight no %}
http://m1.dcos/
{% endhighlight %}

You'll see something like screen shot below.

![DC/OS Local Dashboard](/assets/blog-2016/dcos-local-dashboard.png)

## Doing more

So enjoy your day with new DC/OS cluster! You can do more now: install various
packages, scale the cluster, deploy your own applications. 

When you are done
use following command to shutdown all VMs:

{% highlight no %}
vagrant destroy -f
{% endhighlight %}
