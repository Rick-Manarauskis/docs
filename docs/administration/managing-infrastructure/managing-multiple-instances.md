---
title: Managing Multiple Instances
description: Octopus and Tentacle Manager both have the ability to manage multiple instances.
position: 120
---

In normal usage, there is one Octopus Deploy Server, and one instance of the Tentacle agent running on each of the machines that you plan to deploy to, or deployment targets configured for Azure or SSH endpoints. But sometimes it's necessary to run multiple copies of Octopus and Tentacle on the same machine, perhaps with different configuration, or running under different user accounts. To support this, Octopus and Tentacle have the notion of "Instances".

## Creating and Managing Instances {#Managingmultipleinstances-Creatingandmanaginginstances}

Octopus and Tentacle Manager both have the ability to manage multiple instances. You can launch Octopus Manager or Tentacle Manager via the Windows start screen. Then you can use the instance selector drop down to create or manage instances:

![](3278042.png)

You can use this drop down to create new instances, or to switch between managing instances.

When creating an instance, you will be asked to provide a name. Each instance needs a unique name.

![](3278041.png)

After adding an instance, you'll then be asked to walk through the setup wizard for that instance.

![](3278040.png)

Each configured instance has its own configuration files, home directory and Windows Services. For example, this machine is configured with many instances of Tentacle:

![](3278043.png)

## Command Line {#Managingmultipleinstances-Commandline}

All wizards that you follow in the Octopus or Tentacle Manager provide the ability to export a command-line script of the actions taken. This can be done with the **Show Script** option at the end of every wizard:

![](3278039.png)

All command line operations accept the instance name as an argument. For example, to stop and start a running Tentacle, the command would usually be:

```bash
Tentacle.exe service --stop --start
```

When no instance is specified, the command will apply to the default instance. To specify an instance, use the `--instance` argument:

```bash
Tentacle.exe service --stop --start --instance "Tentacle"
```

You can export a script from the wizard to see what the command-line equivalent would look like, and then change the instance name as appropriate.

## Considerations for Octopus Server Instances {#Managingmultipleinstances-ConsiderationsforOctopusServerinstances}

Different instances of Octopus Server:

- Listen on separate web URLs (e.g. the same server may host [http://my-octopus/group1,](http://my-octopus/group1,)[http://my-octopus/group2,](http://my-octopus/group2,) and [http://test-octopus:81](http://test-octopus:81/)).
- Have completely separate SQL Server databases.
- Have completely separate environments, projects, teams, users and permissions.
- Run in separate Windows services, potentially under different user accounts.
- Can share Tentacle machines (but don't have to).
- Run from the same on-disk executables (the MSI installer is only run once).

:::problem
**Multiple-instance &quot;gotchas&quot;**
There are a few things to keep in mind when running multiple Octopus Server instances.

- When [upgrading the Octopus Server](/docs/administration/upgrading/index.md), each instance of the **Windows service should be stopped first by hand**; the default installation process sometimes seems to ignore non-default instances and won't stop them before replacing files, nor restart them afterwards.
- Each instance is **backed up separately**, so don't forget to [configure backup for each one](/docs/administration/data/backup-and-restore.md).
- Each instance has its own SQL Server database, with a different **Master Encryption Key**; make sure [the key for each instance is recorded somewhere safe](/docs/administration/security/data-encryption.md).
  :::

## Considerations for Tentacle Instances {#Managingmultipleinstances-ConsiderationsforTentacleinstances}

Different instances of Tentacle need to listen on different TCP ports, and should install applications to a different base directory.

### Upgrading Multiple Instances
Upgrades of Tentacles deployed on the same machine are all done at the same time, in other words, if you have multiple Tentacles running on the same machine, when the upgrade is run, all Tentacles will be upgraded.  
The automatic Tentacle upgrade from Octopus feature does support upgrading multiple instances on the same machine.  
If Tentacles are running under different accounts, please ensure the [upgrade account](/docs/infrastructure/machine-policies.md#MachinePolicies-TentacleUpdateAccount) has enough rights to upgrade all Tentacles.


## Deleting Instances {#Managingmultipleinstances-Deletinginstances}

If you no longer need an instance, you can delete it from the Octopus or Tentacle Manager.

![](3278038.png)
