DeskMetrics for Adobe Air runtime - Complete API reference
-----------------------------------------------------------

You'd probably introduced to DeskMetrics through the :doc:`three minute tutorial<AdobeAir_3min>`, which cover basic things like downloading our SWC, adding it as a compiler reference and making a very simple example using the DeskMetrics platform. If you didn't read it, we recommend reading it before reading the complete documentation.

Basic information about DeskMetrics API
----------------------------------------

The DeskMetrics Adobe Air API is, for short, a wrapper for the DeskMetrics web service API. It basic function is take the data collected, serialize it into a JSON string and send to DeskMetrics server.

There are few more features, like caching and background data sending (through a separate thread); If you are curious about our implementation, `check our Adobe Air API code at Github <http://github.com/deskmetrics/FlexMetrics>`_ 


The DeskMetricsTracker class
----------------------------

The DeskMetricsTracker class contains all supported tracking features provided by DeskMetrics' platform. 

.. note::

    You don't need a DeskMetricsTracker instance because all methods are static.


The standard methods provided by the library are:

.. js:function:: com.Deskmetrics.DeskMetricsTracker.Start(appid:String, version:String, realtime:Boolean)


*Must be* called when your application starts. 

.. note::

    You can get your application id at http://analytics.deskmetrics.com/

.

.. js:function:: com.Deskmetrics.DeskMetricsTracker.TrackEvent(category:String, name:string)

Events are actionable things that occur, and that you use to organize your data. The events are updated immediately when a user interacts with your software. They are just strings and they can be called anything you want.

Events can be user actions, such as clicking a mouse button or pressing a key, or software occurrences.

.. js:function:: com.Deskmetrics.DeskMetricsTracker.TrackEventValue(category:string, name:string, value:string)

Method used to track any kind of event and attach a custom value.

.. js:function:: com.Deskmetrics.DeskMetricsTracker.TrackLog(message:string)

Utility to track logs.

.. js:function:: com.Deskmetrics.DeskMetricsTracker.TrackCustomData(name:String,value:String)

Sometimes you may want to obtain some specific information from your users or software. You have two ways to do that, by using our log function or the function to send custom data.

The function `TrackCustomData` is useful to send any type of data and generate information and charts. For example, you may want to get some information about your users, like a phone number. You can used the function to get it and see how many of it you are getting each day.

.. js:function:: com.Deskmetrics.DeskMetricsTracker.TrackCustomDataR(name:String,value:String)

Same as `TrackCustomData` but sends the data in real time.

.. js:function:: com.Deskmetrics.DeskMetricsTracker.TrackException(ex:Error)

Exceptions are used to report errors when code is executing. An exception is thrown when a run-time error is generated for various reasons, i.e. mismatched data type error occurs, out-bounds memory problems and so on.

Debug version
-------------

To activate debug, just add Tracker.debug = true;. Your app will alert some messages when an important event or error happens.

