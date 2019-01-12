# Learning Puppet 4

## What is Puppet

From [the Learn Puppet VM](https://learn.puppet.com/)
> When we talk about Puppet, we're often using the name as a shorthand for a collection of tools and services that work in concert to help you define how you want the systems in your infrastructure configured, and then automate the process of bringing those systems into your desired state and keeping them there. 

From the introduction to *Learning Puppet 4*
> Puppet manages configuration data, including users, packages, processes, and services—in other words, any resource of the node you can define.

Puppet is *declarative* - you declare the final state of your systems and their configuration. Unlike *imperitive* tools, you don't have to worry about:
* the current state of your servers
* adjust for each platform
* the effects of running your code several times in a roll (due to *idempotence*)

This way Puppet code is flexible, easy to read, less prone to breakage due to environment differences, and it achieves consistent, repeatable results. 

Puppet customizes the policy for each node based on stats, called *facts*, such as hostname, OS, memory, etc. Each node is evaluated and updated indepedently without waiting for any other node. 
![An illustration of how the Puppet master manages Puppet agents ](https://docs.google.com/drawings/d/e/2PACX-1vTFzwbRYBFchsTgTuXFR5rr73AbtW20FwrXyQPkzA0lJzqmA0pFNCbQKzaT37PYjXYIkokA2ct_TkgW/pub?w=960&h=720)
## Updating from Puppet 3 to 4

### Deprecated features, introductions and actions:

* **Ruby DSL** - search for manifests that have the .rb extension and rewrite them in the Puppet configuration language
* **Configuration File Environments** - replace with directory environments by moving module path settings from puppet.conf to an environment.conf file in each environment’s directory
* **Node inheritance** - use external node classifiers instead
* **Puppet Kick**
* **Qualifying Relative Class Names** - replace relative type or class declarations with their fully qualified names: include mymodule::ntp:
* **Search function** - remove it and replace with fully qualified name
* **Import** - move the imported manifests into a Puppet module, and include the module class instead: include commonlib::functions; remove the irrelevant ignoreimport setting from your configuration files 
* **Documenting Modules with Puppet Strings**  - The puppet doc command now only documents Puppet commands. Module documentation once provided by puppet doc has been replaced by puppetlabs-strings, which generates a complete documentation tree for the module in Markdown format.
* **Tagmail Report Processor** - no longer inclulded by default, can be found 
* **String interpretation** - quote all strings to avoid misinterpretation; quote strings that may be misinterpreted as numbers.
* **Some PuppetDB queries** - ActiveRecord storeconfigs, inventory service, puppet facts upload command (which utilized the inventory service), puppet facts find --terminus rest command (which utilized the inventory service). Adjust manifests that need these features to use the PuppetDB API.
* **File Mode is no longer numeric** - you must use a string value containing the octal number for file modes: mode => '0644'
* **Qualifying Defined Types** - defined types created in modules need to be prefixed with the module name
* **Assumed permissions** - Add specific owner, group, and mode attributes to all file resources.

Introduced features:
* **Boolean types** - Puppet 4 introduced the Boolean data type, which does not compare equally with a String value. Check each instance of true or false to determine if it should be tested as a string, or boolean. Empty strings evaluate to boolean true.  This can trip you up if you had code that depended on a false evaluation so replace "if ! $empty_string " with "if $empty_string == ''"
* **Ammended Cron Purge** - Previous cron purge invocations would remove unmanaged cron entries from only the user that Puppet was running, this will now purge unknown cron entries for every user.
* **Adjusting Networking Facts** - With the latest version of Facter, many of the network-related facts have been restructured into hashes that allow much more intelligent retrieval and analysis of interfaces.

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
