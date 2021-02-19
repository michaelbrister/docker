# escape=\ 

FROM mcr.microsoft.com/windows/servercore:1909-amd64 

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

COPY ["DatabaseBackupService/", "/Service/"]

WORKDIR "C:/Service/"

RUN "C:/Service/InstallUtil.exe" /LogToConsole=true /ShowCallStack NewInnovations.Utilities.Windows.Services.DatabaseBackupService.exe; \
    Set-Service -Name "\"New Innovations Database Backup Service\"" -StartupType Automatic; \
    Set-ItemProperty "\"Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\New Innovations Database Backup Service\"" -Name AllowRemoteConnection -Value 1

# -StartupType Automatic; 

CMD c:\Service\Wait-Service.ps1 -ServiceName 'New Innovations Database Backup Service' -AllowServiceRestart
