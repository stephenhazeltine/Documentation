DeskMetrics for .NET and Mono - Complete API reference
=======================================================

You'd probably introduced to DeskMetrics through the :doc:`three minute tutorial<Java_3min>`, which cover basic things like downloading our assembly, adding it CLASSPATH and making a very simple example using the DeskMetrics platform. If you didn't read it, we recommend reading it before reading the complete documentation.

Basic information about DeskMetrics API
----------------------------------------

The DeskMetrics Java API is, for short, a wrapper for the DeskMetrics web service API. It basic function is take the data collected, serialize it into a JSON string and send to DeskMetrics server.

There are few more features, like caching and background data sending (through a separate thread); If you are curious about our implementation, `check our Java API code at Github <http://github.com/deskmetrics/jDeskMetrics>`_ 


The com.DeskMetrics.DeskMetrics class
--------------------------------------


It is a `singleton <http://en.wikipedia.org/wiki/Singleton_pattern>`_. For short, you can use it this way:


.. code-block:: java 

    import com.DeskMetrics.DeskMetrics;

    //gets the instance
    DeskMetrics tracker = DeskMetrics.getInstance();

    //initializes the application. you *need* to do it.

    tracker.start("your_app_id_here", "1.0");

    // call this before the application finishes. you *need* to do it

    tracker.stop();

.. note::
    
    You can get your application id at http://analytics.deskmetrics.com/


The complete com.DeskMetrics.DeskMetrics class documentation is shown below:

.. cpp:function:: void start(String appID, String version)

You *must* call this method. It is responsible for get environment information, like processor, operating system  and Java version.


.. cpp:function:: void stop()

You *must* call this method right before you application ends. It is responsible to send the data to DeskMetrics' servers. 


.. cpp:function:: void trackEvent(String category, String name)

Simple event tracking

.. cpp:function:: void trackEventValue(String category, String name, String value)

Simple event tracking with an extra ("value") option enabled.

.. cpp:function:: void trackCustomData(String name, String value)

Tracks an customized data (useful for emails, telephone, extra system configuration, etc)

.. cpp:function:: void trackCustomDataR(String name, String value)

Does the exactly same thing as trackCustomData, but sends the data in real time.


.. cpp:function:: void trackLog(String message)

Simple log utility.

.. cpp:function:: void trackEventTimed(String category, String name, int time, boolean finished)

Tracks events related to time and intervals. It is useful for long-time operations (like disk defrag, big file download, and so on). Time must be specified in seconds.

.. cpp:function:: void trackException(Exception e)

Tracks an exception and its details, like stack trace and message.


