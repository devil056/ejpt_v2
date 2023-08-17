- Windows is the dominant OS worldwide with a market share >=70% as of 2021
- The popularity and development of windows by individuals and companies makes it a prime target for attackers given the threat surface
- Over the last 15 years, windows has had its fair share or severe vuln, ranging from MS08-067(Conflicker) to MS17-010(Eternal blue)
- Given the popularity of windows, most of these vuln have publicly accessible exploit code making them relatively straightforward to exploit.
- MS Windows has various OS versions and releases which makes the threat surface fragmented in terms of vuln. For example the vuln that exist in windows 7 are not present in windows 10.
- Regardless of the various versions and releases, all windows OS share a likeness given the development model and philosohy
	- Windows OS have been developed in the C prog. langugage making them vuln to buffer overflows, arbitrary code execution etc.
	- By default windows is not configured to run securely and require a proactive implementation of security practices in order to configure windows to run securely
	- Newly discovered vuln are not immediately patched by MS and given the fragmented nature of windows many systems are left unpatched.
- The frequent releases of new versions of windows is also a contribution factor to exploitation, as many companies take a substantial length of time to upgrade their systems to the latest version of Windows and opt to use older versions that may be affected by an increasing number of vuln.
- In addition to inherent vuln, Windows is also vuln to cross platform vuln, for example SQL injection attacks.
- System/hosts running windows are also vuln to physical attacks like, theft, malicious peripheral devices etc.

[[WindowsIIS]]
[[Webdav with metasploit]]
[[SMB Exploiting with PsExec]]
[[MS17-010 SMB Vulnerability (Eternal Blue)]]
[[Exploit RDP]]
[[RDP Vuln(CVE-2019-0708) BlueKeep]]
[[Exploiting WinRM]]
