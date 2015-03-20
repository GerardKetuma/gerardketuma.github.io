---
layout: post
title: "Using Bower with Rails 4"
date: 2015-03-19 21:54:26 -0500
comments: true
description: “Setting up a Rails 4 project to utilize Bower in managing front-end resources”
categories: [Rails, Ruby, JavaScript]
image:
  feature: /images/workshop2_web.jpg
share: true
---
Bower is a front-end package manager for the web that makes it easy to manage frameworks, libraries, assets, and utilities. Bower works by fetching and installing packages from all over the web. It finds the packages and downloads them from their sources and helps keep the packages up to date. Bower keeps track of these packages in a manifest file called bower.json.

<!-- more -->

* Bower is a command line utility that is installed with npm.

{% codeblock lang:sh %}
$ npm install -g bower
{% endcodeblock %}

I’m using the -g to install Bower globally so I can continue using it in all of my future projects. Bower requires Node, npm and git.

Now that Bower is installed we can proceed and configure rails to start using Bower, but in this tutorial, I’ll be using the Bower-Rails gem as it makes working with Bower from Rails easier [imho]. It also includes rake tasks for Bower commands and also has a nice Ruby Domain Specific Language (DSL) for specifying which packages you want included, via a Bowerfile.

* Add the Bower-Rails gem to your Gemfile in the root of your Rails application.

{%codeblock lang:ruby Gemfile %}
# Install bower for asset management
gem ‘bower-rails’, ‘0.9.2’
{% endcodeblock %}

* Run bundle to install the gem

{% codeblock lang:sh %}
$ bundle
{% endcodeblock %}

There are two kinds of configurations now for Bower, the standard JSON configuration and Ruby DSL configuration.

For the standard JSON configuration, Bower-rails now supports the standard bower package format out-of-the-box. Simply place your bower.json file the Rails root directory to start. Using the standard format will default all bower components to be installed under the vendor directory.
In Rails the vendor directory is a place for all third-party code.

* Run the following initializer to install dependencies into both lib and vendor directories:

{% codeblock lang:sh %}
$ rails g bower_rails:initialize json
{% endcodeblock %}

This will generate a special bower.json file that combines two standard bower packages into one. Now all you have to do is specify your dependencies under each folder name to install them into the corresponding directories. A config file is also generated in config/initializers/bower_rails.rb for package settings.

Here is an example bower.json file showing the two directories:

{% codeblock lang:js bower.json %}
{
  “lib”: {
    “name”: “bower-rails generated lib assets”,
    “dependencies”: {
      “threex”: “git@github.com:rharriso/threex.git”,
      “gsvpano.js”: “https://github.com/rharriso/GSVPano.js/blob/master/src/GSVPano.js”
    }
  },
  “vendor”: {
    “name”: “bower-rails generated vendor assets”,
    “dependencies: {
      “three.js”: “https://raw.github.com/mrdoob/three.js/master/build/three.js”
    }
  }
}
{% endcodeblock %}


Run the initializer to generate a sample Bowerfile inside the Rails root and a config/initializers/bower_rails.rb config file:

{% codeblock lang:sh %}
$ rails g bower_rails:initialize
{% endcodeblock %}
