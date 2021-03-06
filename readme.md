[![Build Status](https://travis-ci.org/oloc/g10b.png)](https://travis-ci.org/oloc/g10b)
# Get your lab!
Many companies want a lab, a kind of research workspace. And so do some people. That's a good way to understand, learn, and improve some tools, let me say devops tools. And in order to setup a such lab, I propose this project G10B (numeronym for __Get your lab!__).

In my opinion a such lab should be an Infra-as-code Lab. So I decide to code my own lab. The goal of this project is to launch an installer and wait and see for the result. The side effect is that I can install it again and again, I can test improvements easily without fearing a crash. And in fact if I crash my lab I just have to reinstall my master branch. 

For this project I had several goals:
* learn a configuration management tool: I chose to learn Puppet
* learn a new language: I chose to learn Ruby
* use a community software factory: I chose to use [Jenkins](http://jenkins-ci.org/)
* use a piece of metrology: I chose to use the [EleasticSearch-Logstash-Kibana stack](http://elastic.co/)
* use a new piece of technology: I chose to use [Docker](http://docker.io/)
* make it open source, and learn how to use Git and GitHub

# What is done?
As a starter, Vagrant provisions the VMs and bootstraps a basic installation (puppet master or puppet agent). Puppet master installs itself and with the recurring appliance (*puppet agent --test*) installs the others servers.

Anyway, currently the project allows you automatically to:
* Vagrant pops the VMs with Puppet agents
* Bootstap installation of Puppet master
* Puppet-master configures itself
* Puppet configures all machines
* Feed Puppet-master with modules and manifests for g10b itself and others applications (as an example I take the famous petclinic)

Then Jenkins, GitLab, Rundeck, Mesos, Docker, Elasticsearch, Logstash, Kibana, shinken and Puppet are ready!

![Scheme](./docs/g10b.jpg)


![Pipeline](./docs/jenkins_pipeline.jpg)

## Prerequisites
To use this tools, you need a hosting plateform with:
* git
* Vagrant 
* VirtualBox

## Installation
On your host (for example your laptop with virtualBox), follow the step by step commands below:

    env GIT_SSL_NO_VERIFY=true git clone https://github.com/oloc/g10b.git 
    ./g10b/start

This command will pop the needed VMs in virtualbox and install the stuff.
If you possess a datacenter or a cloud and if you want to test the installation you have to launch the *install* scripts in the appropriate machines.

## Usage
You can access to the Jenkins, Gitlab, Rundeck, Mesos and Kibana in your browser at:
* http://\<module name\>.\<domain\> 
* http://g10b.oloc (by default)

## Configuration
The infrastructure is described in the __provision/provision.json__ file. 
The applications are described in the __app__ directory.
The environments are described in the __etc/puppet/environments__ directory.
The configuration of the lab is described in the puppet manifests and modules of the environment g10b.

If you modify the project settings, you have to add the DNS VM (g10b-gateway) as a resolver of your host (for example your laptop with virtualBox), regarding the IP addresses you configure in the *Vagrantfile* and in the *provision.json* (as the iplastdigit parameter). As default, the *start* initiate for you this command:

    echo 'nameserver 192.168.10.50' | sudo resolvconf -a *

## Applications description
All the applications of your lab can be described in the __app__ directory following some rules.
* One directory per application, named with the name of the application

## Environments description
All the environments of your lab can be described in the __etc/puppet/environments__ directory following some rules.
* One directory per environment, named with the name of the environment.
* Environment directory contains the 2 directories below:
  * modules
  * manifests
* Environment directory contains the file with the needed puppet module: __modules.lst__

## What if you want to tweak it?##
In order to help me to test some solutions avoiding to deploy all VMS, I have create a tool __install/config-master.sh__. The different arguments help to deploy, to clean, to update or to remove old puppet modules, even to update from a specific branch.

    sudo ./g10b/install/config-master.sh <arguments>

Without arguments, you just redeploy the G10B you have previously pull. The differents arguments are:    
* -c : clean
* -r : remove old puppet modules
* -u : update (pull the master branch or the specific branch of the -b argument )
* -m : force the update of the puppet modules with a --ignore-dependencies arguments
* -v : verbose
* -b <branch> : specific git branch to pull

## Versions
* V0.9 - Add Shinken. Many technical improvements.
* V0.8 - Add petclinic as an exemple in the Jenkins. Technical improvement: Application notion. g10b is considered as an application.
* V0.7 - Add Docker and Docker Registry. Technical improvements: Hiera variables revision.
* V0.6 - Add ELK (ElasticSearch Logstash Kibana)
* V0.5 - Technical version: Remove Import nodes. Add Environment directory to be Puppet-4-ready. Add Travis rake test.
* V0.4 - Vagrant uses own boxes. Infrastructure is revised. Gateway and DNS are activated. Mesos is available.
* V0.3 - Add Rundeck Jenkins and Gitlab
* V0.2 - Vagrant pops the infra, and bootstap the installation of Puppet-master. Puppet-master feeds and configures itself.
* V0.1 - script of installation of puppet-master on a system. It's more than a bootstrap. It's a bootstrap installing puppet-master and a feeder of modules and manifests needed to configure puppet itself by itself.


## My own configuration
I do not possess a datacenter or a cloud, so I proceed with VMs. To help you if you are in troubles, here is my own configuration:

* Operating System: Ubuntu 14.04.1 LTS, Trusty Tahr
* Kernel: 3.13.0-62-generic
* MoBo: W54_55SU1,SUW
* CPU: Intel(R) Core(TM) i7-4712MQ CPU @ 2.30GHz
* Memory: 2x 8GiB SODIMM DDR3 Synchrone 1600 MHz (0,6 ns)
* VirtualBox 4.3.10 r93012
