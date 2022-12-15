# AKC
AKC is Authorized Keys Command not American Kennel Club :-)

**What is AKC**

This software would like to demonstrate that there are comfortable and user friendly approach to manage OpenSSH access to servers.
SSH (read: SECURE shell) *is a cryptographic network protocol for operating network services securely over an unsecured network*,
the correct way to manage \*nix servers is to leave users/sysadmins to access server thru SSH directly, over a VPN or at least 
over the Internet. Work smart not hard, and leave Micro$oft and their shit outside datacenters!

**What is and NOT AKC**

Why you choose to include username into keys table? It's best practice to use an SSH key to login as a defined user to a server,
not to multiple users accounts with the same keys. Moreover AKC is an SSH key manager and not an AAA system (eg. not a RADIUS so 
no users and groups). Users at now are used only as administrators (to manage all keys/hosts/principals, maybe in the future AKC 
will be multi-tenant). AKC relies on **OpenSSH**'s configuration directives *AuthorizedKeysCommand* and *AuthorizedPrincipalsCommand* 
so **OpenSSH** is a requirements and AKC cannot manage certificates for networking and embedded devices, Dropbear or other lightweight
SSH server.

**AKC mini howto**

AKC can operate in *standalone mode* and *PKI mode* (support for PKI still in development).

You have to specify in your **/etc/ssh/sshd_config** for standalone mode:

```
AuthorizedKeysCommand /etc/ssh/akclient.py -m ack -f %f -u %u http://akc.server.url
AuthorizedKeysCommandUser nobody
```

You have to specify in your **/etc/ssh/sshd_config** for pki mode:

```
TrustedUserCAKeys /etc/ssh/ca.pub
AuthorizedPrincipalsCommand /etc/ssh/akclient.py -m pki -F %F -f %f -i %i -u %u http://akc.server.url
AuthorizedPrincipalsCommandUser nobody
```

**Differences with Keyper**

AKC is written in Python and it's a pure Flask app. It does NOT use javascript at all (no single line of code) or javascript
frameworks, and this is a deliberate choice for better compatibility and maintainability. It uses HTML templates only. Anther 
cornerstone is to never ever call external executables (eg. ssh-keygen) but only tu use Python modules. The last difference is 
the backend: AKC doen't use LDAP, it uses a SQLAlchemy backend so you can choose between relational DBMS eg. SQLite, MySQL/MariaDB, 
PostgreSQL. While I'm evaluating to manage SSH CA, KRL, etc. it won't turns in a password/key manager, I want manage public keys only.

**Acknowledgement**

Developed by Andrea Tuccia [wildstray](https://github.com/wildstray). This is Free and Open Source Software. Free as in freedom,
not free as in free beer. I'm available to provide advice or to be hired.
