# CSCS OpenStack configuration files

## How it works

Here, you can find the "ideal" structure for a global and clean OpenStack configuration.
The configuration is splitted in different files, following the OpenStack recommendations, by defining several cloud configurations.
All these configurations are done in YAML format, and I also use YAML anchors (i.e. &username) and aliases (i.e. *username) in order
to avoir repeating settings.

I've put here an example of two users on the same dev machine, who share four common OpenStack projects:

* ich0XX (on Pollux)
* ich0XXhp (on Pollux)
* ich0XX-castor (on Castor)
* ich0XXhp-castor (on Castor)

Other cloud configurations (i.e. cscs-pollux-unscoped-token) are typically used by my Vagrant setup used in my Docker container in https://github.com/crochat/cscs-openstack.

All the configurations can be placed in one clouds.yaml file, but can also be divided in several files. OpenStack will check every configuration in a certain order.
Any clouds.yaml file in this paths list has precedence over the previous one, so if a configuration is redefined in this file, it will override the previous setting:

* /etc/openstack/
* ~/.config/openstack
* .

Special yaml files can also be used and are searched by OpenStack as well:

* clouds-public.yaml
* secure.yaml

Based on these specifications, I've defined something which is quite flexible and works perfectly well.

## Files

* /home/user1/.config/openstack/secure.yaml
  This file contains the private user file for the different cloud configurations. It can typically contain his credentials for all his
  clouds. This file should be owned by the user only, with permissions 600.
  **You'll have to replace these configurations with your own settings!**

* /etc/openstack/clouds.yaml
  This file contains the specific projects cloud configuration. As the global configurations are common to all projects, we don't have to
  set these here. In this file, the projects name and ID are required.
  **You'll have to replace them with your own configuration!**

* /etc/openstack/clouds-public.yaml
  This file contains all the global configurations which are CSCS Pollux and Castor oriented. All these are obviously common to all projects running on Pollux or Castor.
  You don't have to change anything in this configuration file.

## Commands

Once the configuration is done, you can use the --os-cloud parameter with openstack command to specify which cloud definition you would like to use.
Examples:

* Open an OpenStack CLI on Pollux:

```
openstack --os-cloud ich0XX
```

* Open an OpenStack CLI on Castor:

```
openstack --os-cloud ich0XX-castor
```

* List instances on Pollux:

```
openstack --os-cloud ich0XX server list
```

* List instances on Castor:

```
openstack --os-cloud ich0XX-castor server list
```
