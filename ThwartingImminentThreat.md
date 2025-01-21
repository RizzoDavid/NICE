# Thwarting Imminent Threat

# Challenge 
    - Defend Domain Controller against External Blue

# Steps
    The first step to to look at the network map. The domain controller is located at 172.16.30.5. I ran a nmap scan to see the open ports along with vulnerabilities. namp needed to be run with the -Pn flag because the system denied the original request. 
    - nmap -Pn --script vuln 172.16.30.5
    - Windows Server 2012 R2 Standard
## Discovered 18 open ports. 
        - 22 | SSH
        - 53 | Domain
        - 88 | Kerberos-sec
        - 135 | msrpc
        - 139 | netbios-ssn
        - 389 | ldap
        - 445 | microsoft-ds
        - 464 | kpasswd5
        - 593 | http-rpc-epmap
        - 636 | ldapssl
        - 3268 | globalcatLDAP
        - 3269 | globalcatLDAPssl
        - 49154 | unknown
        - 49155 | unknown
        - 49157 | unknown
        - 49158 | unknown
        - 49159 | unknown
## Vulnerabilites
    Found 1 Vulnerability 
    - CVE-2017-0143 | smb-vuln-ms17-010
    - Risk: High
    - Remote Code Execution
    - NickName: EternalBlue

## Attempted Resolutions
    1. Updating windwos to include the latest security updates. 
        - Did not work. After running vulnerability scan the vulerability was still detected. 
        - Code 57 updating windows .net framework
    2. Attempted to download patch from Microsoft catolog. All files are unable to install on system. 
    3. Disabled SMB1 Protocol on SMB
            - Set-SmbServerConfiguration -EnableSMB1Protocol $false
        - Vulnerability was not found. Successfully mitigated. 
