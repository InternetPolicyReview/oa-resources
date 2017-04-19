## Lagotto

Background information on altmetrics can be found at [altmetrics.org](http://altmetrics.org). The idea behind altmetrics is to go beyond

[Lagotto](http://lagotto.io) is an open-source application for tracking altmetrics. This guide is a resource for

*Note*: Lagotto is not all you need to begin showing altmetrics. Depending on how your journal is set up, you may need

## Factors to consider:

Before installing Lagotto, consider your journal's workflow, resources, and planned use of altmetrics.

### A note about versions:

The [Lagotto Github Repository](https://github.com/lagotto/lagotto) contains a few branches - the stable ones are 5.1, 4.5, 4.3, and 3.2. It is important to consider which branch you will want to install, since the 5.1 branch may not fit well into your existing workflow.

In particular, 5.1 removes support for the following features:
- Importing works into Lagotto directly. Instead, you have to import works into Lagotto with e.g. the CrossRef API, which assumes that you are already using this.
- Non-third-party authentication; this means that you need to go through a service like ORCID or Github.

On the other hand, 5.1 is faster, provides more functionality for third-party authentication, more integration with third-party services.

### Factors to consider:
You will also want to consider a few other factors.

- Where will you run Lagotto?
  - On a server at your institution?
  - Will you rent a cloud server?
  - Can you run it on the same server hosting your website?
- Who should have access to Lagotto?
  - Only journal staff, or also journal authors?
- What technical resources do you have at your disposal?


## Installation

*Note*: These instructions assume you're using a fresh installation of [Debian 8](http://debian.org).

#### Adding the necessary sources

Lagotto requires Ruby 2.2. There is a PPA for this, but using PPAs on Debian requires some additional software:
```sh
sudo apt-get install software-properties-common python-software-properties
```
and then:
```sh
sudo apt-add-repository ppa:brightbox/ruby-ng
```

You also need to some more necessary sources:

```
deb https://oss-binaries.phusionpassenger.com/apt/passenger jessie main
deb https://deb.nodesource.com/node_7.x jessie main
deb-src https://deb.nodesource.com/node_7.x jessie main
deb http://packages.erlang-solutions.com/debian jessie contrib
```

#### Required packages:

```sh
sudo aptitude install ruby2.2 ruby2.2-dev make curl git libmysqlclient-dev avahi-daemon libnss-mdns -y
```


#### CouchDB & Erlang

Lagotto uses CouchDB, which in turns requires Erlang. CouchDB needs to be compiled, but Erlang can be installed from its own repository.

First add:
```sh
sudo echo "deb http://packages.erlang-solutions.com/debian jessie contrib" > /etc/apt/sources.list.d/erlang-solutions.list
```

and then
```sh
wget -qO - http://packages.erlang-solutions.com/debian/erlang_solutions.asc | sudo apt-key add -
```

and then:
```sh
sudo aptitude update
sudo aptitude install build-essential curl libmozjs185-1.0 libmozjs185-dev libcurl4-openssl-dev libicu-dev wget curl
```

followed by the actual installation of Erlang:
```sh
sudo aptitude install erlang-dev=1:17.5.3 erlang-base=1:17.5.3 erlang-crypto=1:17.5.3 \
                      erlang-nox=1:17.5.3 erlang-inviso=1:17.5.3 erlang-runtime-tools=1:17.5.3 \
                      erlang-inets=1:17.5.3 erlang-edoc=1:17.5.3 erlang-syntax-tools=1:17.5.3 \
                      erlang-xmerl=1:17.5.3 erlang-corba=1:17.5.3 erlang-mnesia=1:17.5.3 \
                      erlang-os-mon=1:17.5.3 erlang-snmp=1:17.5.3 erlang-ssl=1:17.5.3 \
                      erlang-public-key=1:17.5.3 erlang-asn1=1:17.5.3 erlang-ssh=1:17.5.3 \
                      erlang-erl-docgen=1:17.5.3 erlang-percept=1:17.5.3 erlang-diameter=1:17.5.3 \
                      erlang-webtool=1:17.5.3 erlang-eldap=1:17.5.3 erlang-tools=1:17.5.3 \
                      erlang-eunit=1:17.5.3 erlang-ic=1:17.5.3 erlang-odbc=1:17.5.3 \
                      erlang-parsetools=1:17.5.3
```
next, put them on hold:
```sh
sudo aptitude hold erlang-dev erlang-base erlang-crypto erlang-nox erlang-inviso erlang-runtime-tools \
                      erlang-inets erlang-edoc erlang-syntax-tools erlang-xmerl erlang-corba \
                      erlang-mnesia erlang-os-mon erlang-snmp erlang-ssl erlang-public-key \
                      erlang-asn1 erlang-ssh erlang-erl-docgen erlang-percept erlang-diameter \
                      erlang-webtool erlang-eldap erlang-tools erlang-eunit erlang-ic erlang-odbc \
                      erlang-parsetools
```

**note**: These packages need to be kept on hold. Because apt-get ignores the holds in aptitude and doesn't have an equivalent feature, only use aptitude from here on out.

Download CouchDB and compile it:
```sh
wget http://apache.panu.it/couchdb/source/1.6.1/apache-couchdb-1.6.1.tar.gz
tar xzf apache-couchdb-1.6.1.tar.gz
cd apache-couchdb-1.6.1
./configure --prefix=/usr/local --with-js-lib=/usr/lib --with-js-include=/usr/include/js --enable-init
make && sudo make install
```


now set up the CouchDB environment:
```sh
sudo useradd -d /var/lib/couchdb couchdb
sudo mkdir -p /usr/local/{lib,etc}/couchdb /usr/local/var/{lib,log,run}/couchdb /var/lib/couchdb
sudo chown -R couchdb:couchdb /usr/local/{lib,etc}/couchdb /usr/local/var/{lib,log,run}/couchdb
sudo chmod -R g+rw /usr/local/{lib,etc}/couchdb /usr/local/var/{lib,log,run}/couchdb
sudo chown couchdb:couchdb /usr/local/etc/couchdb/local.ini
sudo ln -s /usr/local/etc/init.d/couchdb /etc/init.d/couchdb
sudo ln -s /usr/local/etc/couchdb /etc
sudo update-rc.d couchdb defaults
sudo /etc/init.d/couchdb start
```

You can test whether CouchDB is running now, but you'll do it again at the end, too:

```sh
curl http://127.0.0.1:5984/
```
