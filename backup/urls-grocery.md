---
layout: post
title: 7. tools installation & urls grocery
date: 2016-01-14
categories: sugarcrm
tags: [php, composer]
description: To understand python class and metaclass
---


# [node.js](https://nodejs.org)

    sudo apt-get update
    sudo apt-get install nodejs
    sudo apt-get install npm
    ln -s /usr/bin/nodejs /usr/bin/node
    #OR install legacy version 
    #sudo apt-get install nodejs-legacy
    #


> **setup nodejs env variables for node_module lib**
  npm install underscore -g
  **edit /etc/profile** 
  export IBM_DB_HOME=/opt/ibm/db2/V10.5
  export JAVA_HOME=/usr/lib/jvm/java
  export JRE_HOME=${JAVA_HOME}/jre
  export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
  export PATH=/usr/local/node/bin:${JAVA_HOME}/bin:$PATH
  export NODE_PATH=/usr/local/node:/usr/local/lib/node_modules
  **source /etc/profile**
  
  
  
    sudo rm /usr/bin/ruby
    sudo rm /usr/bin/gem
    sudo ln -s /usr/bin/ruby2.0 /usr/bin/ruby
    sudo ln -s /usr/bin/gem2.0 /usr/bin/gem
  
      C:\RubyDevKit>gem install jekyll
    ERROR: Could not find a valid gem 'jekyll' (>= 0), here is why:
    Unable to download data from https://rubygems.org/ - SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed (https://api.rubygems.org/latest_specs.4.8.gz)
        
    sudo apt-get install ruby ruby-dev make
    sudo gem install jekyll --no-rdoc --no-ri
    sudo apt-get install nodejs  
  
  