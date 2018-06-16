# Learning Puppet 4

## What is Puppet

> Puppet manages configuration data, including users, packages, processes, and services—in other words, any resource of the node you can define.

Puppet is *declarative* - you declare the final state of your systems and their configuration. Unlike *imperitive* tools, you don't have to worry about:
* the current state of your servers
* adjust for each platform
* the effects of running your code several times in a roll (due to *idempotence*)

This way Puppet code is flexible, easy to read, less prone to breakage due to environment differences, and it achieves consistent, repeatable results. 

Puppet customizes the policy for each node based on stats, called *facts*, such as hostname, OS, memory, etc. Each node is evaluated and updated indepedently without waiting for any other node. 
![An illustration of how the Puppet master manages Puppet agents ](https://docs.google.com/drawings/d/e/2PACX-1vTFzwbRYBFchsTgTuXFR5rr73AbtW20FwrXyQPkzA0lJzqmA0pFNCbQKzaT37PYjXYIkokA2ct_TkgW/pub?w=960&h=720)
## Glossary

#### Idempotence:
In mathematics and computer science, idempotent operations are those that can be applied multiple times without changing the result beyond the initial application.

#### Node:
A node is a device managed by Puppet. 
Some nodes are masters, which compile manifests into catalogs; most nodes, including most masters, are agents, which receive catalogs and apply them to the node during a Puppet run. 
Most nodes are computers (such as workstations and servers), but some aren’t (such as supported network switches and storage appliances).

#### Puppet Master:
The Puppet master compiles and serves configuration catalogs on demand to Puppet agents on client nodes. The Puppet master provides catalogs to agents using an HTTP server. 

#### Puppet Agent:
A Puppet agent regularly performs Puppet runs, wherein it sends facts to a Puppet master and receives a configuration catalog, then applies the catalog to the local system using its providers.

#### Fact:
A piece of information about a node, such as its hostname, IP address, and operating system, is a fact. Facts are collected by Facter (Puppet’s system inventory tool).

#### Manifest (Puppet code):
The Puppet master service reads an environment’s main manifest. This manifest usually defines nodes, so that each managed agent receives a unique catalog.

#### Catalog:
A catalog describes the desired state of each managed resource on a node. It is a compilation of all the resources that the Puppet agent applies to a given node, as well as the relationships between those resources.
Catalogs are compiled by a Puppet master from manifests and agent-provided data (such as facts, certificates, etc), as well as optional external data (such as data from an external node classifier, exported resources, and functions). 
The master then serves the compiled catalog to the agent when requested.

These definitions and more can be found in [Puppet's offical glossary](https://docs.puppet.com/references/glossary.html)
