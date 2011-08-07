!SLIDE 
# JRuby, Rails, and Torquebox #
## [github.com/fletcherm/detroit\_ruby\_august\_2011](https://github.com/fletcherm/detroit_ruby_august_2011) ##

!SLIDE center
# Matt Fletcher and Micah Alles #
# Atomic Object #
![AO](AO-symbol-color.png)

!SLIDE center
# Brief intro to JRuby #
![JRuby](jruby.png)

!SLIDE
## Ruby interpreter on the JVM ##

!SLIDE bullets incremental
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
# RSpec for testing #

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




> notes 
!SLIDE
# Recent Rails + Torquebox project #
# used MongoDB + Mongoid with no issue or extra work required #
