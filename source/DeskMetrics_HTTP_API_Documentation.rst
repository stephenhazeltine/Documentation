DeskMetrics HTTP API Documentation
===================================

The DeskMetrics HTTP API allow you to send data directly to DeskMetrics' servers and all languages API we release is obviously a layer to this webservice.

In this document, we present our HTTP API characteristics and we show you how to build your DeskMetrics API for your language/platform.


HTTP Parameters
---------------

URL
^^^


The URL is formed using this pattern:

    http://application_id.api.deskmetrics.com/sendData/

It **only** accepts HTTP POST requests. You need to change the `application_id` with your real and valid application ID.


.. note::
    You can get your application ID at DeskMetrics' Dashboard at http://analytics.deskmetrics.com/


SSL Support
^^^^^^^^^^^

DeskMetrics HTTP API also supports SSL. So, you can modify the previous URL to the following one:


    https://application_id.api.deskmetrics.com/sendData/

All services runs at default port. TCP 80 for http and TCP 443 for https.


Server responses
-----------------

For every request, DeskMetrics' HTTP API return a JSON string with the operation status code in the following format:

.. code-block:: javascript
    
    {"status_code":error_code}


Where `error_code` can be any of the following:

=========== ==================================
Error code   Mean
=========== ==================================
0            OK
1            OK
-8           Empty POST data
-9           Invalid JSON string
-10          Missing required JSON data
-11          AppID not found
-12          UserID not found
-13          Use POST request
-14          Application version not found
=========== ==================================


DeskMetrics JSON reference
----------------------------

A bit about syntax
^^^^^^^^^^^^^^^^^^^

All data sent to DeskMetrics HTTP API must be sent in JSON format and **must** follow the JSON.org syntax. If you use a JSON library, it should have no problems. But some developers want to generate JSON from scratch and there is somethings that they should know:

1. All JSON object names bust be enclosed in double quotes (").
   Example:

.. code-block:: javascript
        
    {my_key:"my_data"} // this is *invalid*
    {'my_key':"my_data"} // this is *invalid*
    {"my_key":"my_data"} // this is ok

2. All JSON strings must be enclosed into double quotes(")
   Example:

.. code-block:: javascript

    {"my_key":'my_data'} // this is *invalid*
    {"my_key":"my_data"} // this is valid

The StartAPP JSON
^^^^^^^^^^^^^^^^^

The StartAPP JSON contains various kinds of environment information, like operating system, processors, RAM usage and more. It is a **must have** JSON, you must send it in order to get all things working on DeskMetrics DashBoard.

You should send this JSON when the application starts. 

======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("strApp")
aver     string    Application version (like "1.0")
ID       string    Unique user ID [1]_
ss       string    Session ID     [2]_
ts       integer   Timestamp (GMT 0)
osv      string    Operating System Name/Version
ossp     integer   Operating System Service Pack
osar     integer   Operating System Architecture (32 or 64 bits)
osjv     string    Java Version
osnet    string    .NET/Mono Framework version
osnsp    integer   .NET/Mono Framework Service Pack
oslng    integer   Language ID [3]_
osscn    string    Screen resolution (width x height)
cnm      string    Processor name (e.g Core i7, Pentium IV, etc)
cbr      string    Processor brand (e.g. GenuineIntel, AMD)
cfr      integer   Processor frequency (in hertz)
ccr      integer   Number of processor cores
car      integer   Processor architecture 
mtt      integer   Memory total (bytes)
mfr      integer   Memory free (bytes)
dtt      integer   Disk total (bytes)
dfr      integer   Disk free (bytes)
======= ========= ==================================================================

Example:

.. code-block:: javascript

    {
    "tp": "strApp",
    "aver": "4.3",
    "ID" : "F83959C860F744BEBC9A5D97CCFECF1A",
    "ss": "68D89010D01E4E13B1BAAE37343144EA",
    "ts" : 1290607420,
    "osv": "Windows XP",
    "ossp": 3,
    "osar": 32,
    "osjv": "1.6",
    "osnet": "null",
    "osnsp": null,
    "oslng": 1046,
    "osscn": "1024x768",
    "cnm": "Intel Pentium 4 CPU 3.00GHz",
    "cbr": "Intel",
    "cfr": 2999,
    "ccr": 1,
    "car": 64,
    "mtt": 468103168,
    "mfr": 247398400,
    "dtt": 52427898880,
    "dfr": 43494805504
    }

The StopApp JSON
^^^^^^^^^^^^^^^^^

This is JSON sent by you application when it finishes. For performance reasons, you may send it with all other generated JSON strings together, using only one request to send all gathered data to DeskMetrics.


======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("stApp")
ts       integer   Timestamp (GMT 0)
ss       string    Session GUID [2]_ (same ID from Start App)
======= ========= ==================================================================

Example:

.. code-block:: javascript

    {
    "tp": "stApp",
    "ts" : 1290607434,
    "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD"
    }


The Event  JSON
^^^^^^^^^^^^^^^^^

This is the simplest event tracking method provided by DeskMetricsk. You may send it with Stop App JSON, when your application finishes.


======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("ev")
ts       integer   Timestamp (GMT 0)
ss       string    Session GUID [2]_ (same ID from Start App)
ca       string    Event category name
nm       string    Event name
fl       string    Flow identifier. [4]_ 
======= ========= ==================================================================

Example:

.. code-block:: javascript

    {
    "tp": "ev",
    "ca": "Event Category",
    "nm": "Event Name",
    "ts": 1280249019,
    "ss": "BD62D22731AB43BC8BB7B5A7C9E3E405",
    "fl": 1
    }


The Event Value  JSON
^^^^^^^^^^^^^^^^^^^^^^

Same thing as simple event, with an extra field for a value.

======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("evV")
ts       integer   Timestamp (GMT 0)
ss       string    Session GUID [2]_ (same ID from Start App)
ca       string    Event category name
nm       string    Event name
vl       string    Event Value
fl       string    Flow identifier. [4]_ 
======= ========= ==================================================================

Example:

.. code-block:: javascript

    {
    "tp": "evV",
    "ca": "Event Category",
    "nm": "Event Name",
    "vl": "Event Value",
    "ts": 1280249020,
    "ss": "BD62D22731AB43BC8BB7B5A7C9E3E405",
    "fl": 3
    }

    
The Event Period JSON
^^^^^^^^^^^^^^^^^^^^^^

Useful for operations duration measurements (like how much time a user spend to fill a form).

======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("evP")
ts       integer   Timestamp (GMT 0)
ss       string    Session GUID [2]_ (same ID from Start App)
ca       string    Event category name
nm       string    Event name
fl       string    Flow identifier. [4]_ 
tm       int       Event duration (in seconds)
ec       bool      Event completed? (0 for no, 1 for yes)
======= ========= ==================================================================


Example:

.. code-block:: javascript

    {
    "tp": "evP",
    "ca": "Event Category",
    "nm": "Event Name",
    "ts": 1280249020,
    "ss": "BD62D22731AB43BC8BB7B5A7C9E3E405",
    "fl": 3,
    "tm": 120.
    "ec": 1
    }

The Log JSON
^^^^^^^^^^^^^^^^^^^^^^

Simple logging utility. 

======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("lg")
ts       integer   Timestamp (GMT 0)
ss       string    Session GUID [2]_ (same ID from Start App)
ms       string    Log message
fl       string    Flow identifier. [4]_ 
======= ========= ==================================================================

.. code-block:: javascript

    {
    "tp": "lg",
    "ts": 1280249020,
    "ss": "BD62D22731AB43BC8BB7B5A7C9E3E405",
    "ms": "Log message",
    "fl": 3,
    }

The  Custom Data JSON
^^^^^^^^^^^^^^^^^^^^^^

You can send anything with this feature, not necessarily using a event. This is useful for tracking data like user personal data (like email and telephone), extra environment characteristics (like GPU model). 

======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("ctD")
nm       string    Custom Data Name
vl       string    Custom Data Value
ts       integer   Timestamp (GMT 0)
ss       string    Session GUID [2]_ (same ID from Start App)
fl       string    Flow identifier. [4]_ 
======= ========= ==================================================================

.. code-block:: javascript

    {
    "tp": "ctD",
    "nm": "Custom Data Name",
    "vl": "Custom Data Value",
    "ts": 1280249020,
    "ss": "BD62D22731AB43BC8BB7B5A7C9E3E405",
    "fl": 3,
    }

The Custom Data JSON in Real Time
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This feature works like the previous Custom Data, but it can be sent in real time and without the need of StartApp JSON.

======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("ctDR")
nm       string    Custom Data Name
vl       string    Custom Data Value
ts       integer   Timestamp (GMT 0)
ss       string    Session GUID [2]_ (same ID from Start App, if exists)
fl       string    Flow identifier. [4]_ 
aver     string    Application version
ID       string    User Unique ID [1]_
======= ========= ==================================================================

.. code-block:: javascript

    {
    "tp": "ctDR",
    "nm": "CustomDataR Name",
    "vl": "CustomDataR Value",
    "aver": "4.2",
    "ID": "AA62D22731AB43BC8BB7B5A7C9E3E404",
    "ts": 1280249020,
    "ss": "BD62D22731AB43BC8BB7B5A7C9E3E405",
    "fl": 7
    }


The Install JSON
^^^^^^^^^^^^^^^^^

This JSON must be sent when your application are being installed.


======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("ist")
ts       integer   Timestamp (GMT 0)
ss       string    Session GUID [2]_ (same ID from Start App, if exists)
fl       string    Flow identifier. [4]_ 
aver     string    Application version
======= ========= ==================================================================

.. code-block:: javascript
   
    {
    "tp": "ist",
    "aver": "4.2",
    "ID": "AA62D22731AB43BC8BB7B5A7C9E3E404",
    "ts": 1280249020,
    "ss": "BD62D22731AB43BC8BB7B5A7C9E3E405"
    }

The Uninstall JSON
^^^^^^^^^^^^^^^^^^^

This JSON must be sent when your application are being installed.


======= ========= ==================================================================
Field   Type       Description
======= ========= ==================================================================
tp       string    JSON Type ("ust")
ts       integer   Timestamp (GMT 0)
ss       string    Session GUID [2]_ (same ID from Start App, if exists)
fl       string    Flow identifier. [4]_ 
aver     string    Application version
======= ========= ==================================================================


.. code-block:: javascript
   
    {
    "tp": "ist",
    "aver": "4.2",
    "ID": "AA62D22731AB43BC8BB7B5A7C9E3E404",
    "ts": 1280249020,
    "ss": "BD62D22731AB43BC8BB7B5A7C9E3E405"
    }

Full application JSON example
------------------------------

Start Application
^^^^^^^^^^^^^^^^^^

.. code-block:: javascript

    [
        {
        "tp": "strApp",
        "aver": "4.3",
        "ID": "F83959C860F744BEBC9A5D97CCFECF1A",
        "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD",
        "ts" : 1290607420,
        "osv": "Windows XP",
        "ossp": 3,
        "osar": 32,
        "osjv": "1.6",
        "osnet": "null",
        "osnsp": null,
        "oslng": 1046,
        "osscn": "1024x768",
        "cnm": "Intel Pentium 4 CPU 3.00GHz",
        "cbr": "Intel",
        "cfr": 2999,
        "ccr": 1,
        "car": 64,
        "mtt": 468103168,
        "mfr": 247398400,
        "dtt": 52427898880,
        "dfr": 43494805504
        }
    ]

At the end of application execution
------------------------------------

.. code-block:: javascript

    [
        {
        "tp": "ev",
        "ca": "Window",
        "nm": "Main",
        "fl": 1,
        "ts": 1290607303,
        "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD"
        },
        {
        "tp": "ev",
        "ca": "tsIntroducao",
        "nm": "Analisar",
        "fl": 2,
        "ts": 1290607311,
        "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD"
        },
        {
        "tp": "evS",
        "ca": "Analise",
        "nm": "Time",
        "fl": 3,
        "ts": 1290607311,
        "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD"
        },
        {
        "tp": "ev",
        "ca": "Feature",
        "nm": "LimpezaDisco",
        "fl": 4,
        "ts" : 1290607311,
        "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD"
        },
        {
        "tp": "evST",
        "ca": "Analise",
        "nm": "Time",
        "fl": 5,
        "ts": 1290607411,
        "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD"
        },
        {
        "tp": "ctD",
        "nm": "Otimizacao.Disco",
        "vl": "112254062",
        "fl": 6,
        "ts": 1290607433,
        "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD"
        },
        {
        "tp": "ev",
        "ca": "tsAnaliseFinalizada.Botoes",
        "nm": "Sair",
        "fl": 7,
        "ts": 1290607434,
        "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD"
        },
        {
        "tp": "stApp",
        "ts" : 1290607434,
        "ss": "CA5FF05F8D14453CBCF4A7E03DE4FBDD"
        }
    ]


.. [1] -  You should generate a unique user ID at the first session and store it. DeskMetrics uses this infomation to track a specific user behaviour.
.. [2] -  You should generate a unique session ID for every time the user opens your software. 
.. [3] -  Using the LCID (Language and Country ID) table by Microsoft; You can see this list at http://www.science.co.il/language/locale-codes.asp
.. [4] -  It is an autoincrement integer used by almost all JSONs. It should start  with 1 and icrement at every usage. Its main purpose is to generate a software usage graph at DeskMetrics Analytics.
