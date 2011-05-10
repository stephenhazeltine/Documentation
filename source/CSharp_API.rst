DeskMetrics for .NET and Mono - Complete API reference
=======================================================

You'd probably introduced to DeskMetrics through the :doc:`three minute tutorial<CSharp_3min>`, which cover basic things like downloading our assembly, adding it as a reference and making a very simple example using the DeskMetrics platform. If you didn't read it, we recommend reading it before reading the complete documentation.

Basic information about DeskMetrics API
----------------------------------------

The DeskMetrics dotNet API is, for short, a wrapper for the DeskMetrics web service API. It basic function is take the data collected, serialize it into a JSON string and send to DeskMetrics server.

There are few more features, like caching and background data sending (through a separate thread); If you are curious about our implementation, `check our C# API code at Github <http://github.com/deskmetrics/DeskMetrics.NET>`_ 

The DeskMetrics.Watcher class 
------------------------------

This is the basic DeskMetrics API class. Its basic usage is like this:


.. code-block:: csharp

    using DeskMetrics;

    //...

    Watcher watcher = new Watcher();

    //as the application starts:

    watcher.Start("your_app_id","1.0");

    //as your application ends:

    watcher.Stop();

This is the two basic methods of Watcher object.

.. note::

    You can get your application id at http://analytics.deskmetrics.com/


The Watcher object has the following methods:

    
.. cpp:function:: void Start(string AppID, string AppVersion)

*Required function*. Initializes the application by collecting some environment characteristics like operating system, Java and .NET version, processor, RAM usage and totals.

.. cpp:function:: void Stop()

*Required function*. Must be called right before the application closes. 

.. cpp:function:: void TrackEvent(string Category, string Name)

Simplest event tracking. You can use it on button clicks, form loads or another user-triggered event. 

.. cpp:function:: void TrackEventValue(string Category, string Name, string Value)

Tracks events with the extra "Value" option.

.. cpp:function:: void TrackCustomData(string CustomDataName, string CustomDataValue)

Tracks custom data, like an specific user input. It can be useful when you want to get custom data without using TrackLog or TrackEventValue.


.. cpp:function:: void TrackCustomDataR(string CustomDataName, string CustomDataValue)

Tracks custom data in real time, without waiting for the scheduler to send it later. It throws an exception if the server communication can't be established (e.g. no internet access)

.. cpp:function:: void TrackCachedCustomDataR(string CustomDataName, string CustomDataValue)

Just like TrackCustomDataR, but it stores the custom data if it can't be sent in real time.

.. cpp:function:: void TrackEventPeriod(string EventCategory, string EventName, int EventTime, bool Completed)

This method can be used to track time-consuming operations (like a form-fill by a user or a disk de fragmentation by some utility tool).

The EventTime parameter is the time spent on the event and the Completed parameter is used to inform to DeskMetrics if the event was completed successfully (false if it was cancelled/aborted and true, otherwise).  

.. cpp:function:: void TrackLog(string Message)

Simple log tracking.

.. cpp:function:: void TrackException(Exception ApplicationException)

Tracks an exception with its attributes (stack trace, source, target site and message).

.. cpp:member:: Services Services

Services object. Useful for some proxy and timeout configurations. See its documentation below.

The Deskmetrics.Service class
------------------------------

Responsible for the DeskMetrics API communication layer. It has some public attributes which can be used to configure some aspects of network communication between the DeskMetrics' user and server.

.. cpp:member:: string ProxyHost

Default: null. Specify a proxy host or IP. 

.. cpp:member:: string ProxyUserName

Default: null. Specify a proxy user name, if the proxy has authentication.

.. cpp:member:: string ProxyPassword

Default: null. Specify a password for accessing the proxy.

.. cpp:member:: int ProxyPort 

Default: null. Specify the proxy port.

.. cpp:member:: int PostPort

Default: 80. Specify which port will be used to send data to Deskmetrics' server. It supports two values: 80 (http) and 443 (https);

.. cpp:member:: int PostTimeOut

Default: 25000. Timeout value in milliseconds.
