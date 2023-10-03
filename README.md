# Red Team Methodology for DNS Penetration Testing 
![Dns Penetration Testing Poster](https://github.com/BlackWolfed/DNS-Penetration-Testing-Methodology/blob/main/DNS%20Pentesting.png)
Hello! I'm **Mostafa Tamam, also known as Black_Wolf.** In the vast landscape of the internet, several hidden and obvious places might be vulnerable. **The Domain Name System (DNS)** serves as the internet's phonebook. Thus, I present my methodology for DNS penetration testing to identify potential vulnerabilities in DNS targets. This comprehensive map includes:

- **DNS Lookup**
   - Nslookup
- **DNSRecon**
   - DNSRecon
   - Fierce
   - HostMap
- **Zone Transfer**
   - Nslookup
   - Host
   - dig
- **Dns Enumaration**
	 - Nmap
	 - DNSenum
	 - DNSmap
- **Automation** 
- **Useful metasploit modules**
- **Active Directory servers**

## Recon
You can utilize my Red Team reconnaissance to enhance your visibility on the target.
> [Red Team Recon](https://github.com/BlackWolfed/RedTeamRecon)

## DNS Lookup
Web browsers interact through Internet Protocol (IP) addresses. DNS translates domain names to [IP addresses](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) so browsers can load Internet resources.

### Nslookup

Resolve a given hostname to the corresponding IP.

    nslookup Target.com
    
Reverse DNS lookup

    nslookup -type=PTR IP_address

MX(Mail Exchange) lookup

    nslookup -type=MX domain.com

## DNS Recon
### DNSRecon

    dnsrecon -d TARGET -D /usr/share/wordlists/dnsmap.txt -t std --xml ouput.xml
    
    dnsrecon -r  127.0.0.0/24 -n  <IP_DNS>  #DNS reverse of all of the addresses
    
    dnsrecon -r  127.0.1.0/24 -n  <IP_DNS>  #DNS reverse of all of the addresses
    
    dnsrecon -r  <IP_DNS>/24 -n  <IP_DNS>  #DNS reverse of all of the addresses
    
    dnsrecon -d active.htb -a  -n  <IP_DNS>  #Zone transfer

### Fierce

    fierce -dns targetdomain.com

### HostMap

    hostmap.rb -only-passive -t -with-zonetransfer <IP>

## Zone Transfer
### Nslookup

    nslookup
    server domain.com
    ls -d domain.com
### Host
To test Host domain: 
Criteria: **host -t ns(Name Server) < domain >**

    host -t ns domain.com
To test nameservers: 
Criteria: **host -l < domain > < nameserver >**

    host -l domain.com ns2.domain.com
### dig

    dig ANY @<DNS_IP> <DOMAIN>     #Any information
    dig A @<DNS_IP> <DOMAIN>       #Regular DNS request
    dig AAAA @<DNS_IP> <DOMAIN>    #IPv6 DNS request
    dig TXT @<DNS_IP> <DOMAIN>     #Information
    dig MX @<DNS_IP> <DOMAIN>      #Emails related
    dig NS @<DNS_IP> <DOMAIN>      #DNS that resolves that name
    dig -x 192.168.0.2 @<DNS_IP>   #Reverse lookup
    dig -x 2a00:1450:400c:c06::93 @<DNS_IP> #reverse IPv6 lookup

## Dns Enumaration
### Nmap

    nmap -F --dns-server <dns server ip> <target ip range>

### DNSenum

    dnsenum targetdomain.com
    dnsenum --target_domain_subs.txt -v -f dns.txt -u a -r targetdomain.com

### DNSmap

    dnsmap targetdomain.com -w <Wordlst file.txt>

## Automation 

    for sub in $(cat <WORDLIST>);do dig $sub.<DOMAIN> @<DNS_IP> | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
    
    dnsenum --dnsserver <DNS_IP> --enum -p 0 -s 0 -o subdomains.txt -f <WORDLIST> <DOMAIN>
    
## Useful metasploit modules
Perform enumeration actions

> auxiliary/gather/enum_dns
> 
## Active Directory servers

    dig -t _gc._tcp.lab.domain.com
    dig -t _ldap._tcp.lab.domain.com
    dig -t _kerberos._tcp.lab.domain.com
    dig -t _kpasswd._tcp.lab.domain.com
    nmap --script dns-srv-enum --script-args "dns-srv-enum.domain='domain.com'"
## Happy Hacking 🔥
