DeskMetrics for .NET and Mono - Complete API reference
=======================================================

You'd probably introduced to DeskMetrics through the :doc:`three minute tutorial<Java_3min>`, which cover basic things like downloading our assembly, adding it CLASSPATH and making a very simple example using the DeskMetrics platform. If you didn't read it, we recommend reading it before reading the complete documentation.

Basic information about DeskMetrics API
----------------------------------------

The DeskMetrics Java API is, for short, a wrapper for the DeskMetrics web service API. It basic function is take the data collected, serialize it into a JSON string and send to DeskMetrics server.

There are few more features, like caching and background data sending (through a separate thread); If you are curious about our implementation, `check our C# API code at Github <http://github.com/deskmetrics/jDeskMetrics>`_ 
