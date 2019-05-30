# Playbook and Patches to Prepare Hosts/VMs for CoreMedia

`coremedia-node-preparation`

The [Ansible][ansible] playbook in this repository is intended for preparation 
of CI systems and other hosts, which need to be able let [CoreMedia][coremedia] 
CMS-9 or LiveContext 3 run on the or build the workspaces for them without any 
other installation steps necessary.

The supported target operating systems are CentOS, Debian, RedHat, and Ubuntu.

Find mirrors of this repository at [gitlab][gitlab] and [github][github].


## Setup of the Environment

We rely on the roles `coremedia` for all coremedia hosts and `cmdev` for 
development hosts.

The role `coremedia` provides the (not supported but happily working) OpenJDK 
for users which do not want to include Oracle Java from the default CoreMedia 
chef node configuration.

The role `cmsdev` provides the needed development tools to build a workspace.


## Special Handling for RedHat/CentOS

After the setup via this playbook please reboot the host to let all setting 
take effect.


## Special Handling for Debian/Ubuntu

Ubuntu is not on the list of supported platforms for CoreMedia. Since only
minor changes to the deployment scripts are necessary, this is now our
favorite OS option. We are supporting und running this for customers since
2018 in production.

### Patching the Workspace

The patches in this workspace were prepared to patch a 1710 CMS-9 workspace
to be deployable as a single node configuration on a Ubuntu based host.

Since then they have been used nearly unchanged for installations up to
platform release 1904.

### Additional Ansible Preparation

Install python on your node since otherwise [Ansible][ansible] will not start.

The recipes in here also need a pre-installed ohai.

```
apt-get install python
apt-get install ohai
```

## Preparation

Configure relevant hosts in `inventory.properties` or your global 
[Ansible][ansible] hosts file (if you haven't done so).


## Usage

Run with: 

```
ansible-playbook -i inventory.properties setup.yml
```


## Collection

This playbook provides:

* Java 8 
* mod_jk for Apache httpd

for hosts in role `coremedia`, and

* PhantomJS
* Maven
* [SenchaCMD][sencha]

for hosts in role `cmdev` .


## Deployment of the System to Ubuntu

To deploy CoreMedia CMS-9 to Ubuntu based systems, you will need to apply the
patches provided in this workspace to the chef scripts in your workspace.

### AJP Usage

The usage of AJP is not strictly necessary, but the ability to configure the protocol
in use might still help.

We worked around the fact that hosts directly connected to the internet use an
IP-Address that is not on the default list of Apache Tomcat's

[RemoteIpValve](https://tomcat.apache.org/tomcat-7.0-doc/api/org/apache/catalina/valves/RemoteIpValve.html)

Alternatively you could also change the default in `conf/server.tml` during
deployment.

## Feedback

Please use the [issues][issues] section of this repository at [github][github] 
for feedback. 

[issues]: https://github.com/provocon/coremedia-ubuntu-development/issues
[sencha]: https://www.sencha.com/products/extjs/cmd-download/
[ansible]: https://www.ansible.com/
[coremedia]: https://www.coremedia.com/
[github]: https://github.com/provocon/coremedia-node-preparation
[gitlab]: https://gitlab.com/provocon/coremedia-node-preparation
