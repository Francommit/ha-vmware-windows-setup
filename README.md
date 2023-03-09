# Home Assistant VMware Windows Setup

I want Vmware to auto-start my HomeAssistant VM on boot - SIMPLE. SO WHY IS IT SO HARD?
- The **Configure Auto Start VMs** has a weird bug where it will hang the VM despite who the Windows Service is set to
- Disable as many Network Adapters as possible and ensure you've got your VM set to bridged

### Setup a scheduled task to turn your VMware VM on
```
$action = New-ScheduledTaskAction -Execute 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe' -Argument '-ExecutionPolicy Bypass -Command "& {cd \"C:\Program Files (x86)\VMware\VMware Workstation\"; ./vmrun -T ws start \"C:\Hassio\Hassio.vmx\" nogui}"'
$trigger = New-ScheduledTaskTrigger -AtLogon
$settings = New-ScheduledTaskSettingsSet -AllowStartIfOnBatteries -DontStopIfGoingOnBatteries -RunOnlyIfNetworkAvailable
$user = $env:USERNAME
$principal = New-ScheduledTaskPrincipal -UserId $user -LogonType Interactive -RunLevel Highest
Register-ScheduledTask -TaskName 'Start Hassio VM' -Action $action -Trigger $trigger -Settings $settings -Principal $principal
```
