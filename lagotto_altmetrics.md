# Altmetrics with Lagotto

## Lagotto

Background information on altmetrics can be found at [altmetrics.org](http://altmetrics.org). Altmetrics aims go beyond traditional forms of measuring "academic impact" by drawing on data from online platforms.

[Lagotto](http://lagotto.io) is an open-source application for tracking such article-level metrics. This guide is a resource for journals or other publishers who wish to use Lagotto. <!-- add reasons to use lagotto -->

*Note*: Lagotto is not all you need to begin showing altmetrics. Depending on how your journal is set up, you may need:
- a way to track events that take place on your website, such as page views or pdf downloads. This may occur through e.g. Piwik, Google Analytics, or some other tracking solution.
- a way to visualize the metrics. The [Almviz](https://github.com/jalperin/almviz) project by Juan Alperin, Martin Fenner, and others is an excellent way to do this, but you can also look into other [D3.JS](https://d3js.org/) visualizations. Think about what forms of information portrayal would be useful for your readers.

## Factors to consider:

Before installing Lagotto, consider your journal's workflow, resources, and planned use of altmetrics.

### A note about versions:

The [Lagotto Github Repository](https://github.com/lagotto/lagotto) contains a few branches - the stable ones are 5.1, 4.5, 4.3, and 3.2. It is important to consider which branch you will want to install, since the 5.1 branch may not fit well into your existing workflow.

In particular, 5.1 removes support for the following features:
- Importing works into Lagotto directly. Instead, you have to import works into Lagotto with e.g. the CrossRef API, which assumes that you are already using this.
- Non-third-party authentication; this means that you need to go through a service like ORCID or Github.

On the other hand, 5.1 is faster, provides more functionality for third-party authentication, and more integration with third-party services.

### Additional factors:
You will also want to consider a few other factors.

- Where will you run Lagotto?
  - On a server at your institution?
  - Will you rent space on a cloud server?
  - Can you run it on the same server hosting your website?
- Who should have access to Lagotto?
  - Only journal staff, or also journal authors?
- What technical resources do you have at your disposal?


## Installation

*Note*: These instructions assume you're using a fresh installation of [Debian 8](http://debian.org).

#### Adding the ruby source

Lagotto requires Ruby 2.2. There is a PPA for this, but using PPAs on Debian requires some additional software:
```sh
sudo apt-get install software-properties-common python-software-properties
```
and then:
```sh
sudo apt-add-repository ppa:brightbox/ruby-ng
```


#### Required packages:

```sh
sudo aptitude install ruby2.2 ruby2.2-dev make curl git libmysqlclient-dev avahi-daemon libnss-mdns -y
```

Now follow the official instructions to the letter a bit:

#### Install databases

```sh
sudo apt-get install mysql-server redis-server -y
```

#### Install Memcached
Memcached is used to cache requests (in particular API requests), and the default configuration can be used.

```sh
sudo apt-get install memcached -y
```

#### Install Postfix
Postfix is used to send reports via email. The configuration is done in the `.env` file. More information can be found [here](http://guides.rubyonrails.org/action_mailer_basics.html).

```sh
sudo apt-get install postfix -y
```

#### Node.JS

You want to install Nodejs from its own official [repository](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions), rather than the Debian one.
Do this:
```sh
 curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
sudo aptitude install -y nodejs
```

#### Phusion Passenger and Nginx

Full instructions available on the [Phusion Passenger Website](https://www.phusionpassenger.com/library/install/nginx/install/oss/jessie/)

```sh
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo apt-get install -y apt-transport-https ca-certificates

# Add our APT repository
sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger jessie main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update

# Install Passenger + Nginx
sudo apt-get install -y nginx-extras passenger
```

Then `nano /etc/nginx/nginx.conf` and remove the `#` behind `# include /etc/nginx/passenger.conf;` to uncomment it.

Now `nano /etc/nginx/sites-enabled/lagotto.conf` and make sure it looks like this:
```
server {
  listen 80 default_server;
  server_name EXAMPLE.ORG;
  root /var/www/lagotto/public;
  access_log /var/log/nginx/lagotto.access.log;
  passenger_enabled on;
  passenger_app_env production;
}
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

#### Get Lagotto

```sh
mkdir -p /var/www
cd /var/www
```

Now you have to think about which version of Lagotto to grab. In this case, grab the 4.3 branch.
```
git clone git://github.com/lagotto/lagotto.git -b 4-3-stable
```
Also, install bundler to handle all the Ruby stuff:
```sh
sudo gem install bundler
```

#### Users and permissions
Up until now everything has been done as sudo. This is about to change.

```sh
useradd lagotto
passwd lagotto
```
enter a password, do it again, and remember it.
```sh
mkdir /home/lagotto
chown lagotto:users /home/lagotto
chown -R lagotto:users /var/www/lagotto
```
now log out of root and log in as the lagotto user.

#### Getting Lagotto ready:

There's an issue with the gemfile, so fix that first:
```sh
cd /var/www/lagotto
nano Gemfile.lock
```
and change `ruby-progressbar-1.7.4` to `ruby-progressbar-1.7.5`.

Now you can do a
```sh
bundle install
```

#### Prepare the .env file, part 1

Configuration settings for Lagotto are largely stored in the .env file. Get the template one by doing this:
```sh
cp .env.example .env
```

In particular look at the following settings:
- Set `RAILS_ENV` to `production`
- Think of a `DB_PASSWORD`.
- `DB_USERNAME` as `vagrant` doesn't make much sense outside of a vagrant setup either, but doesn't strictly need to be changed, either.
- change the `DB_SERVER_ROOT_PASSWORD` to something that isn't the default

#### Databases:

Try to run
```sh
rake db:setup RAILS_ENV=production
```
If that doesn't work, you have to do some stuff manually.
First, set the root password to whatever you have in the .env file:
```sh
mysqladmin -u root password NEWPASSWORD
```

Now create the mysql user manually:
```sh
mysql -u root -p
```
enter the `DB_SERVER_ROOT_PASSWORD` you just created.
Now create a new user, using the `DB_PASSWORD` and `DB_USERNAME` variables in the .env file from above:
```mysql
CREATE USER 'DB_USERNAME'@'localhost' IDENTIFIED BY 'DB_PASSWORD';
GRANT ALL PRIVILEGES ON * . * TO 'lagotto'@'localhost';
FLUSH PRIVILEGES;
quit
```


Now, do `rake db:setup RAILS_ENV=production` again.

Either way, you should be good now.
check if CouchDB is doing everything right:
```sh
curl -X PUT http://localhost:5984/lagotto
```

#### .env file, part 2

- Produce a new `SECRET_KEY_BASE` and a new `API_KEY` with `rake secret`

if you are planning on using third party authentication:
- choose an authentication method and set the `OMNIAUTH` variable accordingly
  - see below for instructions on doing so with Github.

#### Test Lagotto:

Now, use root to restart nginx:
```sh
sudo service nginx restart
```

You can now check to see if Lagotto is running by navigating a web browser to the IP address of the machine it's installed on.

Almost there!

Start sidekiq:

```sh
RAILS_ENV=production rake sidekiq:start
```

#### Create Admin user:

Unfortunately, the creation of admin users through the web interface at /users/sign_up is broken. You can do it through the ruby console, though:

```sh
rails console
```

and then 

```ruby
user=User.create!(:email=>'JANEDOE@EXAMPLE.COM',:password=>'PASSWORD',:name=>'JANEDOE',:role=>'admin')
```

## First steps with Lagotto

### Create / add works:

#### API

The best way to add works is using the API. This is how it works:

```sh
curl -X POST -H "Content-Type: application/json" -u JANEDOE@EXAMPLE.COM:PASSWORD -d '{"work":{"doi":"10.1371/journal.pone.0036790","year":2012,"month":5,"day":15,"title":"Test title"}}' http://HOSTNAME/api/v4/articles
```
Make sure to replace the values with an article in your journal, and HOSTNAME with the IP address.

#### Rails

You can also use the Rails console, such as like this:
```ruby
work=Work.create!(:doi=>'10.14763/2014.2.286',:title=>'Bitcoin: a regulatory nightmare to a libertarian dream',:published_on=>'2014-05-23',:created_at=>'2017-05-02 18:30:33',:canonical_url=>'policyreview.info/articles/analysis/bitcoin-regulatory-nightmare-libertarian-dream',:year=>2014,:month=>5,:day=>23,:pid=>'10.14763/2014.2.286')
```
Now go to /works and you should see the articles your Lagotto is tracking.

## Setup of Lagotto

In order to setup Lagotto to begin actively tracking your data you need to configure the sources. Sources that use publicly available APIs (Wikipedia is a good example) do not need configuration; others require API keys and secrets. You can get these from the various platforms.

You can use the rails console to serialize the data properly. First define the source you want to set up:
```ruby
twitter=Source.find(26)
```
and then use a command like the following:
```ruby
twitter.config=OpenStruct.new({:queue => "low", :rate_limiting => 1800, :cron_line => "* 6 * * *", :timeout => 30, :max_failed_queries => 200, :api_key => "YOUR API KEY", :api_secret => "YOUR API SECRET", :access_token => "" })
```
Note that Lagotto will retrieve the access token on its own.


## FAQ

Q = Question, P = Problem, A = Answer.

- Q: Events (such as Tweets) only show up with numeric information.

- Q: Events get lost after an update.

- P: The most likely problem is that the Lagotta App is not communicating to the CouchDB Database Correctly. Lagotto uses two database programs, MySQL and CouchDB, to store the event information that it receives from the APIs. In the MySQL Database, it stores these in the api_responses table; in the CouchDB database, it does in the lagotto database. Each event receives its own document with appropriate values to match. 

- A: Check to make sure that Lagotto is communicating with CouchDB sucessfully.
  - See if couchDB is running: 
    ```sh 
    curl http://localhost:5984/ 
    ```
    The above should respond with a message about the version of CouchDB (1.61)
  - Make sure that the Lagotto database is up:
    ``` sh
    curl http://localhost:5984/lagotto
    ```
  - if this answers with an error message, do the following:
    ```sh
    curl -X PUT http://localhost:5984/lagotto
    ```
  - now see if Lagotto saves data correctly.
  
