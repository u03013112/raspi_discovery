方法3：使用树莓派的主机名

树莓派通常具有默认的主机名raspberrypi。在同一局域网内的设备可以通过主机名找到树莓派，而不是IP地址。这是通过Multicast DNS（mDNS）实现的，它是一种在局域网内解析主机名为IP地址的服务。大多数现代网络都支持mDNS，macOS和Linux系统默认支持mDNS，而Windows需要安装Bonjour服务。

以下是如何使用主机名连接到树莓派的步骤：

确保您的树莓派已连接到Wi-Fi网络。

对于Windows用户，请确保已安装Bonjour服务。您可以从Apple官方网站下载并安装Bonjour Print Services for Windows。安装完成后，您的Windows系统将支持mDNS。

打开您的开发机（Windows、macOS或Linux）上的终端或命令提示符。

输入以下命令以尝试通过主机名连接到树莓派：

ssh pi@raspberrypi.local
如果您的网络支持mDNS，这将允许您直接通过主机名连接到树莓派。如果您更改了树莓派的默认主机名，请使用相应的主机名替换raspberrypi.local。

当提示输入密码时，输入树莓派用户pi的密码。默认密码是raspberry。输入密码后，您将连接到树莓派。

注意：出于安全原因，建议您在首次连接后更改树莓派的默认密码。要更改密码，您可以在树莓派上运行passwd命令并按照提示操作。

mDNS（Multicast DNS）需要在局域网内的设备（如树莓派和PC）上都支持。这样，这些设备才能在局域网内通过主机名相互发现和通信。

对于树莓派（基于Linux的系统），默认已支持mDNS。Raspbian操作系统（树莓派官方操作系统）使用avahi-daemon服务实现mDNS。

对于PC：

macOS系统默认支持mDNS，无需额外配置。
Linux系统通常也支持mDNS，可以通过安装avahi-daemon服务来实现（如果尚未安装）。
Windows系统默认不支持mDNS，需要安装Bonjour服务以实现mDNS支持。安装Bonjour后，Windows系统将能够通过mDNS解析局域网内其他设备的主机名。
总之，要实现树莓派和PC之间通过mDNS通信，需要确保两者都支持mDNS。在大多数情况下，树莓派和PC（macOS和Linux）已默认支持mDNS，而Windows需要安装Bonjour服务。

要确认您的树莓派支持并启用了mDNS，您可以检查avahi-daemon服务是否正在运行。avahi-daemon是一个Linux系统上的mDNS实现，通常在树莓派的Raspbian操作系统中默认启用。

请按照以下步骤检查树莓派上的avahi-daemon服务：

通过SSH连接到您的树莓派。

在终端中运行以下命令，检查avahi-daemon服务的状态：

sudo systemctl status avahi-daemon
查看输出结果。如果avahi-daemon服务正在运行，您将看到类似以下的输出：

● avahi-daemon.service - Avahi mDNS/DNS-SD Stack
  Loaded: loaded (/lib/systemd/system/avahi-daemon.service; enabled; vendor preset: enabled)
  Active: active (running) since Mon 2021-12-13 12:34:56 UTC; 1 day 6h ago
Main PID: 327 (avahi-daemon)
   Tasks: 2 (limit: 4915)
  CGroup: /system.slice/avahi-daemon.service
          ├─327 avahi-daemon: running [raspberrypi.local]
          └─328 avahi-daemon: chroot helper
在这个示例中，avahi-daemon服务已启用且正在运行，主机名为raspberrypi.local。

如果avahi-daemon服务未启用或未运行，您可以使用以下命令启动服务并设置为开机自启动：

sudo systemctl start avahi-daemon
sudo systemctl enable avahi-daemon
现在，您的树莓派应该支持并启用了mDNS。您可以在局域网内的其他设备上通过主机名（例如raspberrypi.local）访问树莓派。

我的树莓派修改主机名之后是不是就要用自定义的主机名来mDNS了？

是的，如果您修改了树莓派的主机名，那么您需要使用新的自定义主机名进行mDNS。

例如，如果您将树莓派的主机名从默认的raspberrypi更改为myraspi，那么在局域网内的其他设备上通过mDNS访问树莓派时，您应该使用myraspi.local作为主机名。

要修改树莓派的主机名，请按照以下步骤操作：

通过SSH连接到您的树莓派。

使用文本编辑器打开/etc/hostname文件。例如，使用nano编辑器：

sudo nano /etc/hostname
将文件中的主机名更改为您选择的新主机名（例如myraspi），然后保存并关闭文件。

使用文本编辑器打开/etc/hosts文件：

sudo nano /etc/hosts
在127.0.1.1行中，将原始主机名更改为您选择的新主机名（例如myraspi），然后保存并关闭文件。

重启树莓派以应用更改：

sudo reboot
现在，您的树莓派已使用新的自定义主机名。使用新主机名（例如myraspi.local）进行mDNS访问。