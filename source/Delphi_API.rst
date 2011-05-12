DeskMetrics for Delphi - Complete API reference
==================================================

You'd probably introduced to DeskMetrics through the :doc:`three minute tutorial<Delphi_3min>`, which cover basic things like downloading our library and DLL, adding it as a reference and making a very simple example using the DeskMetrics platform. If you didn't read it, we recommend reading it before reading the complete documentation.


Basic information about DeskMetrics API
----------------------------------------

The DeskMetrics dotNet API is, for short, a wrapper for the DeskMetrics web service API. It basic function is take the data collected, serialize it into a JSON string and send to DeskMetrics server.

There are few more features, like caching and background data sending (through a separate thread); If you are curious about our implementation, `check our Delphi API code at Github <http://github.com/deskmetrics/DelphiMetrics>`_ 


Basic example
--------------

The basic Delphi API usage is like the following:


.. code-block:: csharp

    use DeskMetrics;

    //as the application starts:

    DeskMetricsStart("your_app_id","1.0");

    //as your application ends:

    DeskMetricsStop;


.. function:: function DeskMetricsStart(FApplicationID: PWideChar; FApplicationVersion: PWideChar): Boolean 

*Required function*. Initializes the application by collecting some environment characteristics like operating system, Java and .NET version, processor, RAM usage and totals.

.. function:: function DeskMetricsStop: Boolean

*Required function*. Must be called right before the application finishes. 

.. function:: procedure DeskMetricsTrackEvent(FEventCategory, FEventName: PWideChar)

Simplest event tracking. You can use it on button clicks, form loads or another user-triggered event. 

.. function:: procedure DeskMetricsTrackEventValue(FEventCategory, FEventName, FEventValue: PWideChar)

Tracks events with the extra "Value" option.

.. function:: procedure DeskMetricsTrackCustomData(FName, FValue: PWideChar)

Tracks custom data, like an specific user input. It can be useful when you want to get custom data without using TrackLog or TrackEventValue.


.. function:: function DeskMetricsTrackCustomDataR(FName: PWideChar; FValue: PWideChar): Integer

Tracks custom data in real time, without waiting for the scheduler to send it later. It throws an exception if the server communication can't be established (e.g. no internet access)

.. function:: procedure DeskMetricsTrackEventPeriod(FEventCategory, FEventName: PWideChar; FEventTime: Integer; FEventCompleted: Boolean)

This method can be used to track time-consuming operations (like a form-fill by a user or a disk de fragmentation by some utility tool).

The EventTime parameter is the time spent on the event (in seconds) and the Completed parameter is used to inform to DeskMetrics if the event was completed successfully (false if it was cancelled/aborted and true, otherwise).  

.. function:: procedure DeskMetricsTrackLog(FMessage: PWideChar)

Simple log tracking.

.. function:: procedure DeskMetricsTrackException(FExpectionObject: Exception)

Tracks an exception with its attributes (stack trace, source, target site and message).

.. note::

    You can get your application id at http://analytics.deskmetrics.com/
