title: Android Debug Bridge
id: 1269
categories:
  - Android
date: 2014-03-11 22:51:41
tags:
---

源自:[http://developer.android.com/tools/help/adb.html](http://developer.android.com/tools/help/adb.html "http://developer.android.com/tools/help/adb.html")

安卓调试桥,是一个CS程序,包括:
A client, which runs on your development machine. You can invoke a client from a shell by issuing an adb command. Other Android tools such as the ADT plugin and DDMS also create adb clients.
A server, which runs as a background process on your development machine. The server manages communication between the client and the adb daemon running on an emulator or device.
A daemon, which runs as a background process on each emulator or device instance.

adb位置:/platform-tools/
当启动adb客户端的时候,客户端首先检查adb服务进程是否存在,如果不存在,则启动adb服务进程.
当adb服务启动的时候,会绑定本地TCP端口5037来监听来自adb客户端的命令,所有的adb客户端都通过5037端口与服务器通信.<!--more-->
然后服务器与所有运行中的模拟器实例/设备实例进行连接.它通过扫描模拟器实例/设备实例使用的5555到5585之间的奇数端口来定位模拟器/设备实例.
当服务器在端口中发现了adb守护进程时,它会连接到那个端口.
注意每个模拟器实例/设备实例将获得一对连续的端口,一个偶数端口用于控制台连接和一个奇数端口用于adb连接.

Emulator 1, console: 5554
Emulator 1, adb: 5555
Emulator 2, console: 5556
Emulator 2, adb: 5557
and so on...

用法:

```shell
adb [-d|-e|-s <serialNumber>] <command>
```

If there's only one emulator running or only one device connected, the adb command is sent to that device by default. If multiple emulators are running and/or multiple devices are attached, you need to use the -d, -e, or -s option to specify the target device to which the command should be directed.

Available adb commands
<table>
<tbody>
<tr>
<th>Category</th>
<th>Command</th>
<th>Description</th>
<th>Comments</th>
</tr>
<tr>
<td rowspan="3">Target Device</td>
<td>`-d`</td>
<td>Direct an adb command to the only attached USB device.</td>
<td>Returns an error if more than one USB device is attached.</td>
</tr>
<tr>
<td>`-e`</td>
<td>Direct an adb command to the only running emulator instance.</td>
<td>Returns an error if more than one emulator instance is running.</td>
</tr>
<tr>
<td>`-s <serialNumber>`</td>
<td>Direct an adb command a specific emulator/device instance, referred to by its adb-assigned serial number (such as "emulator-5556").</td>
<td>See [Directing Commands to a Specific Emulator/Device Instance](http://developer.android.com/tools/help/adb.html#directingcommands).</td>
</tr>
<tr>
<td rowspan="3">General</td>
<td>`devices`</td>
<td>Prints a list of all attached emulator/device instances.</td>
<td>See [Querying for Emulator/Device Instances](http://developer.android.com/tools/help/adb.html#devicestatus) for more information.</td>
</tr>
<tr>
<td>`help`</td>
<td>Prints a list of supported adb commands.</td>
<td></td>
</tr>
<tr>
<td>`version`</td>
<td>Prints the adb version number.</td>
<td></td>
</tr>
<tr>
<td rowspan="3">Debug</td>
<td>`logcat [option] [filter-specs]`</td>
<td>Prints log data to the screen.</td>
<td></td>
</tr>
<tr>
<td>`bugreport`</td>
<td>Prints `dumpsys`, `dumpstate`, and `logcat` data to the screen, for the purposes of bug reporting.</td>
<td></td>
</tr>
<tr>
<td>`jdwp`</td>
<td>Prints a list of available JDWP processes on a given device.</td>
<td>You can use the `forward jdwp:<pid>` port-forwarding specification to connect to a specific JDWP process. For example:
`adb forward tcp:8000 jdwp:472`
`jdb -attach localhost:8000`</td>
</tr>
<tr>
<td rowspan="3"">Data</td>
<td>`install <path-to-apk>`</td>
<td>Pushes an Android application (specified as a full path to an .apk file) to an emulator/device.</td>
<td></td>
</tr>
<tr>
<td>`pull <remote> <local>`</td>
<td>Copies a specified file from an emulator/device instance to your development computer.</td>
<td></td>
</tr>
<tr>
<td>`push <local> <remote>`</td>
<td>Copies a specified file from your development computer to an emulator/device instance.</td>
<td></td>
</tr>
<tr>
<td rowspan="2">Ports and Networking</td>
<td>`forward <local> <remote>`</td>
<td>Forwards socket connections from a specified local port to a specified remote port on the emulator/device instance.</td>
<td>Port specifications can use these schemes:

*   `tcp:<portnum>`
*   `local:<UNIX domain socket name>`
*   `dev:<character device name>`
*   `jdwp:<pid>`
</td>
</tr>
<tr>
<td>`ppp <tty> [parm]...`</td>
<td>Run PPP over USB.

*   `<tty>` — the tty for PPP stream. For example `dev:/dev/omap_csmi_ttyl`.
*   `[parm]... ` — zero or more PPP/PPPD options, such as `defaultroute`, `local`, `notty`, etc.
Note that you should not automatically start a PPP connection.</td>
<td></td>
</tr>
<tr>
<td rowspan="3">Scripting</td>
<td>`get-serialno`</td>
<td>Prints the adb instance serial number string.</td>
<td rowspan="2">See [Querying for Emulator/Device Instances](http://developer.android.com/tools/help/adb.html#devicestatus) for more information.</td>
</tr>
<tr>
<td>`get-state`</td>
<td>Prints the adb state of an emulator/device instance.</td>
</tr>
<tr>
<td>`wait-for-device`</td>
<td>Blocks execution until the device is online — that is, until the instance state is `device`.</td>
<td>You can prepend this command to other adb commands, in which case adb will wait until the emulator/device instance is connected before issuing the other commands. Here's an example:
<pre>adb wait-for-device shell getprop</pre>
Note that this command does _not_ cause adb to wait until the entire system is fully booted. For that reason, you should not prepend it to other commands that require a fully booted system. As an example, the `install` requires the Android package manager, which is available only after the system is fully booted. A command such as
<pre>adb wait-for-device install <app>.apk</pre>
would issue the `install` command as soon as the emulator or device instance connected to the adb server, but before the Android system was fully booted, so it would result in an error.</td>
</tr>
<tr>
<td rowspan="2">Server</td>
<td>`start-server`</td>
<td>Checks whether the adb server process is running and starts it, if not.</td>
<td></td>
</tr>
<tr>
<td>`kill-server`</td>
<td>Terminates the adb server process.</td>
<td></td>
</tr>
<tr>
<td rowspan="2">Shell</td>
<td>`shell`</td>
<td>Starts a remote shell in the target emulator/device instance.</td>
<td rowspan="2">See [Issuing Shell Commands](http://developer.android.com/tools/help/adb.html#shellcommands) for more information.</td>
</tr>
<tr>
<td>`shell [shellCommand]`</td>
<td>Issues a shell command in the target emulator/device instance and then exits the remote shell.</td>
</tr>
</tbody>
</table>

## 
