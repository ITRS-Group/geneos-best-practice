# Geneos Best Practices Guidelines - Getting Started

## Permissions and Access

You will need the appropriate level of privileges, or be able call on the help of colleagues in the appropriate teams to assist, including:

* Registered for an ITRS web account to download software
* Internet access to download software either directly to your systems or via an intermediate host, like your desktop, and can copy them to each server
* Command line access to the Linux system where Geneos Gateways will be installed
* Permissions to create new directories and files
* Can ensure selected TCP ports are open for access on servers where Geneos components are installed
* Administrative access to any Windows systems where Netprobes are needed, including rights to install software

The various Geneos components all have their own prerequisites and these are listed in the respective technical reference guides.

## Download `cordial` and the `geneos` utility

### From ITRS Download Site

You can download the complete `cordial` achive from our download site at <https://resources.itrsgroup.com> under the Utilties section. The direct link to the latest release is <https://resources.itrsgroup.com/download/latest/Cordial+-+Geneos+Utilities>.

Once you have downloaded the archive, extract the `geneos` binary and place it in a directory that is in your `PATH` or create a new directory and add it to your `PATH`.

```bash
tar xf cordial-<version>.tar.gz
mkdir -p ${HOME}/bin && cd ${HOME}/bin
cp ../cordial-<version>/bin/geneos .
```

>[!NOTE]
>Because the files in the `tar.gz` archive carry their own poermissions, you do not need to run `chmod +x geneos` after copying it to your `bin` directory.

### From GitHub

If you have direct access to GitHub you can also download the latest release of either the `cordiaal` archive, like above, from <https://github.com/ITRS-Group/cordial/releases/latest> or you can download just the `geneos` binary; the latest standalone Linux binary can always be downloaded from <https://github.com/ITRS-Group/cordial/releases/latest/download/geneos>.

To install it in your user's `bin` directory, do the following:

```bash
mkdir ${HOME}/bin && cd ${HOME}/bin
curl -OL https://github.com/ITRS-Group/cordial/releases/latest/download/geneos
chmod +x geneos
```

<https://resources.itrsgroup.com/download/latest/Cordial+-+Geneos+Utilities>

>[!NOTE]
>
> * You can instead place the binary in a system directory such as `/usr/local/bin`, if you have administrator privileges.
> * You may also need to re-read your shell dot files if the destination directory did not exist when you logged in.
> * If you download in a location that already contains a *directory* called `geneos` - such as an existing installation - then the `curl` command will fail.
> * On some Linux distributions you may need to replace `curl -OL` with `wget` to do the same thing.
> * Remember, this is **NOT** the Geneos product release but a tool developed by ITRS Professional Services to help you manage Geneos.

You can run `geneos` at the command line and you should see the standard help text, like below. If you don't see this then please check the downloaded file, it's permissions and if the directory you downloaded to is in your execution `PATH`.

```bash
$ geneos

  The  geneos  program will help you manage your Geneos environment.

  ...

  Use "geneos [command] --help" for more information about a command.
```

