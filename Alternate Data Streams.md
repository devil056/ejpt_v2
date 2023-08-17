### Alternate data streams

- Alternate Data Streams (ADS) is an NTFS (New Technology File System) file attribute and was designed to provide compatibility with the MacOS HFS (Hierarchial File System)
- Any file created on an NTFS formatted drive will have two different forks/streams
	- Data stream - default stream that contains the data of the file
	- Resource stream - typically contains the metadata of the file
- Attackers can use ADS to hide malicious code or executables in legitimate files in order to evade detecion
- This can be done by storing the malicious code or executables in the file attribute resource stream (metadata) of a legitimate file.
- This technique is usually used to evade basic signature based AVs and static scanning tools.

-> notepad test.txt:winpeas.exe
-> del
-> type payload.exe > notepad test.txt:winpeas.exe
-> start test.txt:winpeas.exe
-> mklink wupdate.exe C:\Temp\test.txt:winpeas.exe