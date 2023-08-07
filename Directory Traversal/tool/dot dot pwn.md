This useful fuzzer was written in the Perl programming language. And surprisingly, not only can it be run from most Linux operating systems, but Windows as well. Nevertheless, it is so useful that it is included in the Kali packages. In fact, it was included in BackTrack Linux, which is Kali’s predecessor. The following include dotdotpwn’s different modules:

- HTTP and HTTP URL
    
- FTP and TFTP (Trivial File Transfer Protocol)
    
- A protocol independent payload module
    
- STDOUT
    

**Syntax and Options**

Let’s start by taking a high-level overview of the tool, it’s syntax, and it’s options. The following is the basic syntax of the command:

- **./dotdotpwn.pl -m [module] -h [host] [additional_options]**

Furthermore, the following are the possible options and their respective values you can append to the end of the command:

- **-m <module> Selects a module such as http, http-url, ftp, tftp, payload, or stdout
- -h <host> Specifies a hostname to target
- -O Uses NMAP for intelligent operating system detection
- -s Detects the target’s service version
- -d Deep of traversals
- -f Specific filename
- -u <url> URL with the part to be fuzzed marked as TRAVERSAL
- -k [string_pattern] String pattern to match in the response if it’s vulnerable
- -U <username> Specifies the default username
- -P <password> Specifies the default password
- -p <file> Filename with the payload to be sent and the part to be fuzzed marked as TRAVERSAL
- -x <port> Port to connect default: HTTP=80, FTP=21, TFTP=69
- -t <number> Time in milliseconds between each test default: 300
- -b Break after the first vulnerability is found
- -q Quiet mode

In practice, you won’t need to use each of these options every time you use dotdotpwn. Instead, you only need to know a small subset of these options, though I want to point out how flexible this fuzzer tool is.

**Dotdotpwn Tutorial and Example**

In this brief example, we’re going to be running through an example that tests a target for HTTP weaknesses and vulnerabilities. The command syntax is as follows:

- **./dotdotpwn.pl -m http -h 192.168.1.1 -x 8080 -f /etc/hosts -k “localhost” -d 8 -t 200 -s**
    

In this example, note first that it is using the HTTP module and targeting 192.168.1.1. Also, it’s worth noting that it’s listening on port 8080, which is typically used to establish HTTPS connections. Next, the **-f** command will attempt to capture the etc/hosts file, and look for the keyword “localhost.”

This is pretty typical syntax for the HTTP module, but note that you can substitute different values for the options. For instance, you could change the listening port number to 80 (HTTP). In addition, you can append additional options to the command.