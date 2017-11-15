# [Docker For Windows](https://docs.docker.com/docker-for-windows/)
***
<2017.1114> by Colin Wu colinabc@qq.com
current version: v17.09

## Installation
1. [Docker Community Ediction (CE)](https://www.docker.com/community-edition), ([Stable](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe) | [Edge](https://download.docker.com/win/edge/Docker%20for%20Windows%20Installer.exe))
2. Double-click **Docker for Windows Installer.exe** to run the installer.
3. Follow the install wizard to accept the license, authorize the installer, and proceed with the install.
4. Click Finish on the setup complete dialog to launch Docker.

## Check version of Docker Engine, Compose, and Machine
1. Start your favorite shell(cmd.exe, PowerShell, or other)
2. type the following commands:
```
docker -v[,--version]
docker-compose -v[,--version]
docker-machine -v[,--version]
```

## Explore the application and run examples
1. Open a shell(cmd.exe, PowerShell, or other).
2. Run some Docker commands, such as `docker ps`, `docker version`, and `docker info`
3. Run `docker run hello-world` to test pulling an image from Docker Hub and starting a container.
4. Try something more ambitious, and run an Ubutu container with this command:
```
docker run -it ubuntu bash
```
This will download the `ubuntu` container image and share it.
Type `exit` to stop the container and close the powershell.
5. Start a Dockerized webserver with this command:
```
docker run -d -p 80:80 --name websever nginx
```
This will download the nginx container image and start it. 
6. Point your web browser at `http://localhost` to display the start page. (Since you specified the default HTTP port, it isn't necessary to append `:80` at the end of the URL)
7. Run `docker ps` while your webserver is running to see details on the container.
8. Stop or remove containers and images.

	The `nginx` webserver will continue to run in the container on that port until you stop and/or rmove the container. If you want to stop the webserver, type: `docker stop webserver` and start it again with `docker start webserver`.

	To stop and remove the running container with a single command, type: `docker rm -f webserver`. This will remove the container, but not the `nginx` image. You can list local images with `docker images`. You  might want to keep some images around so that you don't have to pull them again from Docker Hub. To remove an image you no longer need, use `docker rmi` followed by an image ID or image name. For example, `docker rmi nginx`

Want more example applications? [Get Started](https://docs.docker.com/get-started/) and [Samples](https://docs.docker.com/samples) are great places to start.

## Set up tab completion in PowerShell
If you would like to have handy tab completion for Docker commands, you can install the posh-docker PowerShell Module as follows.
1. Start an "elevated" PowerShell(i.e., run it as administrator)
2. Set the [script execution policy](https://msdn.microsoft.com/en-us/powershell/reference/5.1/microsoft.powershell.security/set-executionpolicy) to allow downloaded scripts signed by trusted publisher to run on your computer. To do so, type this at the PowerShell prompt.
```
Set-ExecutionPolicy RemoteSigned
```
To check that the policy is set properly, run `get-executionpolicy`, which should return `RemoteSigned`.
3. To install the `posh-docker` PowerShell module for auto-completion of Docker commands, type:
```
Install-Module posh-docker
```
Or, to install the module for the current user only, type:
```
Install-Module -Scope CurrentUser posh-docker
```
4. After installation to enable autocompletion for the current PowerShell only, type:
```
Import-Module posh-docker
```
5. To make tab completion persistent across all PowerShell sesssions, add the command to a `$PROFILE` by typing these commands at the PowerShell prompt.
```
if (-Not (Test-Path $PROFILE)) {
    New-Item $PROFILE –Type File –Force
}

Add-Content $PROFILE "`nImport-Module posh-docker"
```
This create a $PROFILE if one does not already exist, and adds this line into the file:
```
Import-Module posh-docker
```
To check that the file was properly created, or simply edit it manually, type this in PowerShell:
```
Notepad $PROFILE
```
Open a new PowerShell session. Now, when you press tab after typing the first few letters, Docker commands such as `start`, `stop`, `run`, and their options, along with container and image names should now auto-complete.

## Docker Settings
To sign in for integrated Docker Cloud access, see [Docker Cloud](https://docs.docker.com/docker-for-windows/#docker-cloud).
The **Settings** dialogs provide options to allow Docker auto-start, automatically check for updates, share local drives with Docker containers, enable VPN compatibility, manage CPUs and memory Docker uses, restart Docker, or perform a factory reset.

1. General
	- Start Docker when you log in
	- Check for updates when the application starts
	- Send usage statistics

2. Shared Drives
	Share your local drives (volumes) with Docker for windows, so that they are available to your containers.
2.1 Tips

	- Shared drives are only required for volume mounting Linux containers, not for Windows containers. For [Linux containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers), you need to share the drive where your project is located (i.e., where the Dockerfile and volume are located). Runtime errors such as file not found or cannot start service may indicate shared drives are needed. (See also [Volume mounting requires shared drives for Linux containers](https://docs.docker.com/docker-for-windows/troubleshoot/#volume-mounting-requires-shared-drives-for-linux-containers).)
	- If possible, avoid volume mounts from the Windows host, and instead mount on the MobyVM, or use a [data volume](https://docs.docker.com/engine/tutorials/dockervolumes/#data-volumes) (named volume) or [data container](https://docs.docker.com/engine/tutorials/dockervolumes/#creating-and-mounting-a-data-volume-container). There are a number of issues with using host-mounted volumes and network paths for database files. Please see the troubleshooting topic on [Volume mounts from host paths use a nobrl option to override database locking](https://docs.docker.com/docker-for-windows/troubleshoot/#volume-mounts-from-host-paths-use-a-nobrl-option-to-override-database-locking).
	- You cannot control (`chmod`) permissions on shared volumes for deployed containers. Docker for Windows sets permissions to a default value of [0755](http://permissions-calculator.org/decode/0755/) (`read`, `write`, `execute` permissions for `user`, `read` and `execute` for group). This is not configurable. See the troubleshooting topic [Permissions errors on data directories for shared volumes](https://docs.docker.com/docker-for-windows/troubleshoot/#permissions-errors-on-data-directories-for-shared-volumes) for workarounds and more detail.
	- Make sure that the domain user has permissions to shared drives, as described in the troubleshooting topic ([Verify domain user has permissions for shared drives](https://docs.docker.com/docker-for-windows/troubleshoot/#verify-domain-user-has-permissions-for-shared-drives-volumes)).
	- You can share local drives with your containers but not with Docker Machine nodes. See [Can I share local drives and filesystem with my Docker Machine VMs?](https://docs.docker.com/docker-for-windows/faqs/#can-i-share-local-drives-and-filesystem-with-my-docker-machine-vms) in the FAQs.

  2.2 FIREWALL RULES FOR SHARED DRIVES
    Shared drives require port 445 to be open between the host machine and the virtual machine that runs Linux containers.
	To share the drive, allow connections between the Windows host machine and the virtual machine in Windows Firewall or your third party firewall software. You do not need to open port 445 on any other network. By default, allow connections to 10.0.75.1 port 445 (the Windows host) from 10.0.75.2 (the virtual machine). If the firewall rules appear to be open, consider [reinstalling the File and Print Sharing service on the virtual network adapter](http://stackoverflow.com/questions/42203488/settings-to-windows-firewall-to-allow-docker-for-windows-to-share-drive/43904051#43904051).
  2.3 SHARED DRIVES ON DEMAND
	You can share a drive “on demand” the first time a particular mount is requested.

	If you run a Docker command from a shell with a volume mount (as shown in the example below) or kick off a Compose file that includes volume mounts, you get a popup asking if you want to share the specified drive.

	You can select to **Share it**, in which case it is added your Docker for Windows [Shared Drives list](https://docs.docker.com/docker-for-windows/#shared-drives) and available to containers. Alternatively, you can opt not to share it by hitting Cancel.
	
	`docker run --rm -v c:/Users:/data alpine ls /data`

3. Advanced 
	- CPUs - Change the number of processors assigned to the Linux VM.
	- Memory - Change the amount of memory the Docker for Windows Linux VM uses.

  Please note, updating these settings requires a reconfiguration and reboot of the Linux VM. This will take a few seconds.

4. Network 
You can configure Docker for Windows networking to work on a virtual private network (VPN).

	- **Internal Virtual Switch** - You can specify a network address translation (NAT) prefix and subnet mask to enable internet connectivity.

	- **DNS Server** - You can configure the DNS server to use dynamic or static IP addressing.
	> **Note**: Some users reported problems connecting to Docker Hub on Docker for Windows stable version. This would manifest as an error when trying to run `docker` commands that pull images from Docker Hub that are not already downloaded, such as a first time run of `docker run hello-world`. If you encounter this, reset the DNS server to use the Google DNS fixed address: `8.8.8.8`. For more information, see [Networking issues](https://docs.docker.com/docker-for-windows/troubleshoot/#networking-issues) in Troubleshooting.
	
  Note that updating these settings requires a reconfiguration and reboot of the Linux VM.

5. Proxies
Docker for Windows lets you configure HTTP/HTTPS Proxy Settings and automatically propagate these to Docker and to your containers. For example, if you set your proxy settings to `http://proxy.example.com`, Docker will use this proxy when pulling containers.

6. Docker daemon
You can configure options on the Docker daemon that determine how your containers will run. You can configure some **Basic** options on the daemon with interactive settings, or switch to **Advanced** to edit the JSON directly.

 The settings offered on **Basic** dialog can be configured directly in the JSON as well. This version just surfaces some of the common settings to make it easier to configure them.
	- [Experimental mode](https://docs.docker.com/docker-for-windows/#experimental-mode)
	- [Custom registries](https://docs.docker.com/docker-for-windows/#custom-registries)
	- [Edit the daemon configuration file](https://docs.docker.com/docker-for-windows/#edit-the-daemon-configuration-file)

 For a full list of options on the Docker [daemon](https://docs.docker.com/engine/reference/commandline/dockerd/), see daemon in the Docker Engine command line reference.
 In that topic, see also:
	- [Daemon configuration file](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-configuration-file)
	- [Linux configuration file](https://docs.docker.com/engine/reference/commandline/dockerd/#linux-configuration-file)
	- [Windows configuration file](https://docs.docker.com/engine/reference/commandline/dockerd/#windows-configuration-file)

 Note that updating these settings requires a reconfiguration and reboot of the Linux VM.

## Swith between Windows and Linux containers
You can select which daemon (Linux or Windows) the Docker CLI talks to. Select **Switch to Windows containers** to toggle to Windows containers. Select **Switch to Linux containers** to toggle back to the default, Linux containers.
1. GETTING STARTED WITH WINDOWS CONTAINERS
If you are interested in working with Windows containers, here are some guides to help you get started.

	- [Build and Run Your First Windows Server Container (Blog Post)](https://blog.docker.com/2016/09/build-your-first-docker-windows-server-container/) gives a quick tour of how to build and run native Docker Windows containers on Windows 10 and Windows Server 2016 evaluation releases.

	- [Getting Started with Windows Containers (Lab)](https://github.com/docker/labs/blob/master/windows/windows-containers/README.md) shows you how to use the [MusicStore](https://github.com/aspnet/MusicStore/blob/dev/README.md) application with Windows containers. The MusicStore is a standard .NET application and, [forked here to use containers](https://github.com/friism/MusicStore), is a good example of a multi-container application.

	> Disclaimer: This lab is still in work, and is based off of the blog, but you can test and leverage the example walkthroughs now, if you want to start experimenting. Please check back as the lab evolves.

	- This troubleshooting issue is useful for understanding how to connect to Windows containers from the local host: [Limitations of Windows containers for `localhost` and published ports](https://docs.docker.com/docker-for-windows/troubleshoot/#limitations-of-windows-containers-for-localhost-and-published-ports)

2. ABOUT THE DOCKER WINDOWS CONTAINERS SPECIFIC DIALOGS
When you switch to Windows containers, the Settings panel updates to show only those [dialogs](https://docs.docker.com/docker-for-windows/#docker-settings) that are active and apply to your Windows containers:

	- General
	- Proxies
	- Docker daemon
	- Diagnose and Feedback
	- Reset

 Keep in mind that if you set proxies or daemon configuration in Windows containers mode, these apply only on Windows containers. If you switch back to Linux containers, proxies and daemon configurations return to what you had set for Linux containers. Your Windows container settings are retained and become available again when you switch back.

 The following settings are **not available in Windows containers mode**, because they do not apply to Windows containers:

	- Shared Drives
	- Network
	- Advanced (CPU and Memory configuration)

## Docker Store
Choose **Docker Store** from the Docker for Windows menu to get to the Docker app downloads site. [Docker store](https://store.docker.com/) is a component of the next-generation Docker Hub, and the best place to find compliant, trusted commercial and free software distributed as Docker Images.

## Docker Cloud
You can access your [Docker Cloud](https://docs.docker.com/docker-cloud/) account from within Docker for Windows.