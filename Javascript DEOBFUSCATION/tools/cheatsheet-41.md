# Commands

| **Command**   | **Description**   |
| --------------|-------------------|
| `curl http:/SERVER_IP:PORT/` | cURL GET request |
| `curl -s http:/SERVER_IP:PORT/ -X POST` | cURL POST request |
| `curl -s http:/SERVER_IP:PORT/ -X POST -d "param1=sample"` | cURL POST request with data |
| `echo hackthebox \| base64` | base64 encode |
| `echo ENCODED_B64 \| base64 -d` | base64 decode |
| `echo hackthebox \| xxd -p` | hex encode |
| `echo ENCODED_HEX \| xxd -p -r` | hex decode |
| `echo hackthebox \| tr 'A-Za-z' 'N-ZA-Mn-za-m'` | rot13 encode |
| `echo ENCODED_ROT13 \| tr 'A-Za-z' 'N-ZA-Mn-za-m'` | rot13 decode |

# Deobfuscation Websites

| **Website** | **Use case** |
| :-:| :-: |
| [JS Console](https://jsconsole.com) | Running JS Code |
| [Prettier](https://prettier.io/playground/) | make the minifed code readable |
| [Beautifier](https://beautifier.io/) | Similar case as Prettier  |
| [JSNice](http://www.jsnice.org/) | Used for Deobfuscation |


# Misc

| **Command**   | **Description**   |
| --------------|-------------------|
| `ctrl+u` | Show HTML source code in Firefox |