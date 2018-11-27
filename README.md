# zabbix_ip_check_spam_lists

Public spam databases or “black lists” of IP addresses contain information about IP, which for some reason has been declared unfriendly to users. We will not delve into the technological details; It is important that email programs and services use information from these and their own databases in order to protect recipients' email inboxes from unsolicited email, from spam.

### The essence of the problem

If your IP address is blacklisted, the recipients will not receive your emails.

IP penetration into public spam databases threatens the onset of a corporate mail collapse. This is unpleasant, even if the email address on the domain is only 5 and all users can be temporarily “transplanted” to “regular” mail on Yandex or Mail.ru. But, when more than 50 mailboxes integrated with the CRM system are “nailed” to the internal mail server, the problem becomes catastrophic.

### Decision

``To know, to foresee; foresee to manage. `` O. Comte

It is possible and necessary to calculate the looming threat before the client feels the punitive measures of mail servers, and the sales department in a panic will collectively lynch the local admin. To do this, we have created an automatic IP monitoring script notifying the user of possible problems.
### How does it work?

Checking the IP address for the presence in the black list (DNSBL) is carried out as follows: specify the checked IP in the DNS PTR notation (that is, vice versa “front to back”) and add the DNSBL domain name of the server. If a response from the server is received, then the address being checked is blocked: that is, the IP is seen in one or more blacklists. Regardless of the specifics of the response (it can be any), its very fact indicates that the IP is in the spam database.

## How to use
- Put the script in the directory specified in the zabbix-server config in the ExternalScripts parameter, for example in ``/usr/lib/zabbix/externalscripts/ip_blacklist.sh``
- We grant launch rights, for example, ``chmod + x /usr/lib/zabbix/externalscripts/ip_blacklist.sh``
- We check that all utilities used are installed on the server (xargs, host, grep, wc), for example, by running the handles ``/usr/lib/zabbix/externalscripts/ip_blacklist.sh 127.0.0.1``
- We create the necessary items, for example, ``ip_blacklist [192.168.0.1]``, the type ``External check``, and the trigger on ``.last> 0``
-  If necessary, we put items with a trigger into a template and bind it to the hosts.
- More details here: https://www.zabbix.com/documentation/4.0/manual/config/items/itemtypes/external
