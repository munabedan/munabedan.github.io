---
layout: post
title:
  "Incul-manager: an attempt at recreating qubes os functionality  with Incus
  containers and XPRA"
date: 2024-05-28 00:00
categories:
  - Linux
  - DevOps
tags:
  - linux
  - devops
image:
  path: /images/incul_manager/work_container_menu.png
---

## Introduction

I will begin this article by prefacing that this is just a hobbyist project and that I am in no way shape or form a security expert. Qubes OS is a security first distro that has a very hard working security team overseeing your security.

<https://www.qubes-os.org/doc/security-design-goals/>

<https://www.qubes-os.org/security/>

Having worked on this tool I do understand why Qubes OS does not use containers as far as security is concerned. You can also reference the replies to my question in the Qubes OS subreddit.

<https://www.reddit.com/r/Qubes/comments/1cxl80q/why_does_qubes_use_vms_instead_of_containers/>

You can also visit the more detailed documentation for details on the Qubes OS architecture

<https://www.qubes-os.org/doc/#system>

I began working on this tool without a plan, and it shows , the code is currently a mess of scripts that somehow managed to achieve something worth showing. (Don't contact me for butchering clean code practices). 

For issues or suggestions, please visit the [GitHub repository](https://github.com/munabedan/incul-manager) or leave a comment on this article.

Now with the disclaimers aside let's get to the project.

## The goal

I have used Qubes OS on my machine for some time, but I found it a bit heavy on my machine as it does require very fast SSDs and lots of RAM which mine lacked.

So I decided to take it upon myself and see to what extent I can recreate Qubes OS functionality especially the compartmentalization of different software using containers.

Below are the main points I needed to recreate to consider the system usable:

- Seamless access to applications
- Integration of container applications to menus
- Color indicators for different windows

## Architecture

The tool is built on [Incus](https://linuxcontainers.org/incus/docs/main/), which advertises itself as a modern, secure and powerful system container and virtual machine manager.


According to their documentation 

> The Incus project was created by Aleksa Sarai as a community driven alternative to Canonical's LXD.
> Today, it's led and maintained by much of the same people that once created LXD.

I mainly used it as an alternative to Lxd because I dislike Ubuntu Snaps which is the only way of installing Lxd.


The other tool is [XPRA](https://github.com/Xpra-org/xpra) which allows us to connect to X11 applications running on a remote server with ssh. We use this and other features it provides to connect to applications running within the containers.

## The tool at work


To use this tool you will have to first have a running installation of debian XFCE.

![Desktop View](/images/incul_manager/begin_with_debian_xfce.png){: width="972" height="589" }
_A default install of Debian XFCE_

I have packaged the tool as a .deb package that runs on the command line so running the program gives you this.

![Desktop View](/images/incul_manager/installed_incul_manager.png){: width="972" height="589" }
_Installed incul manager_

The next step is to run `incul-manager init` which will first configure debian backports to fetch install incus system container and virtual machine manager.

![Desktop View](/images/incul_manager/set_up_backports.png){: width="972" height="589" }
_Running incul manager init_

During initialization, the default menu will be replaced with a custom menu that groups all items into System Tools.

![Desktop View](/images/incul_manager/modify_application_menu.png){: width="972" height="589" }
_Modified application menu_

The next step is to run `incul-manager create-template` which will create a debian based incus system container. This template container will have the following installed `thunar xfce4-terminal xfce4-notifyd xfwm4 openssh-server xpra openssl`, so we dont have to reinstall them each time when creating containers.

![Desktop View](/images/incul_manager/create_template.png){: width="972" height="589" }
_Create template_

We can also run `incul-manager sync` to update the host menu with container applications.

![Desktop View](/images/incul_manager/synchronize-menu.png){: width="972" height="589" }
_Synchronize menu_

Below we can see the updated menu showing the updated template applications.

![Desktop View](/images/incul_manager/xpra-menu.png){: width="475" height="411" }
_XPRA menu_

From this template we can create a `personal container` 

![Desktop View](/images/incul_manager/create-container.png){: width="972" height="589" }
_Create container_

We can also run `incul-manager list` to show all containers.

![Desktop View](/images/incul_manager/created-containers-list.png){: width="972" height="589" }
_List of created containers_

Below we can see the updated menu showing the updated personal container applications.


![Desktop View](/images/incul_manager/personal_container_menu.png){: width="475" height="411" }
_Personal container applications integrated to menu_

Clicking on an application on the menu for the first time will notify you to confirm the key to connect to the container.

![Desktop View](/images/incul_manager/confirm-ssh-config-for-xpra.png){: width="475" height="411" }
_Confirm key to connect_

We can see below that we can seamlessly run applications , in this image we are running Thunar and XFCE-terminal.

![Desktop View](/images/incul_manager/thunar_terminal_running_in_same_container.png){: width="972" height="589" }
_Thunar and XFCE-terminal running in the same container_

We can also create another container `Work` which will by default be created from the template.

![Desktop View](/images/incul_manager/work_container_menu.png){: width="475" height="411" }
_Work container applications synchronized in menu_

We can also have different color indicators for windows running in different containers.

![Desktop View](/images/incul_manager/thunar_running_in_different_containers.png){: width="972" height="589" }
_Thunar running in different containers_

We can also, using the provided xpra panel menu to configure features such as clipboard sharing.

![Desktop View](/images/incul_manager/clipboard_features_provided_by_xpra.png){: width="475" height="411" }
_Clipboard feature provided by XPRA_

Using the same, we can also upload files to the container.

![Desktop View](/images/incul_manager/xpra_features_upload_file.png){: width="475" height="411" }
_Upload files to container feature provided by XPRA_

Finally we can use the `incul-manager delete` command to delete containers.

![Desktop View](/images/incul_manager/deleting_container.png){: width="972" height="589" }
_Deleting container_


## Conclusion

I had a great time working on this. It did challenge me enough to did deep into Qubes OS docs to solve some problems. 

This tool and similar tools can be fashioned for use by developers to be able to spin up disposable containers quickly for specific projects without taking the hit that virtual machines have on system resources. 

Anyone who seeks security should really check out Qubes OS and enjoy the multiple security benefits it provides.