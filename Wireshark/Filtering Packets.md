
***
###### `Title :- Filtering Packets`
###### `Created on :- 2023-08-19 - 02:12`
###### `Created by:- Prem J`
***

#### `Filtering Operators -->`

- and - operator: and / `&&`
- or - operator: `or` / `||`
- equals - operator: `eq` / `==`
- not equal - operator: `ne` / `!=`
- greater than - operator: `gt` / ` >`
- less than - operator: `lt` / `<`

#### `Basic Filtering -->`

- `ip.addr == <IP address>` - filtering based on IP address

![[Pasted image 20230819021419.png]]

- The basic syntax of Wireshark filters is some kind of service or protocol like ip or tcp, followed by a dot then whatever is being filtered for example an address, MAC, SRC, protocol, etc.
- `ip.addr == <SRC IP> and ip.dst == <DST IP>` - Filtering based on source and destination.

![[Pasted image 20230819021551.png]]

- `tcp.port eq <port> or <protocol name>` - as or has entered we can't use `=`
- `udp.port eq <port> or <protocol name>`
- `frame.number==<number>`

>[!Note]
>In a case where the attacker uses commands we use the `follow > TCP Stream` option to get a detailed view of the commands used by the attacker

