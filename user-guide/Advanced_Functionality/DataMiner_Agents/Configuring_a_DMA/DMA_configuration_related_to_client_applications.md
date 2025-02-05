---
uid: DMA_configuration_related_to_client_applications
---

# DMA configuration related to client applications

This section consists of the following topics:

- [Configuring the default landing page](#configuring-the-default-landing-page)

- [Configuring client communication settings](#configuring-client-communication-settings)

## Configuring the default landing page

In the file *Config.manual.asp*, located in the folder *C:\\Skyline DataMiner\\WebPages\\*, you can specify what happens when a user browses directly to the IP of the DMA. In some cases, a customizable landing page can be displayed instead of a client application.

In this section:

- [Setting the default client application](#setting-the-default-client-application)

- [Redirection overview](#redirection-overview)

- [Showing the company logo on the landing page](#showing-the-company-logo-on-the-landing-page)

- [Removing applications from the default landing page](#removing-applications-from-the-default-landing-page)

- [Making users download the Microsoft .NET Framework from an internal server](#making-users-download-the-microsoft-net-framework-from-an-internal-server)

- [Example of a config.manual.asp file](#example-of-a-configmanualasp-file)

> [!NOTE]
> - If the *Config.manual.asp* file does not exist on a DMA, it is automatically created during DataMiner startup.
> - Some of the settings in the *Config.manual.asp* file are only available from DataMiner 9.0 onwards. If you upgrade to 9.0 from an older version of DataMiner, the file will not be updated automatically, so some settings may need to be added manually.
> - The file is NOT automatically synchronized on other DMAs in the DMS.

### Setting the default client application

1. Open *C:\\Skyline DataMiner\\WebPages\\config.manual.asp* in a text editor.

2. To configure the default setting when Internet Explorer is used (or Edge in IE compatibility mode), set the variable *defaultApp* to one of the following values:

    | Value | Default client application/landing page |
    |---------|-----------------------------------------|
    | Cube    | DataMiner Cube.                         |
    | HTML5   | DataMiner Monitoring & Control app.     |
    | (empty) | DataMiner Cube.                         |

    For example, in the following configuration, DataMiner Cube is set as the default client user interface:

    ```txt
    var defaultApp = "Cube";
    ```

3. To configure the default setting when a different browser than Internet Explorer is used, set the variable *defaultHTMLApp* to one of the following values:

    | Value | Default client application/landing page                                                                             |
    |---------|---------------------------------------------------------------------------------------------------------------------|
    | HTML5   | DataMiner Monitoring & Control app.                                                                                 |
    | (empty) | If no value is specified, the landing page is displayed, where the user can choose which client application to use. |

    For example, if the following configuration is applied, the landing page will be displayed:

    ```txt
    var defaultHTMLApp = "";
    ```

### Redirection overview

The behavior of the specified setting depends on:

- The operating system.

- The internet browser used.

| Operating system  | Browser           | Redirect to ...                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft Windows | Internet Explorer | The default application or the landing page, as specified in *Config.manual.asp*.<br> If no default application has been specified: DataMiner Cube.                                                                                                                                                                                                                                                                  |
| Microsoft Windows | Other             | A landing page with two options:<br> -  Install the DataMiner Cube desktop app<br> -  Open the Monitoring & Control app<br> If the default application for other browsers specified in *Config.manual.asp* is “HTML5”, you will be redirected to the Monitoring & Control app instead. |
| Other             | Other             | Monitoring & Control app                                                                                                                                                                                                                                                                                                                                                                                                                        |

### Showing the company logo on the landing page

It is possible to customize the landing page so that the company logo is displayed.

1. Open *C:\\Skyline DataMiner\\WebPages\\config.manual.asp* in a text editor.

2. Set the setting *showCustomerLogo* to *true*. This setting is set to false by default.

    ```txt
    var showCustomerLogo = true;
    ```

> [!NOTE]
> To set your company logo in DataMiner Cube, go to *System Center > Agents > System > System Logo*.

### Removing applications from the default landing page

It is possible to customize the landing page so that certain applications are not listed on it.

1. Open *C:\\Skyline DataMiner\\WebPages\\config.manual.asp* in a text editor.

2. Specify the applications that should not be displayed in the *disallowed* setting.

    For example, in the following configuration, the landing page will not show the possibility to install the DataMiner Cube desktop app or to open the Monitoring & Control app:

    ```txt
    var disallowed = ["InstallCube","HTML5"];
    ```

### Making users download the Microsoft .NET Framework from an internal server

When a user selects the *Install DataMiner Cube* option from the landing page, a setup program will first check whether Microsoft .NET Framework 4 is installed. If this is not the case, the user will be redirected to *http://dmaip/Tools/InstallNet4.asp*, where the Microsoft .NET Framework 4.6 installer can be downloaded.

By default, the online installer will be downloaded from the Microsoft website. If, however, users do not have public internet access, then you can allow them to download the offline installer from an internal server (e.g. the DataMiner Agent) instead.

To make users download the installer from an internal server:

1. Download the offline .Net Framework 4.6 installer:

    - http://www.microsoft.com/en-US/download/details.aspx?id=48137

2. Place the offline installer in a server folder, and make sure your users are allowed access to it.

3. Specify the internal download URL in the *urlOfflineNetFrameworkInstaller* setting of the *config.manual.asp* file. Make sure to use a “http://” or “https://” address.

    ```txt
    var urlOfflineNetFrameworkInstaller = "http://myServer/myFolder/NDP46-KB3045557-x86-x64-AllOS-ENU.exe";
    ```

4. Make sure that, on all necessary computers, the browser security settings allow users to download the file you specified in step 3.

### Example of a config.manual.asp file

```xml
<%
/*
    * Default application to launch
    * Values: "Cube", "SystemDisplay", "SystemDisplayNU", "SystemDisplayNSU", "HTML5"  or empty to show the landing page.
*/
var defaultApp = "Cube";
/*
    * If operators not have access to a public Internet connection, then specify the internal url to download the offline Microsoft .NET Framework installer
    * Leave empty to download the online installer from the Microsoft website.
*/
var urlOfflineNetFrameworkInstaller = "";
/*
    * Which applications should not be shown on the landing page
    * Array containing values: "Cube", "InstallCube", "HTML5" (multiple values can be filled in separated by a comma)
*/
var disallowed = [];
/*
    * Show the customer logo on the DataMiner landing page.
    * Default: false
*/
var showCustomerLogo = false;
/*
 * Default application to launch when not using Internet Explorer
 * Values: "HTML5" or empty to show the landing page
*/
var defaultHTMLApp = "";
%>
```

## Configuring client communication settings

Users can define whether DataMiner Cube has to automatically detect the settings to be used when establishing a connection toward a certain DataMiner Agent.

If it is set to automatically detect the connection settings, Cube will connect to a DataMiner Agent according to the communication settings defined on the DataMiner Agent to which it is connecting. As a consequence, no settings will have to be changed when a user opens multiple instances of DataMiner Cube, all pointing to different DataMiner Agents.

> [!TIP]
> See also:
> - [Configuring the IP network ports](xref:General_DMA_configuration#configuring-the-ip-network-ports)
> - [Configuring SLNet settings in MaintenanceSettings.xml](xref:Configuration_of_DataMiner_processes#configuring-slnet-settings-in-maintenancesettingsxml)

### .NET Remoting with or without eventing

By default, clients connecting to a DataMiner Agent use .NET Remoting with eventing.

- **.NET Remoting with eventing**: When a client uses .NET Remoting with eventing, the DataMiner Agent will send server-initiated notifications to the client.

    So, when a client application such as DataMiner Cube connects to a DataMiner Agent, it will open a random port on the client to which the DMA will sent its notifications.

- **.NET Remoting without eventing**: When a client uses .NET Remoting without eventing, the DataMiner Agent will not send server-initiated notifications to the client.

    Instead, the client application will continuously poll the DataMiner Agent for notifications.

> [!NOTE]
> When a fallback from polling to eventing occurs, an information event is generated that contains the IP address and port that the events are sent to. Clients can use this information to detect why they cannot receive events via the callback method. When a client connects, the log files and diagnostic info also contain the external client IP address as seen from the DataMiner Agent.

### Manual configuration of client communication settings

To customize how Cube connects to a DMA for a specific computer:

1. In the Cube header bar, click the user icon or the user name (in versions prior to DataMiner 10.0.0/10.0.2), and select *Settings*.

2. Go to the *computer* tab of the *Settings* window.

3. On the *Connection* page, in the drop-down list next to *Connection type*, and select one of the following options: *Auto* (default), *Remoting* or *Web services.*

    > [!NOTE]
    > - Connecting via web services is not possible if WSE is not installed on the DMA.
    > - As WSE is deprecated, the Web Services option is no longer available from DataMiner 10.0.0 \[CU6\]/10.0.11 onwards.

4. Modify the different settings for the selected connection type as necessary:

    - **Destination port**: Select this option to specify a custom destination port number. If you specify “-1”, the port will be detected automatically.

    - **Polling interval**: The frequency at which the client application should poll the DMA, in milliseconds. If you want to use remoting and there are firewalls in your network or NAT is used, make sure this option is selected, to ensure that polling is used instead of eventing. For Web Services, the option is enabled by default.

    - **Use data compression** (Remoting only): Determines whether data are compressed or not. By default, this option is selected, in order to reduce network traffic.

    - **Custom binding IP address** (Remoting only): Only needed in case the server cannot automatically resolve the IP address that it needs to send callback events to the client, which can be the case when a VPN connection is used. The option only applies when eventing has been configured.

> [!NOTE]
> You can also configure this in the DataMiner Cube logon screen, before you actually log on. See [Overriding the default connection type](xref:Logging_on_to_DataMiner_Cube#overriding-the-default-connection-type).

The procedure above only applies to the one computer where it is done. If you want to change the default client communication settings for a DMA, you can do so in the file *ConnectionSettings.txt*.

For example, this is what the default setting to use eventing looks like in this file:

```txt
* type=RemotingConnection;polling=0;zip=true
```

To use polling (1000 ms) by default, change this as follows:

```txt
* type=RemotingConnection;polling=1000;zip=true
```

For more detailed information on where you can find this file and on the different settings it contains, see [ConnectionSettings.txt](xref:ConnectionSettings_txt#connectionsettingstxt).
