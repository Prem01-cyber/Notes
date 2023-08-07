#### `Brief -->`

- In the wast world of internet the systems use IP (Internet Protocol) in order to communicate with each other. But using the IP address directly is not easy to remember.
- This is where DNS (Domain Name System) comes in, it is used to resolve IP addresses to domain names.

		104.26.10.226 --> hackthebox.com

- When a user types `www.facebook.com` into their web browser, a translation must occur between what the user types and the IP address required to reach the `www.facebook.com` webpage.

#### `Advantages -->`

- It allows names to be used **instead of numbers** to identify hosts.
- It is a **lot easier to remember a name** than it is to recall a number.
- By merely re-targeting a name to the new numeric address, **a server can change numeric addresses** without having to notify everyone on the Internet.
- A single name might refer to several hosts **splitting the workload** between different servers.

- There is a hierarchy of names in the DNS structure[^1].

Example --> www.facebook.com

> [!Note]
> The system root or highest level domain or root domain is unnamed '.'

- gTLD --> Generic Top Level Domain --> tell user domain's their purpose -- less in number
- ccTLD --> Country Code Top Level Domain[^2] --> for geographical purpose -- more

A manager for each nation organises country code domains. These managers provide a public service on behalf of the Internet community. Resource Records are the results of DNS queries and have the following structure:

|  |  |
| --- | --- |
| `Resource Record` | A domain name, usually a fully qualified domain name, is the first part of a Resource Record. If you don't use a fully qualified domain name, the zone's name where the record is located will be appended to the end of the name. |
| `TTL` | In seconds, the Time-To-Live (`TTL`) defaults to the minimum value specified in the SOA record. |
| `Record Class` | Internet, Hesiod, or Chaos |
| `Start Of Authority` (`SOA`) | It should be first in a zone file because it indicates the start of a zone. Each zone can only have one `SOA` record, and additionally, it contains the zone's values, such as a serial number and multiple expiration timeouts. |
| `Name Servers` (`NS`) | The distributed database is bound together by `NS` Records. They are in charge of a zone's authoritative name server and the authority for a child zone to a name server. |
| `IPv4 Addresses` (`A`) | The A record is only a mapping between a hostname and an IP address. 'Forward' zones are those with `A` records. |
| `Pointer` (`PTR`) | The PTR record is a mapping between an IP address and a hostname. 'Reverse' zones are those that have `PTR` records. |
| `Canonical Name` (`CNAME`) | An alias hostname is mapped to an `A` record hostname using the `CNAME` record. |
| `Mail Exchange` (`MX`) | The `MX` record identifies a host that will accept emails for a specific host. A priority value has been assigned to the specified host. Multiple MX records can exist on the same host, and a prioritized list is made consisting of the records for a specifi |

>[!tip]
>PTR record is the data verifying that the IP address matching the domain name and its the reverse with the A record, which provides the IP address with the domain name  

#### `Enumeration -->`

- upon using tools like **nslookup**, **dig** we can enumerate DNS, refer to [[information_gathering_tools]] for the syntax and examples of the mentioned tools.
- The recent RFC8482 States that the **ANY** query has been abolished so we may not get any results from that query.

>[!Note]
>	dig facebook.com @1.1.1.1
>The entry starts with the complete domain name, including the final dot. The entry may be held in the cache for `169` seconds before the information must be requested again. The class is understandably the Internet (`IN`).

[^1]: Refer Class work with DNS In detail.
[^2]: just an example list of ccTLD's [ISO-3166-1 table](https://en.wikipedia.org/wiki/List_of_ISO_3166_country_codes)

