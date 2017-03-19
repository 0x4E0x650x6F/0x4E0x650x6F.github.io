---
layout: page 
title :  Install Openvas 8 with Postgres on Kali linux Rolling
categories : [linux, os, Kali Linux]
---

This one is something that took quite some time to pull off while searching, most of the information I found was either wrong or incomplete, so here we go.  

## Objectives  

- Build **openvas** with **postgresql** support.  
- Make required configuration changes to **Kali linux source**, such as service scripts. 
- Create **.deb** package with kali linux source.  


## Requirements

To pull this off I will be using the **Kali Linux Rolling v4.6.4**, other than the usual build tools we will need **Postgresql** libraries to build **Openvas-Manager** with **Postgresql**.  


## Database Setup  

We will start with the Database preparations, we are told by documentation on **Openvas** document that we will need user equal to login user if your using **Kali** your probably using root.  

{% highlight bash %}
sudo postgres
createuser -DRS root
createdb -O root tasks
{% endhighlight %} 

In the previous commands we create a user, with the following privileges.

- -D: The new user will not be allowed to create databases.
- -R: The new user will not be allowed to create new roles.
- -S: The new user will not be a superuser.

And new database with **root** as its owner, we will then create a new role, and assign it to the new database user **root**, and load the uuid-ossp database extension.  


{% highlight bash %}
psql tasks
create role dba with superuser noinherit;
grant dba to root;
create extension "uuid-ossp";
{% endhighlight %} 

## Dependencies  

 The stuff needed to build this are as follows:

{% highlight bash %}
apt-get update
apt-get install postgresql-contrib postgresql-server-dev-9.5
{% endhighlight %}  

Does are the required libraries to **openvas** to **postgresql**.  

{% highlight bash %}
mkdir -p openvas/debs
cd openvas/

apt-get source libopenvas8
apt-get source openvas-cli 
apt-get source openvas-manager
apt-get source openvas-scanner
apt-get source openvas
{% endhighlight %} 

Each of does packages have dependencies on their own, we can check does by executing:  

{% highlight bash %}
dpkg-checkbuilddeps
{% endhighlight %} 

The result is a list of dependencies that we need to install before we build any of the packages.  
- openvas-libraries-8.0.7  

{% highlight bash %}
apt-get install bison cmake debhelper doxygen libgcrypt-dev libglib2.0-dev libgnutls28-dev \
libgpgme11-dev libldap2-dev libpcap-dev uuid-dev libssh-dev libksba-dev libhiredis-dev \
libsnmp-dev libfreeradius-client-dev
{% endhighlight %}  

For the openvas-manager-6.0.8 the dependencies are as follows.  

{% highlight bash %}
apt-get install dh-systemd libsqlite3-dev xmltoman flawfinder
{% endhighlight %}  

The package needs some changes before we building the libopenvas8.

{% highlight bash %}
cd libopenvas8
dpkg-buildpackage
mv ../*.deb debs/
dpk -i ../debs/*.deb
{% endhighlight %} 

Next is openvas-manager, we will have to to change the build rules at **debian/rules** and  
**-DBACKEND=POSTGRESQL**.  

{% highlight bash %}
cd openvas-manager-6.0.8/debian/
vim rules
# from
dh_auto_configure -- -DCMAKE_INSTALL_PREFIX=/usr -DSYSCONFDIR=/etc \
-DLOCALSTATEDIR=/var -DCMAKE_BUILD_TYPE=release  
# to
dh_auto_configure -- -DBACKEND=POSTGRESQL -DCMAKE_INSTALL_PREFIX=/usr \
 -DSYSCONFDIR=/etc -DLOCALSTATEDIR=/var -DCMAKE_BUILD_TYPE=release 
 cd ..
{% endhighlight %} 


A minor detail about this package is an error that showed up about a missing library after a while i found out that it wasn't included in the **.deb** file so we need to add this lines to make shore its included.  

{% highlight bash %}
echo "var/lib/openvas/openvasmd/pg" >> debian/openvas-manager.dirs
echo "var/lib/openvas/openvasmd/pg" >> debian/openvas-manager.install
# change service config
sed -i -- 's/\/var\/lib\/openvas\/mgr\/tasks.db/tasks/g' debian/openvas-manager.service
# change init.d default config
sed -i -- 's/\/var\/lib\/openvas\/mgr\/tasks.db/tasks/g' debian/openvas-manager.default
#build package
dpkg-buildpackage
mv ../*.deb ../debs/
dpkg -i ../debs/openvas-manager*.deb
{% endhighlight %} 

After installing **openvas-manager** build **openvas-scanner**, **openvas-cli** and **openvas**, after installing, does packages finishing the installation of any missing runtime dependencies like **redis-server** with.  

{% highlight bash %}
apt-get -f install
{% endhighlight %} 

After is the same as regular package. 

{% highlight bash %}
openvas-setup
{% endhighlight %} 

