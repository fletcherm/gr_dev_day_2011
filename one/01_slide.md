!SLIDE 
# JRuby, Rails, and Torquebox #
## GR DevDay November 2011 ##
## [github.com/fletcherm/gr\_dev\_day\_2011](https://github.com/fletcherm/gr_dev_day_2011) ##

!SLIDE center
# Matt Fletcher #
# [Atomic Object](http://www.atomicobject.com) #
### big thanks to Micah Alles ###
![AO](AO-symbol-color.png)

!SLIDE center
# Brief intro to JRuby #
![JRuby](jruby.png)

!SLIDE
# Ruby interpreter on the JVM #

!SLIDE bullets
# Upsides #
* fifteen+ years of Java libraries
* deploy almost anywhere
* easy distribution

!SLIDE
# Demo! #

!SLIDE center
## DOS simulators => Java + JRuby simulators ##
![AGI](agi_logo.png)

!SLIDE
# Techs #

!SLIDE
# Java #

!SLIDE
# JRuby #
## started with version 1.0 (2007) ##

!SLIDE
# Write Ruby #
    @@@ Ruby
    class ConfigurationFacade
      def method_missing(name, *args, &block)
        @config[name.to_s]
      end

      def capital_investment
        @config["capital_investment"].present?
          ? @config["capital_investment"]
          : 0
      end
    end

!SLIDE
# Interface with Java #
    @@@ Ruby
    class AgingReportView
      def view
        @table_model = ReadOnlyTableModel.new
        @aging = JTable.new(@table_model)
        @aging.row_selection_allowed = false
        @aging.cell_selection_enabled = true
      end
    end

!SLIDE
# Cross-pollinate for maximum goodness #
    @@@ Ruby
    def invoke_later(&block)
      if javax.swing.SwingUtilities.
                     is_event_dispatch_thread
        block.call
      else
        ...
      end
    end

!SLIDE
    @@@ Ruby
    SwingUtilities.invoke_later(Runnable.impl do
      begin
        block.call
      rescue Exception => ex
        puts ex.message
        puts ex.backtrace.join("\n")
      end
    end)

!SLIDE
    @@@ Ruby
    def add_row(row)
      invoke_later do
        @table_model.add_row row.to_java
      end
    end

    def clear_rows
      invoke_later do
        @table_model.clear
      end
    end

!SLIDE
# RSpec #

!SLIDE
# Substance #

!SLIDE
# Apache Batik #

!SLIDE
# Java scenegraph #

!SLIDE
# JFreeChart #

!SLIDE
# Beta distribution #

!SLIDE
# Bouncy Castle #

!SLIDE
# YAML #

!SLIDE
# NSIS - Windows installer #

!SLIDE
# Launch4J - Windows Java app launcher #

!SLIDE
# .app bundle on Mac #

!SLIDE
# shell script on Linux #

!SLIDE
# Rails and Torquebox #

!SLIDE bullets
# Why? #
* customers with sensitive info - no outside hosting
* self-contained app - minimize dependences
* JRuby + Rails + Torquebox a great fit

!SLIDE
# (skipping Rails introduction) #

!SLIDE center
# Torquebox #
![Torquebox](torquebox.png)
credit Torquebox website

!SLIDE bullets
# features #
* easy deploy
* web server
* scheduled jobs
* synchronous or asynchronous tasks
* messaging
* caching

!SLIDE bullets
# easy deploy #
* rolls your app into a knob
* to be deployed with the rest of Torquebox

!SLIDE
# web server #
## JBoss Application Server ##

!SLIDE bullets
# jobs #
* define a Ruby class with a `run` method
* add to the `jobs` key in `torquebox.yml`
* specify class and frequency. done.

!SLIDE bullets
# tasks #
* define a Ruby class
* mixin `TorqueBox::Messaging::Backgroundable`
* call method `foo.bar` synchronously
* or `foo.background.bar` asynchronously
* futures help you know when it's done

!SLIDE bullets
# messaging #
* define a Ruby class with an `on_message` method
* mixin `TorqueBox::Messaging`
* add to the `messaging` key in `torquebox.yml`
* specify class and message queue name
* publish messages by injecting the appropriate queue

!SLIDE
# caching #
## Infinispan ##
## alternative to memcached ##
## `config.cache_store = :torque_box_store`

!SLIDE bullets
# Torquebox 2.0 #
* Ruby configuration file
* JBoss AS7 - improved performance
* pluggable message encodings (binary, text, json)
* improved transactions

!SLIDE smaller
    @@@ Ruby
    class Processor < TorqueBox::Messaging::MessageProcessor
      always_background :send_thanks_email

      def on_message(msg)
        thing = Thing.create(:name => msg)
        inject('/queues/post-process').publish(thing.id)
        send_thanks_email(thing)
        # raise "rollback everything"
      end
    end

### credit torquebox website ###

!SLIDE
    @@@ Ruby
    User.transaction do
      User.create(:username => 'Kotori')
      User.transaction do
        User.create(:username => 'Nemu')
        raise ActiveRecord::Rollback
      end
    end

### credit torquebox website ###

!SLIDE
    @@@ Ruby
    TorqueBox.transaction do
      User.transaction do
        User.create(:username => 'Kotori')
        User.transaction do
          User.create(:username => 'Nemu')
          raise ActiveRecord::Rollback
        end
      end
    end

### credit torquebox website ###

!SLIDE
# Building a Rails app with Torquebox and JRuby #

!SLIDE
# the recipe #

!SLIDE bullets
# ingredients #
* Torquebox - hold the JRuby
* your preferred version of JRuby
* your gems
* your app
* support scripts

!SLIDE
# stirring the pot #

!SLIDE bullets
# prereq software #
* download and cache Torquebox
* download and cache your JRuby version

!SLIDE
# unpack Torquebox and remove its JRuby #

!SLIDE
# unpack your JRuby #

!SLIDE
# use bundler to install your gems into this JRuby #

!SLIDE
# `>> rake torquebox:archive` #
# `=> knob` #

!SLIDE
# copy knob and JRuby into Torquebox #

!SLIDE
# copy in support scripts (startup, shutdown, etc) #

!SLIDE
# tar it up #

!SLIDE
# consume #

!SLIDE
# scp archive to test machine with only Java and database #

!SLIDE
# unpack archive and launch app in production mode #

!SLIDE
# run acceptance tests against the running server #

!SLIDE
# passed? publish the artifact and mark it green #

!SLIDE
# ups and downs of these technologies #

!SLIDE
## (see previous slides for goodies) ##

!SLIDE
# used MongoDB + Mongoid with no issue or extra work required #

!SLIDE
# many other rubygems with no issue #

!SLIDE
# things that sucked #

!SLIDE bullets
# startup time #

!SLIDE
# `export JAVA_OPTS="-d32"` #

!SLIDE
# JRuby + Rails loading problem or JVM problem? #

!SLIDE bullets
# Ruby 1.9 support #
* it was mostly there, but not quite enough (January 2011)
* hard to debug errors 
* might be there now (November 2011)

!SLIDE
# Torquebox slow to start #
## usually only an issue once a day ##
## somewhat slow to reload in dev mode ##
## blazing fast reloads in production mode ##

!SLIDE
# battling the environment #

!SLIDE center
# Thanks! #
# Matt Fletcher #
## [github.com/fletcherm/gr\_dev\_day\_2011](https://github.com/fletcherm/gr_dev_day_2011) ##
![AO](AO-symbol-color.png)
