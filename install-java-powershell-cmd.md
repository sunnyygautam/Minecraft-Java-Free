# PowerShell commands for java 21.
* 1. Define the direct link and a temporary download location.
```powershell
$url = "https://download.oracle.com/java/21/latest/jdk-21_windows-x64_bin.exe"
$output = "$env:TEMP\jdk21_installer.exe"

# 2. Add a standard User-Agent header to bypass Oracle's 403 firewall block
$userAgent = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"

Write-Host "Downloading Java 21 (LTS) Installer..." -ForegroundColor Cyan

try {
    # 3. Use Invoke-WebRequest with the browser User-Agent
    Invoke-WebRequest -Uri $url -OutFile $output -UserAgent $userAgent
    
    if (Test-Path $output) {
        Write-Host "Installing Java 21 silently..." -ForegroundColor Cyan
        
        # 4. Run the installer silently and wait for it to finish
        Start-Process -FilePath $output -ArgumentList "/s" -Wait
        
        # 5. Clean up the installer file
        Remove-Item $output
        Write-Host "Java 21 Installation Successfully Completed!" -ForegroundColor Green
    }
}
catch {
    Write-Host "An error occurred during download: $_" -ForegroundColor Red
}
```
* 2. Dynamically get the exact path found above.
```powershell
$jdkPath = (Get-ChildItem "C:\Program Files\Java" -Filter "jdk-21*" | Select-Object -First 1).FullName

if ($jdkPath) {
    # 2. Update JAVA_HOME permanently
    [Environment]::SetEnvironmentVariable("JAVA_HOME", $jdkPath, "Machine")
    
    # 3. Safely update System PATH (adding the new bin path at the very front so it takes priority over Java 8)
    $currentPath = [Environment]::GetEnvironmentVariable("Path", "Machine")
    if ($currentPath -notlike "*$jdkPath\bin*") {
        $newPath = "$jdkPath\bin;$currentPath"
        [Environment]::SetEnvironmentVariable("Path", $newPath, "Machine")
    }
    Write-Host "Environment variables updated to Java 21!" -ForegroundColor Green
} else {
    Write-Host "Failed to update variables because JDK 21 path was not found." -ForegroundColor Red
}
```
* 3. Pull the updated path directly from the machine's registry.
```powershell
$env:Path = [Environment]::GetEnvironmentVariable("Path", "Machine") + ";" + [Environment]::GetEnvironmentVariable("Path", "User")
$env:JAVA_HOME = [Environment]::GetEnvironmentVariable("JAVA_HOME", "Machine")
```
* 4. Verify it works in this exact window
```powershell
java -version
```
* 5. Run backend server.
```powershell
PS D:\Games\Minecraft> & "C:\Program Files\Eclipse Adoptium\jdk-25.0.4.101-hotspot\bin\java.exe" -Xmx4G -Xms4G -jar .\server.jar nogui

PS D:\Games\Minecraft> java -Xmx2G -jar fabric-server-launch.jar nogui
```
