this markdown file is to be used in conjunction with the slides...[hardlinked!]

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

HTTP supports both TCP and UDP, typically TCP is used as the underlying protocol. A 3 way handshake.

### netcat / curl / wireshark for http requests

- note in the app layer, you'll see under the HTTP dropdown field for the packet in wireshark that you can see: 
     * info about the server that's replying...
- netcat is a replacement for telnet. remember to use `connection close` to close up after you pipeline HTTP requests since the HTTP connection will exist for q long.
  can use netcat to frame your own http requests



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

- note that trace, put, delete may not have been enabled for all servers...


## lab2 HTTP method enumeration

self signed http ssl certificate? 

- purpose: get some interesting insights on what's available

- first thing is to check what services there are: using `dirb <ipaddr>`
   - has a dict of ipaddr and it will see if that dir exists under that place
    as there is no definite way to find all directories present on the server if the directory listed is not allowed (which is the common case), dirb is trying different directory names from the dictionary to see if this specific one exist
    It is dictionary attack to find the present directories.

    . dirb will be able to show us the common dir.
    . we might look at uploads dir to see if there's some interesting stuff left available: see "google dorking" 
      where info available in the uploads dir shows up via google searches 
      look at "google site operators" 
      nb: just cuz google made it discoverable doesn't mean that it's legal to get that info

- note that ARP is more at the mac addr level

- use `curl` to run the get request manually  the `-X GET` specifies the Methodology. note that `BURP` is a gui alternative to curl.
- e.g. using the OPTOINS verb will return the... 

- you may uncover interesting endpoints that allows us to download/upload stuff 
   a webdav put available then you can compromise the server.

- forms and post methods.. see use the OPTIONS verb to check if posting is allowed


- check the /uploads dir. check if put method is allowed

   . web DAV for sharing and collabing doc over a web protocol. should be enabled w the proper authentication and authorisation...
     using highlevel software might inadvertently open up some of these vulnerabilities, making the server insecure 

     . **core problem: insecure web DAV deployment**
    
- if we find that we can make PUT requests
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

## todo for session 1: 

- learn burp 
-  do lab 1 and 2 
-  look at networking recon 
