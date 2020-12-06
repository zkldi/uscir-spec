Common API Information
==================================

=================
Architecture
=================

USC-IR operates over HTTP or HTTPS. The USC client will make requests to endpoints at given times,
such as POSTing ``/score/submit`` when a score is achieved.

USC-IR only makes use of GET and POST, to simplify implementation.

USC-IR is intended to operate **on top** of HTTP(S), to simplify its implementation.
Any request that is received by the server successfully should be responded to with a HTTP 200 OK.
Rejection reasons that are not a failed request should be indicated using a USC-IR status code in the response body, as detailed in Response Format.

.. note::
    Any HTTP response bearing a non-200 status code will be interpreted by the client as a generic failure, and will not be consumed.

=================
Request Format
=================

All data sent to the server is expected to be in JSON format, with ``Content-Type: application/json`` set in request headers.

To authenticate with the server, users are expected to send an Authorization header, containing ``Bearer <token>``.

How the server distributes these tokens is up to them, but they are to be used to authenticate who is making what request.

The body sent is dependent on the endpoint targeted by the request.

=================
Response Format
=================

All data returned from the server is in JSON format.

.. note::
    As noted above, if the server returns a HTTP code other than ``200 OK`` , the response body will not be consumed.

The below table indicates the two-digit USC-IR status codes to be used by the server in response to a request.
Given that we use HTTP status codes to determine whether our *request* succeeded, USC-IR implements two digit integer ``statusCode`` s to indicate the status of a request.

============== ============= =======
``statusCode`` Title         Meaning
============== ============= =======
20             Success       Request succeeded.
40             Bad Request   The request was malformed.
41             Unauthorized  No token, or an invalid token, was provided.
42             Chart Refused The server is not accepting scores for this chart.
43             Forbidden     The token has been banned.
50             Server Error  The server encountered an error while handling the request.
============== ============= =======

###################
Always Present Keys
###################

Returned JSON objects will *always* have these keys.

.. list-table::
   :widths: 25 25 50
   :header-rows: 1

   * - Key
     - Type
     - Description
   * - ``statusCode``
     - 20 | 40 | 41 | 42 | 43 | 50
     - See table above.
   * - ``description``
     - String
     - A human-readable error message, which can be displayed to the user.

###################
Conditional Keys
###################

These are keys that are only present under certain scenarios, such as keys that make no sense under certain ``statusCode`` s.

.. list-table::
   :widths: 25 25 50
   :header-rows: 1

   * - Key
     - Type
     - Description
   * - ``body``
     - Object
     - Only present on ``statusCode 20``. Contains the results of your request (such as score data, chart data, etc.)
