# obfs4proxy-openvpn

A Bash script for obfuscating OpenVPN traffic using obfs4proxy

## Overview

![obfs4proxy-openvpn diagram](https://github.com/HRomie/obfs4proxy-openvpn/blob/master/img/obfs4proxy-openvpn-diagram.png)

[obfs4proxy](https://github.com/Yawning/obfs4) developed by the [Tor Project](https://www.torproject.org/), is primarily written to obfuscate Tor traffic. But with a little effort, it can be used to [obfuscate any other TCP traffic as well](https://hamy.io/post/000d/how-to-hide-obfuscate-any-traffic-using-obfs4/).

While there are couple of obfs4proxy general wrappers around, this Bash script is specifically designed to make obfs4proxy work with OpenVPN. It's more of a helper than a wrapper since it bootstraps the start of obfs4proxy/OpenVPN and then gets out of the way.

It is specifically written for obfs4 transport protocol. But it also supports older obfs3 and obfs2 transports. Unless you have a good reason, you should stick to obfs4.

Since the script uses standard Linux commands, it should work in most major distros but it's been specifically tested on:

* Ubuntu 18.04
* Debian 9, 10
* CentOS 7
* Fedora 29

If you believe that it doesn't work on your system, let me know.

## Getting started

### Prerequisites

* Linux (obviously!)
* [Bash](https://www.gnu.org/software/bash/)
* [OpenVPN](https://openvpn.net/)
* [obfs4proxy](https://github.com/Yawning/obfs4)
* Standard Linux commands (e.g, sudo,grep,ps) which should be available on all distros.

OpenVPN and obfs4proxy can either be compiled from their source code or installed from your distros repository. Just don't forget to put them somewhere in your *PATH* if you decided to compile them yourself.

The script must be run as root to do its magic but it will use a dedicated account for running obfs4proxy by default. You can also make OpenVPN to drop its root privilege later on.

### Installing

* Download the [obfs4proxy-openvpn](obfs4proxy-openvpn) script, give it +x permission and put it in a location in your *PATH* (e.g, */usr/local/bin/*):
  * `wget https://raw.githubusercontent.com/HRomie/obfs4proxy-openvpn/master/obfs4proxy-openvpn`
  * `mv obfs4proxy-openvpn /usr/local/bin`
  * `chmod +x /usr/local/bin/obfs4proxy-openvpn`
* [obfs4proxy-openvpn.conf.sample](examples/obfs4proxy-openvpn.conf.sample) contains a sample of the needed config file. Edit it to your needs and save it as */etc/obfs4proxy-openvpn.conf* .
  * Use `obfs4proxy-openvpn --export-cert -` on the server to get the required obfs4 *CERT* for the client.
  * [openvpn_client.conf.obfs4.sample](examples/openvpn_client.conf.obfs4.sample) / [openvpn_server.conf.obfs4.sample](examples/openvpn_server.conf.obfs4.sample) contain samples of OpenVPN client/server configurations.
* [obfs4proxy-openvpn.service.sample](examples/obfs4proxy-openvpn.service.sample) contains sample of a systemd unit for obfs4proxy-openvpn.
  * By default, the provided OpenVPN configurations use pre-shared key. So the key should be created on the server and then be imported to the client as well.
    * Key creation on the server can be done using: `openvpn --genkey --secret /etc/openvpn/secret.obfs4.key`
    * Use the same location on the client (*/etc/openvpn/secret.obfs4.key*), to import the generated key

### Usage

`obfs4proxy-openvpn --help` should give you some basic info on the command line arguments.

Most needed documentations are placed in the sample files in [examples/](examples/) folder. That should be enough to get you started. But a more in-depth doc can be found here: [obfs4proxy-openvpn: Obfuscating OpenVPN traffic using obfs4](https://hamy.io/post/000f/obfs4proxy-openvpn-obfuscating-openvpn-traffic-using-obfs4proxy/)

After the initial startup, the execution is passed to openvpn and it stays in the foreground (just like the real openvpn execution). You may then use the provided systemd service sample file to run it as a service.

## Feedback

I would love to know you thoughts on this project. Please share them with me [here](https://hamy.io/post/000f/obfs4proxy-openvpn-obfuscating-openvpn-traffic-using-obfs4proxy/#disqus_thread).

## Author

* **Hamy** - [hamy.io](https://hamy.io)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

* [Tor Project developers](https://www.torproject.org/about/corepeople.html.en)
* OpenVPN developers and other open source communities.

## Helpful links

* [Project's dedicated blog post](https://hamy.io/post/000f/obfs4proxy-openvpn-obfuscating-openvpn-traffic-using-obfs4proxy/)
* [Tor Pluggable Transports](https://www.torproject.org/docs/pluggable-transports)
* [pt-spec-v1](https://gitweb.torproject.org/torspec.git/tree/pt-spec.txt)
* [obfs4-spec](https://gitweb.torproject.org/pluggable-transports/obfs4.git/tree/doc/obfs4-spec.txt)
* [IAT-Mode (Inter-Arrival Time Mode) study](https://people.torproject.org/~dcf/obfs4-timing/) by David Fifield
* [How to hide (obfuscate) any traffic using obfs4](https://hamy.io/post/000d/how-to-hide-obfuscate-any-traffic-using-obfs4/)
* [PTProxy](https://github.com/gumblex/ptproxy) wrapper
* [ptadapter](https://github.com/twisteroidambassador/ptadapter) wrapper
* [Shapeshifter](https://github.com/OperatorFoundation/shapeshifter-dispatcher) wrapper
* [obfsproxy-openvpn](https://github.com/khavishbhundoo/obfsproxy-openvpn) (note that it doesn't support obfs4)
