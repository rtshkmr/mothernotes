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



# day 3: directory enumeration and the various tools for it

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

### week3 lab: [dirbuster directory enum](https://www.hackingarticles.in/comprehensive-guide-on-dirbuster-tool/)

- dirbuster can control the thread count, that's a benefit, but beware being network-throttled 


- Dirbuster can follow redirects!

- by controlling the number of requests per second, thread count and the request timeout duration, we can avoid server detection


### week 3 lab: [burpsuite directory enum]()

- note that the free version has some limitations:

    * need to config a [payload position](https://portswigger.net/burp/documentation/desktop/tools/intruder/positions) : 
    ```http            
    GET /§name§ HTTP/1.0
    Cookie: c=cval
    Content-Length: 17

    \r\n
    ```
    here, the `§name§` is a placeholder for the various words that we gonna try with
    * the payload you use can't add an entire list file, must add strings word by word.
    * it's awfully slow to do a full enumeration

* burpsuite is horrible when runing an entire dictionary attack but allows us to tweak payload positions and all so might be really useful when searching for specific directories and resources


### week 3 lab: [gobuster](https://github.com/OJ/gobuster)

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

# todos and toreads

* [CRLF characters](https://tools.ietf.org/html/rfc2616)
* [metasploit tutorial](https://www.youtube.com/watch?v=8lR27r8Y_ik)
* [case study of equifax leak: some apache struct caveat](https://www.brighttalk.com/webcast/13983/280311/behind-the-equifax-breach-a-deep-dive-into-apache-struts-cve-2017-5638)
* [honeypots](https://www.forcepoint.com/cyber-edu/deception-technology)

# Useful References
* [tldrs for man pages](https://tldr.sh/)
* [pentesteracademy relevant webapp pentesting labs](https://www.attackdefense.com/listing?labtype=pa-web-app-pentesting&subtype=pa-web-app-pentesting-video-labs)
* [google dorking methods](https://securitytrails.com/blog/google-hacking-techniques) and [a database for google dorks](https://www.exploit-db.com/google-hacking-database)
* [shodan search engine](https://www.shodan.io/)
* [burpsuite downloads](https://portswigger.net/burp/releases/professional-community-2020-4)
* hashcat, john the ripper,mdk <---some tools: 
* - hydra :
  https://tools.kali.org/password-attacks/hydra
  https://sectools.org/tag/pass-audit/
* [metasploit tutorial](https://www.youtube.com/watch?v=8lR27r8Y_ik)
* 

