this markdown file is to be used in conjunction with the slides and is supposed to track my self learning...[hardlinked!]

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






tldrs for man pages: https://tldr.sh/

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











# Current Reading List: 

webapp vs website: https://www.guru99.com/difference-web-application-website.html

using telnet to send http request?? aren't they diff protocols though  https://www.the-art-of-web.com/system/telnet-http11/

more on IP addrs and subnets, representation in CIDR(classless inter domain routing) https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing

google dorking and db hacking: 
https://securitytrails.com/blog/google-hacking-techniques
https://www.exploit-db.com/google-hacking-database
https://www.shodan.io/


burp downloads: https://portswigger.net/burp/releases/professional-community-2020-4


metasploit: https://www.youtube.com/watch?v=8lR27r8Y_ik





# Day 2: database


- nmap's mongodb-brute script uses a default dictionary (it's a hybrid of )


Reading list: 
- more on webshells: https://www.acunetix.com/blog/articles/introduction-web-shells-part-1/
- hashcat, john the ripper,mdk <---some tools: 
- https://dev.mysql.com/doc/mysql-security-excerpt/8.0/en/connection-control-installation.html  <-- setting limits on number of queries in a db
- 
