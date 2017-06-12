# Sudo for Powershell

sometimes you need to run a command as an elevated user, here is a simple function that can be used to ask for elevated permissions to run a function


```powershell
function sudo  
{
  $file, [string]$arguments = $args;
  $psi = new-object System.Diagnostics.ProcessStartInfo $file;
  $psi.Arguments = $arguments;
  $psi.Verb = "runas";
  $psi.WorkingDirectory = get-location;
  [System.Diagnostics.Process]::Start($psi);
}
```


Full PS1 example usage

```powershell
function sudo  
{
  $file, [string]$arguments = $args;
  $psi = new-object System.Diagnostics.ProcessStartInfo $file;
  $psi.Arguments = $arguments;
  $psi.Verb = "runas";
  $psi.WorkingDirectory = get-location;
  [System.Diagnostics.Process]::Start($psi);
}
sudo route delete 0.0.0.0 192.168.254.1
```
