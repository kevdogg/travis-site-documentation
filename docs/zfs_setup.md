---
layout: default
title: ZFS
permalink: /zfs/
nav_order: 4
---
<div class="outerbox">
  <div class="box">
    <div> <img src="/assets/images/dockerhero.jpg" alt="Docker"></div>
{% comment %}    
    {% assign key = 'docker' %}
    {% assign URL = site.data.links | map: key | first | prepend: site.baseurl %}
{% endcomment %}
    <div><a href="{{ site.data.links.docker }}">Docker</a></div>
  </div>
  <div class="box">
    <div> <img src="/assets/images/logo-open-zfs.png" alt="Open-ZFS"></div>
    <div><a href="{{ site.data.links.open_zfs }}">OpenZFS</a></div>
  </div>
</div>

<br>

# Contents
{: .no_toc}

1. Docker and the use of External Volumes
{:toc}

## Docker and the use of External Volumes

Docker is utilized within the authelia setup to provide mysql and redis services.  Even the main authelia applicationis run within a docker application

External Volumes or Volumes are used within Docker to maintain persistent data that is intended to be saved between uses of the container.  In a nutshell, volumes are used to save the important data created when running authelia. The use of external volumes separates the data from the container itself.  The external volume(s) is/are mounted within the docker container at the time of container creation.  Data is written to these volumes during use of the container.  When the container is stopped or destroyed, the external volume(s) is/are unmounted and the state of the data is preserved.  

External Volumes usually refer to directories (which are usually read/writeable) or files (which are usually only readable) within the host system running docker. Volums used in this documentation are:

  * /etc/authelia/config/configuration.yml - Main authelia configuration file
  * /etc/authelia/config/users.yml - User database of specifying users/groups who are able to register and authenticate with the authelia system
  * /var/data/db - mySQL/MariaDB used to store login two factor data

## ZFS Setup

_Instructions in this section are only relevant if your underlying file system is ZFS_,

If using ZFS as the host's underlying file system, its easy to leverage the zfs' snapshot capability in order to save versioned copies of the authelia datasets and automate backup of these volumes to another systemrunning zfs.

[John Ramsden's guide]({{ site.data.links.john_ramsden_zfs_guide }})[^1] for setting up ZFS mounts and directories is a great read if you are not familiar with the ZFS concepts and setting up datasets.

If using authelia, I recommend setting up the following zfs datasets (if not already setup):
  * etc/authelia
  * var/data/db

Using your zfs pool's name is _tank_, these datasets could be setup with:

```bash
[root]# zfs create -o compression=lz4 -o mountpoint=/etc/authelia tank/etc/authelia
[root]# zfs create -o compression=lz4 -o xattr=sa -o atime=off -o mountpoint=/var/data/db tank/var/data/db
```

You may need to import/export the pool to prevent later issues:

```bash
[root]# zfs umount -a
[root]# zfs export tank
[root]# zfs import tank
```

Once zfs datasets are setup, you may want to configure the zfs autosnap functionality. Two tools that I use and recommend are [zfs-auto-snapshot]({{ site.data.links.zfs-auto-snapshot }})[^2] and [ZnapZend]({{ site.data.links.znapzend }})[^3]. Depending on your linux system, there are packages that are prebuilt:

* Ubuntu
  - zfs-auto-snapshot (Available in multiverse respository)
  - [znapzend]({{ site.data.links.znapzend_ubuntu_ppa }})  (Available as ppa)
* Arch
  - [zfs-auto-snapshot-git]( {{ site.data.links.zfs-auto-snapshot_arch_aur }}) (Available in AUR)
  - [znapzend]({{ site.data.links.znapzend_arch_aur }}) (Available in AUR)

## Permissions

Discussion regarding Volume/User permission may be needed


## References
[^1]: [John Ramsden's ZFS_Guide]({{ site.data.links.john_ramsden_zfs_guide }})
[^2]: [zfs-auto-snapshot]({{ site.data.links.zfs-auto-snapshot }})
[^3]: [ZnapZend]({{ site.data.links.znapzend }})

