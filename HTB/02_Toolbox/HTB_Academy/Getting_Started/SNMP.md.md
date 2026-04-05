**Syntax:** `snmpwalk -v [version] -c [community] [IP] [OID]`

**Commands:**
```
# Enumerate SNMP with community string 'public'
snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0

# Bruteforce community strings using a wordlist
onesixtyone -c dict.txt 10.129.42.254
```

**Flags/Parameters:**
- `-v 2c`: Use SNMP version 2c.
    
- `-c`: Specify the "Community String" (essentially a password).