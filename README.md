# Custom-Installation-of-ESP-IDF-for-ESP32-Development
This tutorial, we'll guide you through setting up a custom installation of ESP-IDF for ESP32 development. This setup will allow you to run multiple versions of ESP-IDF independently (such as master, stable, etc. ), utilize shortcuts for commonly used commands, by integrating PowerShell Core into VS Code for a streamlined development experience.
### Requirements:
- Windows OS
- VS Code
- PowerShell Core

### Steps:

1. **Create Folders:**
   - Create two folders named `IDF521` and `IDF521Tools`(to install idf ver 5.2.1).
  
   - For other versions of IDF you can create similar folders and follow these steps again.

2. **Download ESP-IDF:**
   - Open Windows PowerShell.
   - Navigate to the `IDF521` folder.
   - Run the command:
     ```
     git clone -b v5.2.1 --recursive https://github.com/espressif/esp-idf.git
     ```
     
3. **Install IDF Toolchain:**
  
     	 
   Now time to install IDF toolchain. By default the tool chain is installed inside the user home directory: $HOME/.espressif on Linux and ‎macOS, and folder %USERPROFILE%\.espressif on       Windows. ‎As we are going to install the tools into a different directories (d:\iot\IDF521Tools ‎), we will have to set the environment variable IDF_TOOLS_PATH before running the            ‎installation scripts.

   First run the script(to bypass execution policy for permanently for current user)
     ```
     Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope CurrentUser
      ```
     
	 Run command (to set the IDF_TOOLS_PATH to d:\iot\IDF521Tools temporarily as long as ‎PS (or VS Code) is running.)

	```
	 $Env:IDF_TOOLS_PATH = "d:\iot\IDF521Tools
	```
		
	 Run command (to check the IDF_TOOLS_PATH environment path is set correctly)
	 
	```
	$Env: IDF_TOOLS_PATH
	```
 
	Now that IDF_TOOLS_PATH is set correctly to d:\iot\IDF521Tools,  install IDF toolchain by running the ‎script inside the IDF folder "d:\iot\IDF521"
	
	```
	.\install.ps1 
	```
5. **Set Environment Variables:**
The installed tools are not yet added to the PATH environment variable. To make the tools usable from the command line, some environment variables must be set. ESP-IDF provides another script which does that.
   - Run export script Inside the "d:\iot\IDF521":
     ```
     .\export.ps1
     ```

7. **Add PowerShell Core in VS Code:**
   - Install PowerShell Core.
   - Open VS Code settings.json.
   - Set PowerShell Core as default terminal:
     ```
     {
         "terminal.integrated.shell.windows": "C:\\Program Files\\PowerShell\\7\\pwsh.exe"
     }
     ```

9. **Set Up VS Code for ESP-IDF Projects:**
   - Open your project folder in VS Code.
   - Modify .vscode\c_cpp_properties.json to include IDF components.
   
 
     	    "configurations": [
	        {
	            "name": "Win32",
	            "includePath": [
	                "${workspaceFolder}/**",
	                "D:/IOT/IDF521/components/**"
	               
	            ],



	 Configure VS Code Settings:

	- Open Setting of VSCode (File/Preferences/Settings).
	- Search for “C_Cpp.intelliSenseEngine”
	- Change C_Cpp.intelliSenseEngine to “Tag Parser”

8. **Add functions in PowerShell to add sortcut keys for lengthy commands:**
   - Open PowerShell profile.ps1.
   - Add functions for custom commands:
     ```powershell
     function idf521 {
         $Env:IDF_TOOLS_PATH="D:\IOT\IDF521Tools"
         . d:/IOT/IDF521/export.ps1
     }
     ```
		Now whenever the function is executed by typing “idf521” in PowerShell window,  the first line will set the environment variable of IDF_TOOLS_PATH to "D:\IOT\IDF521Tools" and the second line will set rest of env. variables so you can use esp-idf in 	current window of powershell. It adds the installed tools to the PATH environment variable.
		
		Everytime you start VSCode for esp32 development, you will have to run ```idf521``` first.  
		
		You can further add many functions such as:
		
			 function monit3 {idf.py -p com3 monitor} # Monitor on port 3
		 
		 	 function flash4 {idf.py -p com4 flash monitor} # Flash and monitor on port 4
		  
		  	 function fclean {idf.py fullclean} # Clean build folder
			
			 function prts {$portList = get-pnpdevice -class Ports -ea 0
				if ($portList) {
				     foreach($device in $portList) {
				          if ($device.Present) {
				               Write-Host $device.Name "(Manufacturer:"$device.Manufacturer")"
				          }
				     }
				}}  # Get the detail of ports in use
		  
		  	function ver {git describe} # get the current version of IDF




### Conclusion:
By following these steps, you've set up a custom ESP-IDF environment tailored to your needs. You can now develop ESP32 projects efficiently using VS Code and PowerShell Core, with the flexibility to switch between different IDF versions and utilize shortcuts for common tasks.

