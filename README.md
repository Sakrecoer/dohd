![dohd logo](https://raw.githubusercontent.com/dyne/dohd/master/docs/dohd.png)
<!-- josefin sans 400 -->

<p align="center">
  <a href="https://dyne.org">
    <img src="https://img.shields.io/badge/%3C%2F%3E%20with%20%E2%9D%A4%20by-Dyne.org-blue.svg" alt="Dyne.org" />
  </a>
</p>

<h4 align="center">
  <a href="#-install">💾 Install</a>
  <span> • </span>
  <a href="#-quick-start">🎮 Quick start</a>
  <span> • </span>
  <a href="#-configuration">🔧 Configuration</a>
  <span> • </span>
  <a href="#-acknowledgements">😍 Acknowledgements</a>
  <span> • </span>
  <a href="#-license">💼 License</a>
</h4>

Dohd (pron. doh-dee) is a minimalist DNS-over-HTTPS daemon that redirects all DoH queries
to a traditional DNS server open to UDP queries.

## Features

- Support for RFC8484 DNS-over-HTTPS (POST and GET method) - HTTP/2
- Record freshness derived from minimum TTL among answers
- Optimized and readable C code below 1500 locs
- Privacy focused, no logs are kept

***
# 💾 Install

Build system requirements: gcc or clang, GNU make

1. Ensure that [wolfSSL](https://github.com/wolfssl/wolfssl) is
installed on your system, and configured to support TLS 1.3 (configure
option: `--enable-tls13` or simply `--enable-all`).

2. Type `make`.

3. Executable will be found in `./src/dohd`

***
# 🎮 Quick start

Commands must be issued as root on the host machine.

Generate a fake local certificate for testing:

```
openssl req -nodes -newkey rsa:4096 -keyout /etc/test.key -out /etc/test.csr \
    -subj "/C=IT/ST=Rome/L=Rome/O=Local Host/OU=Testing Department/CN=example.com" \
    && openssl x509 -req -sha256 -days 365 \
    -in /etc/test.csr -signkey /etc/test.key -out /etc/test.crt
```

Create a dohd user and grant it access to certificates
```
adduser -D -H -s /bin/false dohd
chown dohd:dohd /etc/test.*
```

Start dohd as root to proxy all queries to a public dns and drop privileges to dohd user

```
dohd -c /etc/test.crt -k /etc/test.key -p 8053 -d 8.8.8.8 -u dohd -F
```

***
# 🔧 Configuration

There are several browsers compatible with DNS over HTTPS (DoH). The instructions below let you setup their connection to our demo server dns.dyne.org in order to protect your DNS queries from privacy intrusions and tampering.

- [Mozilla Firefox](https://dyne.github.io/dohd/#/Configure?id=mozilla-firefox)
- [Google Chrome](https://dyne.github.io/dohd/#/Configure?id=google-chrome)
- [Microsoft Edge](https://dyne.github.io/dohd/#/Configure?id=microsoft-edge)
- [Brave](https://dyne.github.io/dohd/#/Configure?id=brave)


## dohd daemon

Commandline options are few, help is shown using `-h`

```
Usage: dohd -c cert -k key [-p port] [-d dnsserver] [-F] [-u user] [-V] [-v] [-h]

	'cert' and 'key': certificate and its private key.
	'user' : login name (when running as root) to switch to (dropping permissions)
	Default values: port=8053 dnsserver="::1"
	Use '-h' for help
	Use '-V' to show version
	Use '-v' for verbose mode
	Use '-F' for foreground mode
```

- '-c *certificate*' - specifies which certificate for the TLS server. (Mandatory parameter)
- '-k *private-key*' - specifies the private key used by the TLS server. (Mandatory parameter)
- '-p *port*' - changes the listening port for the DoH service (default:8053)
- '-u *user*' - drop root privileges after binding to the TCP port by switching user (mandatory when running as root)
- '-F' - runs dohd in foreground (instead of creating a daemon)

***
# 😍 Acknowledgements

      Authors: Daniele Lacamera <root@danielinux.net>
               Denis "Jaromil" Roio <jaromil@dyne.org>

This software is provided "as is", without warranty of any kind,
express or implied, including but not limited to the warranties of
merchantability, fitness for a particular purpose, title and
non-infringement. in no event shall the copyright holders or anyone
distributing the software be liable for any damages or other
liability, whether in contract, tort or otherwise, arising from, out
of or in connection with the software or the use or other dealings in
the software.

***
# 💼 License

This is free software: you can redistribute it and/or modify it under
the terms of the GNU Affero General Public License, as published by
the free Software Foundation.

dohd is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
for more details.

You should have received a copy of the GNU Affero General Public
License along with dohd.  If not, see <http://www.gnu.org/licenses/>.
Dohd is licensed under the terms of GNU Affero General Public License
(GNU AGPL).  See COPYING for details.

