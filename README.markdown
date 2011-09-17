OS Packages
===========

    # aptitude install build-essential curl libssl-dev zlib1g-dev \
    libreadline5-dev libxml2-dev libyaml-dev automake


Install Ruby 1.8.7
==================

    # curl -L -O ftp://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.7-p352.tar.bz2
    # tar -xjf ruby-1.8.7-p352.tar.bz2
    # cd ruby-1.8.7-p352
    # autoconf
    # ./configure --prefix=/opt/ruby
    # make
    # make test
    # make install
    # /opt/ruby/bin/ruby --version


Install Rubygems
================

    # curl -L -O http://production.cf.rubygems.org/rubygems/rubygems-1.8.10.tgz
    # tar -xf rubygems-1.8.10.tgz
    # cd rubygems-1.8.10/
    # ruby setup.rb


Installing Facter, Puppet, and Thin
===================================

    # gem install facter puppet ronn thin


Installing Nginx
================

    # curl -L -O ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.13.tar.bz2
    # tar -xf pcre-8.13.tar.bz2 -C /tmp/
    # curl -L -O http://nginx.org/download/nginx-1.0.6.tar.gz
    # tar -xf nginx-1.0.6.tar.gz
    # cd nginx-1.0.6
    # ./configure --prefix=/opt/nginx \
    --with-debug \
    --with-pcre=/tmp/pcre-8.13 \
    --pid-path=/var/run/nginx/nginx.pid \
    --lock-path=/var/lock/nginx \
    --with-http_ssl_module \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log

    # make
    # make install


Test our installation
=====================

    # export PATH="/opt/ruby/bin:/opt/nginx/sbin:$PATH"
    # facter --version
    # puppet --version
    # thin --version
    # nginx -v


Configure Puppet
================

    # puppet master --mkusers
    # killall puppet
    # vim /etc/puppet/puppet.conf
    # cp /opt/ruby/lib/ruby/gems/1.8/gems/puppet-2.7.3/conf/auth.conf /etc/puppet/
    # puppet master
    # puppet agent --test
    # killall puppet


Configuring Thin
================

    # mkdir /etc/thin
    # mkdir /var/run/puppet
    # chown puppet /var/run/puppet
    # vim /etc/thin/puppetmaster.yml
    # mkdir -p /etc/puppet/rack/{logs,tmp}
    # cp /opt/ruby/lib/ruby/gems/1.8/gems/puppet-2.7.3/ext/rack/files/config.ru /etc/puppet/rack/
    # chown puppet:puppet -R /etc/puppet/


Configure Nginx
===============

    # vim /etc/nginx/puppet_proxy
    # vim /etc/nginx/puppet_master.conf
    # vim /etc/nginx/nginx.conf

      include puppetmaster.conf;


Putting it all together
=======================

    # thin -C /etc/thin/puppetmaster.yml start
    # puppet master
    # puppet agent --test