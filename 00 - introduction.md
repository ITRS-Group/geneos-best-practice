# Geneos Best Practices

## Introduction

In these guides we are going to show you how we recommend you install, configure, operate and maintain Geneos. These guides have been written, primarily, by the ITRS Professional Services team who between the members have many decades of experience with Geneos.

While it's not critical that you have any experience with Geneos already, it would be greatly beneficial to your journey if you take some time to familiarise yourself with some of the architectural details of the product. To find out more about Geneos as an enterprise monitoring platform, please take a look at the extensive online documentation, starting with the [Geneos Architecture](https://docs.itrsgroup.com/docs/geneos/6.6.0/getting-started/architecture/index.html) document.

We have also developed tools to make your experience easier, the main one we will be using is a command line program called, fittingly, `geneos` (note how we display is in fixed-width `code-like` text, to distinguish it from the actual Geneos software) which is part of the `cordial` tool-set, published on github. While everything we show you can be done using traditional system commands, using `geneos` should make things much quicker and simpler.

## Prerequisites

### Permissions and Access

We are going to assume that you have the appropriate level of permissions to perform actions, or can call on colleagues in the appropriate teams to assist, including:

* Have command line access to the UNIX/Linux servers where Geneos will be installed
* Have access to any Windows servers that Geneos Netprobes may be required for, including administrative rights to install from MSI packages
* Have a registered ITRS account to access software releases
* Download software either directly to the servers or via an intermediate host and can copy them locally to each server
* Permission to create new directories and files
* Can ensure selected TCP ports are open for access on servers where Geneos services are installed

### Download `geneos`

You should also download the latest release of the `geneos` utility from github - <https://github.com/ITRS-Group/cordial/releases/latest/download/geneos> - and install it in a suitable directory as an executable, like this:

```bash
$ mkdir ${HOME}/bin && cd ${HOME}/bin
$ curl -OL https://github.com/ITRS-Group/cordial/releases/latest/download/geneos
$ chmod +x geneos
```

💡Notes:

* You can instead place the binary in a system directory such as `/usr/local/bin`, if you have administrator privileges.

* You may also need to re-read your shell dot files if the destination directory did not exist when you logged in.

* If you download in a location that already contains a *directory* called `geneos` - such as an existing installation - then the `curl` command will fail.

* On some Linux distributions you may need to replace `curl -OL` with `wget` to do the same thing.

* Remember, this is **NOT** the Geneos product release but a tool developed by ITRS Professional Services to help you manage Geneos.

You can run `geneos` at the command line and you should see the standard help text, like below. If you don't see this then please check the downloaded file, it's permissions and if the directory you downloaded to is in your execution `PATH`.

```bash
$ geneos

  The  geneos  program will help you manage your Geneos environment.                                                          
                                                                                                                              
  The program will help you initialise a new installation, migrate an old  geneos-utils  one, install and update software     
  releases, add and remove instances, control processes and build template based configuration files for SANs and more.

  ...
  
  Use "geneos [command] --help" for more information about a command.
```


