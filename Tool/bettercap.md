## 36. bettercap(dsniff)
#bettercap
### 36.1 嗅探

| parameter           | default   | description                                                  |
| ------------------- | --------- | ------------------------------------------------------------ |
| `net.sniff.output`  |           | If set, the sniffer will write captured packets to this pcap file. |
| `net.sniff.source`  |           | If set, the sniffer will read from this pcap file instead of the current interface. |
| `net.sniff.verbose` | `false`   | If true, every captured and parsed packet will be sent to the  events.stream for displaying, otherwise only the ones parsed at the  application layer (sni, http, etc). |
| `net.sniff.local`   | `false`   | If true it will consider packets from/to this computer, otherwise it will skip them. |
| `net.sniff.filter`  | `not arp` | BPF filter for the sniffer.                                  |
| `net.sniff.regexp`  |           | If set, only packets with a payload matching this regular expression will be considered. |
| `net.fuzz.layers`   | `Payload` | Comma separated [types of layer](https://github.com/google/gopacket/blob/master/layers/layertypes.go#L13) to fuzz. |
| `net.fuzz.rate`     | `1.0`     | Rate in the [0.0,1.0] interval of packets to fuzz.           |
| `net.fuzz.ratio`    | `0.4`     | Rate in the [0.0,1.0] interval of bytes to fuzz for each packet. |
| `net.fuzz.silent`   | `false`   | If true it will not report fuzzed packets.                   |

### 36.2 代理（arp欺骗）

#### 36.2.1 arp spoof

| parameter              | default           | description                                                  |
| ---------------------- | ----------------- | ------------------------------------------------------------ |
| `arp.spoof.targets`    | `<entire subnet>` | A comma separated list of MAC addresses, IP addresses, IP ranges or aliases to spoof ([a list of supported range formats](https://github.com/malfunkt/iprange)). |
| `arp.spoof.whitelist`  |                   | A comma separated list of MAC addresses, IP addresses, IP ranges or aliases to skip while spoofing. |
| `arp.spoof.internal`   | `false`           | If true, local connections among computers of the network will be  spoofed as well, otherwise only connections going to and coming from the external network. |
| `arp.spoof.fullduplex` | `false`           | If true, both the targets and the gateway will be attacked, otherwise only the target (**if the router has ARP spoofing protections in place this will make the attack fail**). |

#### 36.2.2 http proxies

| parameter              | default               | description                                                  |
| ---------------------- | --------------------- | ------------------------------------------------------------ |
| `http.port`            | `80`                  | HTTP port to redirect when the proxy is activated.           |
| `http.proxy.address`   | `<interface address>` | Address to bind the HTTP proxy to.                           |
| `http.proxy.port`      | `8080`                | Port to bind the HTTP proxy to.                              |
| `http.proxy.sslstrip`  | `false`               | Enable or disable SSL stripping.                             |
| `http.proxy.script`    |                       | Path of a proxy module script.                               |
| `http.proxy.injectjs`  |                       | URL, path or javascript code to inject into every HTML page. |
| `http.proxy.blacklist` |                       | Comma separated list of hostnames to skip while proxying (wildcard expressions can be used). |
| `http.proxy.whitelist` |                       | Comma separated list of hostnames to proxy if the blacklist is used (wildcard expressions can be used). |

### 36.3 dns spoof

| parameter           | default               | description                                                  |
| ------------------- | --------------------- | ------------------------------------------------------------ |
| `dns.spoof.domains` |                       | Comma separated values of domain names to spoof.             |
| `dns.spoof.address` | `<interface address>` | IP address to map the domains to.                            |
| `dns.spoof.all`     | `false`               | If true the module will reply to every DNS request, otherwise it will only reply to the one targeting the local pc. |
| `dns.spoof.hosts`   |                       | If not empty, this hosts file will be used to map domains to IP addresses. |

### 36.4 https sniff

| parameter                                    | default                                      | description                                                  |
| -------------------------------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| `https.port`                                 | `443`                                        | HTTPS port to redirect when the proxy is activated.          |
| `https.proxy.address`                        | `<interface address>`                        | Address to bind the HTTPS proxy to.                          |
| `https.proxy.port`                           | `8083`                                       | Port to bind the HTTPS proxy to.                             |
| `https.proxy.certificate`                    | `~/.bettercap-ca.cert.pem`                   | HTTPS proxy certification authority TLS certificate file.    |
| `https.proxy.key`                            | `~/.bettercap-ca.key.pem`                    | HTTPS proxy certification authority TLS key file.            |
| `https.proxy.certificate.bits`               | `4096`                                       | Number of bits of the RSA private key of the generated HTTPS certificate authority. |
| `https.proxy.certificate.commonname`         | `Go Daddy Secure Certificate Authority - G2` | Common Name field of the generated HTTPS certificate authority. |
| `https.proxy.certificate.country`            | `US`                                         | Country field of the generated HTTPS certificate authority.  |
| `https.proxy.certificate.locality`           | `Scottsdale`                                 | Locality field of the generated HTTPS certificate authority. |
| `https.proxy.certificate.organization`       | `GoDaddy.com, Inc.`                          | Organization field of the generated HTTPS certificate authority. |
| `https.proxy.certificate.organizationalunit` | `https://certs.godaddy.com/repository/`      | Organizational Unit field of the generated HTTPS certificate authority. |
| `https.proxy.sslstrip`                       | `false`                                      | Enable or disable SSL stripping.                             |
| `https.proxy.script`                         |                                              | Path of a proxy module script.                               |
| `https.proxy.injectjs`                       |                                              | URL, path or javascript code to inject into every HTML page. |
| `https.proxy.blacklist`                      |                                              | Comma separated list of hostnames to skip while proxying (wildcard expressions can be used). |
| `https.proxy.whitelist`                      |                                              | Comma separated list of hostnames to proxy if the blacklist is used (wildcard expressions can be used). |