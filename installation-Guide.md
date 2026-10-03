Here is a comprehensive markdown-formatted summary of Minecraft Java Edition hosting, client setup, mod installation, and useful commands. You can copy and paste this directly into a `.md` file for your GitHub repository.

---

# Minecraft Java Edition: Local Server & Fabric Modding Guide

This guide covers setting up a local Minecraft Java Edition server, configuring TLauncher, installing custom content loader dependencies, adding weapon mods safely, and utilizing essential server management commands.

---

## 🛠️️ 1. What is Required to Get Started

To host a local modded survival server and connect to it using TLauncher, ensure you have the following downloaded and installed:

* **Java Runtime Environment (JRE)**: Compatible with your server version (e.g., Java 25 for modern snapshots/releases) - [Java 21 Official download link](https://download.oracle.com/java/21/latest/jdk-21_windows-x64_bin.exe).
* PowerShell commands for java 21.
# 1. Define the direct link and a temporary download location
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
* **Fabric Server Launcher**: Downloaded from the [Fabric Official Website](https://fabricmc.net/).
* **TLauncher**: Used as your game client gateway. Download it safely from the [TLauncher Official Website](https://tlauncher.org/).
* **Mods for Guns**: Downloaded from the [Just Enough Guns New(unofficial port)](https://modrinth.com/mod/just-enough-guns-neoforge).
---

## 📥 2. Installation & Directory Structure

### **A. Server-Side Installation**

1. Create a dedicated folder for your server (e.g., `D:\Games\Minecraft`).
2. Place your `fabric-server-launch.jar` file inside this directory.
3. Create a **`mods`** folder inside `D:\Games\Minecraft\` to store your backend server mod files.

### **B. Client-Side (TLauncher) Installation**

1. Open TLauncher and select the **Fabric profile** matching your game version (e.g., Fabric 26.3) from the version dropdown menu.
2. Open your client's `.minecraft` directory by clicking the folder icon near the version selection. Navigate to or create the **`mods`** folder at:
```text
C:\Users\<Your-Username>\AppData\Roaming\.minecraft\mods

```


3. Ensure that **identical mod `.jar` files** are present in both your server's `mods` folder and your client's `mods` folder to avoid network mismatch or registry rejection errors.



---

## 🧩 3. Essential Mods & Dependencies

For custom weapon mods (such as *Just Enough Guns* / *JEG*) to function smoothly, you must include the core library dependencies alongside the mod package:

* **[Fabric API](https://modrinth.com/mod/fabric-api)**: Core foundational API required by almost all Fabric mods.
* **[GeckoLib](https://modrinth.com/mod/geckolib)**: Animation library required for custom item modeling and rendering.
* **Just Enough Guns (JEG)**: The firearm gameplay modification package.

> **⚠️ Important Troubleshooting Rule:**
> Ensure weapon mods are placed strictly inside the **`mods`** folder as `.jar` files. Avoid placing client libraries or core mods into the world `datapacks` folder, as mismatched registry structures will trigger JSON parsing crashes or `swing_animation` component errors.

---

## 🎮 4. Connecting to Your Local Server

1. **Start the Server**: Open PowerShell inside your server directory and boot up the server via command:
```powershell
java -Xmx2G -jar fabric-server-launch.jar nogui

```


2. **Launch the Client**: Open TLauncher, ensure your profile is explicitly set to **Fabric** (matching your server version), and click *Enter the Game*.
3. **Join Multiplayer**: Navigate to `Multiplayer` -> `Direct Connection` (or add a server) and connect using your loopback address:
```text
127.0.0.1

```



---

## ⌨️ 5. Useful Commands & Settings

Use these controls directly in your server console (without a slash `/`) or in-game via chat (provided you have operator permissions via `op <username>`):

### **Environment & Time Manipulation**

* Set time to daytime:
```text
/time set day

```


* Set time to nighttime:
```text
/time set night

```


* Clear active weather conditions:
```text
/weather clear

```


* Lock weather cycles permanently (disables rain/snow cycles):
```text
/gamerule doWeatherCycle false

```



### **Item & Game Mode Shortcuts**

* Switch to Creative Mode (to access modded gun tabs directly from your inventory menu):
```text
/gamemode creative

```


* Switch back to Survival Mode:
```text
/gamemode survival

```


* Direct Item Grant Example (Vanilla testing or weapon components):
```text
/give @p crossbow[enchantments={levels:{'minecraft:quick_charge':5,'minecraft:multishot':1}}] 1

```
