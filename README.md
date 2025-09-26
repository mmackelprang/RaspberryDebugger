# Raspberry Debugger forked from nforgeio/RaspberryDebugger
_A Visual Studio Extension for debugging .NET Core applications remotely on a Raspberry Pi_

---
**IMPORTANT:** Can be ONLY used in Visual Studio 2022 and Visual Studio 2026! Currently supports .NET 3.1, .NET 5, .NET 6, .NET 7, and .NET 8.

For debugging set the runtime identifier project property to:

```
<PropertyGroup>
   ...
   <PlatformTarget>AnyCPU</PlatformTarget>
   <RuntimeIdentifier>linux-arm</RuntimeIdentifier>
</PropertyGroup>
```
https://docs.microsoft.com/de-de/dotnet/core/rid-catalog
---

You can use Visual Studio Code to develop and debug .NET Core applications either directly on your Raspberry or remotely from another computer but until today, there's been no easy way to use regular Visual Studio to develop and debug applications for Raspberry.

The new **Raspberry Debugger** Visual Studio extension allows you to code your application on a Windows workstation and then build and debug it on a Raspberry by just pressing **F5 - Start Debugging**.

### Requirements

* Windows 10/11
* Visual Studio 2022 Community Edition (or better)
* Raspberry Pi running 32-bit or 64-bit Raspberry Pi OS - grab this source and feel free to test, please
* Raspberry user allowed to `sudo`
* [Windows Open SSH Client](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse) (installed automatically when required)

### Features

* Supports Raspberry Pi 3+ devices and Zero 2 W 
* Take a Pi 4 (4/8GB) for development work with large-scale solutions
* Raspberry Pi OS 32/64-bit (raspbian)
* Console and ASPNET .NET applications supported
* Configures SSH 2048-bit RSA key pair automatically
* **F5/Run** debugging
   * .NET SDK and VSDBG installations handled automatically
   * Program files transferred to the Raspberry
* Raspberry debugging can be enabled/disabled for specific projects
* Project specific Raspberry devices
* C# and Visual Basic (other .NET languages should work too)
* Debug activity logs

### Installation

You can install the Raspberry Debugger from within Visual Studio:

1. Start Visual Studio
2. Select the **Extensions/Manage Extensions** menu
3. Search for **Raspberry Debugger**
4. Double-click it to install

You can install also this from our GitHub releases page:

1. Go to [GitHub releases page](https://github.com/nforgeio/RaspberryDebugger/releases)
2. Click the most recent release
3. Scroll down to the **Assets** second and expand the list
4. Click **RaspberryDebugger.vsix** to download it
5. You should be prompted to open it with the **VisualStudio.Launcher.vsix**.  Click **OK** to install it
6. Otherwise, save the file to your desktop and double-click to install it

### Configure your Raspberry

After setting up your Raspberry based on the instructions you received with it, you'll need to perform a couple additional steps to make it ready for remote debugging:

1. Enable SSH so the Raspberry Debugger will be able to connect to your Raspberry over the network: Start the **Terminal** on your Raspberry and enter these commands:
   ```
   sudo systemctl enable ssh
   sudo systemctl start ssh
   ```
   You'll probably want to change the **pi** user password from the default **raspberry** if you haven't already done so.

1. Ensure that your Raspberry is connected to the network via WiFi or a wired ethernet.

1. You'll need to need to know the IP address for your Raspberry.  Go back to the **Terminal** and enter this command:
   ```
   ip -h address
   ```
   You'll see something like this:
   <br/>![Screenshot](/Doc/Images/GettingStarted/ip-address.png?raw=true)<br/>
   You're looking for an **inet** address.  In this example, my Raspberry is connected to WiFi and so the connection information will be located under the **wlan0** network interface.  I've highlighted the interface and the internet address here.

   When your Raspberry is connected to a wired network, you'll see the IP address beneath the **eth0** network interface which I've also highlighted but there is no IP address listed because my Raspberry is not connected to a wired network.

   Make a note of your Raspberry's IP address; you'll need it to configure a connection in Visual Studio.

1. **Advanced:** Your Raspeberry's IP address may change from time-to-time, depending on how your network is configured.  I've configured my home router to reserve an IP address for my Raspberry so it won't change.  You can do this too if you control your network or you may need to [configure a static IP address](https://www.raspberrypi.org/documentation/configuration/tcpip/) on the Raspberry itself.

Your Raspberry is now ready!

### Configure Visual Studio

On your Windows workstation:

1. Download the **RaspberryDebugger.vsix** file from the latest release on this GitHub repository and double-click it to install it on Visual Studio.  Alternatively, you can install this from the **Visual Studio Marketplace** from within Visual Studio via **Extensions/Manage Extensions** (search for _Raspberry Debugger_).

1. Start or restart Visual Studio.

1. Create a connection for your Raspberry:
   Choose the **Tools/Options...** menu and select the **Raspberry Debugger/Connections** panel.
   <br/>![Screenshot](/Doc/Images/GettingStarted/ToolsOptions1.png?raw=true)<br/>
   Click **Add** to create a connection.  The connection dialog will be prepopulated with the default **pi** username and the default **raspberry** password.  You'll need to update these as required and also enter your Raspberry's IP address (or hostname).
   <br/>![Screenshot](/Doc/Images/GettingStarted/ToolsOptions2.png?raw=true)<br/>
   When you click **OK**, we'll connect to the Raspberry to validate your credentials and if that's successful, we'll configure the Raspberry by installing any required packages and also create and configure the SSH key pair that will be used for subsequent connections.
   <br/>![Screenshot](/Doc/Images/GettingStarted/ToolsOptions3.png?raw=true)<br/>
   Your new connection will look something like this:
   <br/>![Screenshot](/Doc/Images/GettingStarted/ToolsOptions4.png?raw=true)<br/>

1. Configure your .NET Core project for debugging.  Raspberry Debugger supports Console and ASPNET applications targeting .NET Core 3.1 and .NET 5
   Open one of your project source files and choose the new **Project/Raspberry Debug** menu:
   <br/>![Screenshot](/Doc/Images/GettingStarted/RaspberryDebugMenu.png?raw=true)<br/>
   The project Raspberry settings dialog will look like this:
   <br/>![Screenshot](/Doc/Images/GettingStarted/RaspberryProjectSettings.png?raw=true)<br/>
   Click the **Target Raspberry** combo box and choose the Raspberry connection you created earlier or **[DEFAULT]** to select the connection with its **Default** box checked and click **OK** to close the dialog.  You can use this dialog to assign specific Raspberry devices to your projects.

That's all there is to it: just **press F5 to build and debug** your program remotely on the Raspberry.  Add a `Debugger.Break()` call in your program to verify that it actually works:

![Screenshot](/Doc/Images/GettingStarted/DebuggerBreak.png?raw=true)

**NOTE:** The Raspberry Debugger relies on the [Windows Open SSH Client](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse) which is installed by default for recent Windows 10 updates.  If this is not installed for some reason, you'll see the following dialog when you start debugging.  This requires a reboot.  Click **Yes** to install (this takes a few minutes so be patient) and then save any work and restart your workstation.

![Screenshot](/Doc/Images/GettingStarted/WindowsOpenSSH.png?raw=true)

**NOTE:** The first time you debug a program on your Raspberry it will take a minute or two to start because we'll be installing the .NET SDK and debugger.  Debugging will start much faster the second time.

### Console Project Debug Properties

When debugging .NET Core Console projects, you can pass command line arguments and environment variables to the remote program.  We don't currently honor any other properties on this page.

![Screenshot](/Doc/Images/GettingStarted/ConsoleProperties.png?raw=true)

## ASPNET Project Debug Properties

When debugging ASPNET projects, you can pass command line arguments and environment variables to the remote program and you also have the option to start a browser on your workstation and have it display a page from your application.

For Asp.NET debugging set the runtime identifier project property to:

```
<PropertyGroup>
   ...
   <RuntimeIdentifier>linux-arm</RuntimeIdentifier>
</PropertyGroup>
```

Your web application will be deployed on your Raspberry on all network interfaces, using the port specified by the **App URL**.

![Screenshot](/Doc/Images/GettingStarted/AspNetProperties.png?raw=true)

## Debug Logs

The Raspberry Debugger writes detailed log information to the Visual Studio Debug Output Window.  This is designed to give you some idea of what happened when something goes wrong.

![Screenshot](/Doc/Images/GettingStarted/Logging.png?raw=true)

### Under the Covers

Here are some brief comments describing how the Raspberry Debugger works.

#### SSH Keys

The Raspberry Debugger automatically generates and configures a 2048-bit RSA key pair for SSH autentication.  This is actually generated on the Raspberry and the public key is added to the connection user's `~/.ssh/authorized_key` file on the Raspberry.

The public and private keys are also persisted to on your development workstation in the `%USERPROFILE%\.raspbery` folder.  You'll find a `connections.json` file there with the connection information and the key file will be persisted to `%USERPROFILE%\.raspbery\keys`.

You'll need to create connections with a username and password so we'll be able to create and configure the SSH keys and we'll use key authentication thereafter.  You may remove the connection password after creating a connection if you wish, but for most situations we recommend that you leave the password in place.

If you reimage your Raspberry flash drive and configure the same Raspberry password, the Raspberry Debugger will be smart enough to realize the SSH key authentication failed and it will retry connecting with the password and reauthorizing the existing public key.  This makes reimaging a Raspbery nearly seamless.

#### Raspbery Directories

The Raspberry Debugger installs the .NET SDKs to `/lib/dotnet` and the **vsdbg** debugger to `/lib/dotnet/vsdbg`and `/etc/.profile` is updated to set `DOTNET_ROOT=/lib/dotnet` and `DOTNET_ROOT` is also added to the `PATH`.

The programs being debugged will uploaded to `~/vsdbg/NAME` where `NAME` is the name of the program output assembly file.

#### Project Raspberry Debug Settings

The **Project/Raspberry Debug Settings** menu persists the settings to the new `$(SolutionDir)\.vs\raspberry-project.json` file.  This keeps track of which Raspberry connection your projects will use or whether Raspberry debugging is disabled for projects.  We put the file here because these are really developer specific settings and source control solutions are typically be configured to not track files in this folder.

### Limitations

* .NET Core is not supported on Raspberry 1, 2, or Zero cards
* 64-bit Raspberry Pi OS (Raspbian) is supported now
* Only LTS .NET Core SDKs supported: LTS .NET versions (3.1 + 6.0) and latest STS 7.0 (debugger version 3.3)
* **Start Without Debugging** or **Attach to Process...** are not supported (yet)
* Raspberry debugging uses the default project debugging profile
* HTTPS is currently supported for ASPNET debugging (for reverse proxy and for Kestrel)
* Program assembly names can't include spaces

# Maintainer Notes

We used the [Command Exporer](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.CommandExplorer) (by Mads Kristenson) VS extension to discover the IDs for existing VS commands so we can insert our commands.

### Disclosures

* _The Raspberry Debugger extension is compatible with Raspberry Pi_
* _Raspberry Pi is a trademark of the Raspberry Pi Foundation_
