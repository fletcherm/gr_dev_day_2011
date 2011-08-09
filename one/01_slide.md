!SLIDE 
# JRuby, Rails, and Torquebox #
## DetroitRuby August 2011 ##
## [github.com/fletcherm/detroit\_ruby\_august\_2011](https://github.com/fletcherm/detroit_ruby_august_2011) ##

!SLIDE center
# Matt Fletcher #
# Atomic Object #
### big thanks to Micah Alles ###
![AO](AO-symbol-color.png)

!SLIDE center
# Brief intro to JRuby #
![JRuby](jruby.png)

!SLIDE
## Ruby interpreter on the JVM ##

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
## Write Ruby ##
    @@@ Ruby
    class ConfigurationFacade
      def initialize
        @config = ...
      end

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
## Interface with Java ##
    @@@ Ruby
    class AgingReportView
      def view(parent)
        @table_model = ReadOnlyTableModel.new
        @aging = JTable.new(@table_model)
        @aging.row_selection_allowed = false
        @aging.cell_selection_enabled = true
      end
    end

!SLIDE
## Cross-pollinate for maximum goodness ##
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
# Substance - look and feel #

!SLIDE
# Apache Batik - SVG #

!SLIDE
# Java scenegraph #

!SLIDE
# JFreeChart #

!SLIDE
# Beta distribution #

!SLIDE
# Bouncy Castle - encryption #

!SLIDE
# YAML - configuration #

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
* JRuby + Rails + Torquebox a perfect fit

!SLIDE
# (skipping Rails introduction) #

!SLIDE center
# Torquebox #
![Torquebox](torquebox.png)
credit torquebox website

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
* to be deployed with the rest of torquebox

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
* define a Ruby class with a `on_message` method
* mixin `TorqueBox::Messaging`
* add to the `messaging` key in `torquebox.yml`
* specify class and message queue name
* publish messages by injecting the appropriate queue

!SLIDE
# caching #
## Infinispan ##
## alternative to memcached ##
## `config.cache_store = :torque_box_store`

!SLIDE
# Building a Rails app with Torquebox and JRuby #

!SLIDE
## (see previous slides for goodies) ##

!SLIDE
# things that sucked #

!SLIDE bullets
# startup time #

!SLIDE
# `export JAVA_OPTS="-d32"` #

!SLIDE
# more likely a Rails + JRuby loader problem than JVM problem #

!SLIDE bullets
# Ruby 1.9 support #
* it was mostly there, but not quite enough (January 2011)
* hard to debug errors 
* might be there now (August 2011)

!SLIDE
# Torquebox slow to start #
## usually only an issue once a day ##
## somewhat slow to reload in dev mode ##
## blazing fast reloads in production mode ##

> notes 
!SLIDE
# Recent Rails + Torquebox project #
# used MongoDB + Mongoid with no issue or extra work required #
