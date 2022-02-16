# escape=`

#FROM mcr.microsoft.com/dotnet/framework/runtime:3.5
FROM mcr.microsoft.com/dotnet/framework/aspnet:3.5

# Enable IIS .Net 4.5
RUN dism /online /enable-feature /all /featurename:IIS-ASPNET45 

#ENABLE IIS
RUN powershell -Command `
    Add-WindowsFeature File-Services; `
    Add-WindowsFeature FS-FileServer; `
    Add-WindowsFeature Web-Server; `
    Add-WindowsFeature Web-WebServer; `
    Add-WindowsFeature Web-Common-Http; `
    Add-WindowsFeature Web-Static-Content; `
    Add-WindowsFeature Web-Default-Doc; `
    Add-WindowsFeature Web-Dir-Browsing; `
    Add-WindowsFeature Web-Http-Errors; `
    Add-WindowsFeature Web-Http-Redirect; `
    Add-WindowsFeature Web-App-Dev; `
    Add-WindowsFeature Web-Asp-Net; `
    Add-WindowsFeature Web-Net-Ext; `
    Add-WindowsFeature Web-ASP; `
    Add-WindowsFeature Web-ISAPI-Ext; `
    Add-WindowsFeature Web-ISAPI-Filter; `
    Add-WindowsFeature Web-Includes; `
    Add-WindowsFeature Web-Health; `
    Add-WindowsFeature Web-Http-Logging; `
    Add-WindowsFeature Web-Log-Libraries; `
    Add-WindowsFeature Web-Request-Monitor; `
    Add-WindowsFeature Web-Http-Tracing; `
    Add-WindowsFeature Web-Security; `
    Add-WindowsFeature Web-Basic-Auth; `
    Add-WindowsFeature Web-Windows-Auth; `
    Add-WindowsFeature Web-Digest-Auth; `
    Add-WindowsFeature Web-Client-Auth; `
    Add-WindowsFeature Web-Cert-Auth; `
    Add-WindowsFeature Web-Url-Auth; `
    Add-WindowsFeature Web-Filtering; `
    Add-WindowsFeature Web-IP-Security; `
    Add-WindowsFeature Web-Performance; `
    Add-WindowsFeature Web-Stat-Compression; `
    Add-WindowsFeature Web-Dyn-Compression; `
    Add-WindowsFeature Web-Scripting-Tools; `
    Add-WindowsFeature Web-Mgmt-Service; `
    Add-WindowsFeature Web-Mgmt-Compat; `
    Add-WindowsFeature Web-Metabase; `
    Add-WindowsFeature Web-WMI; `
    Add-WindowsFeature Web-Lgcy-Scripting; `
    Add-WindowsFeature NET-Framework-Core; `
    Add-WindowsFeature NET-HTTP-Activation; `
    Add-WindowsFeature NET-WCF-HTTP-Activation45; `
    Add-WindowsFeature WAS; `
    Add-WindowsFeature WAS-Process-Model; `
    Add-WindowsFeature WAS-NET-Environment; `
    Add-WindowsFeature WAS-Config-APIs; `
    Add-WindowsFeature Web-WebSockets;

COPY ["ABCpdf.msi", "/"]
COPY ["abcpdfreg.ps1", "/"]

# ABCPdf requires  .net 3.5 
# Powershell has to be run in 32-bit mode
RUN msiexec /i C:\ABCpdf.msi /quiet /qn /passive /L c:\abcpdf-install-log.txt & `
    %SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe -Command c:\abcpdfreg.ps1

# Add AppPool
RUN powershell -Command `
    mkdir C:\inetpub\wwwroot\Lists; `
    Remove-Website 'Default Web Site'; `
    New-WebSite -Name Lists -PhysicalPath C:\inetpub\wwwroot; `
	New-WebAppPool Lists; `
	New-WebApplication -Name Lists -Site 'Lists' -PhysicalPath C:\inetpub\wwwroot\Lists -ApplicationPool Lists; 

# Change AppPool properties
RUN C:\Windows\System32\inetsrv\appcmd.exe set app /app.name:"Lists/" /enabledProtocols:"http,net.tcp" & `
	C:\Windows\System32\inetsrv\appcmd set apppool /apppool.name:Lists /enable32BitAppOnWin64:true & `
	C:\Windows\System32\inetsrv\appcmd set apppool /apppool.name:Lists /processModel.identityType:"LocalSystem" & `
	C:\Windows\System32\inetsrv\appcmd set apppool /apppool.name:Lists /managedRuntimeVersion:"v4.0"

COPY ["RMSImages.zip", "c:\\RMSImages.zip"]
#COPY ["RMSImages", "/inetpub/wwwroot/RMSImages"]
COPY ["web.config", "/inetpub/wwwroot/web.config"]
COPY ["hc.txt", "/inetpub/wwwroot"]
COPY ["Legacy.Lists/", "/inetpub/wwwroot/Lists"]

RUN powershell -command Expand-Archive -Path c:\RMSImages.zip -DestinationPath c:\inetpub\wwwroot\

EXPOSE 80
