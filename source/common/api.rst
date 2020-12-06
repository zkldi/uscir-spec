Common API Information
==================================

=================
Architecture
=================

USC-IR operates over HTTP or HTTPS. The USC client will make requests to endpoints at given times,
such as POSTing ``/score/submit`` when a score is achieved.

USC-IR only makes use of GET and POST, to simplify implementation.

USC-IR is intended to operate **ontop** of HTTP(S), to make the results of requests more obvious.
That is to say, USC will look at the HTTP status code to determine if the request hit the server or not.

As much as this sounds like a massive code smell (I wasn't initially fond of it either!), it works out very neatly implementation wise,
allowing us to very easily differentiate between errored requests to a down/malfunctioning server, and errored requests due to improper user input.

.. warning::
    As a result, USC-IR expects ``200 OK``, for **ANY** response that successfully reaches the server, regardless of how the server interpreted the request.

=================
Request Format
=================

All data sent to the server is expected to be in JSON format, with ``Content-Type: application/json`` set in request headers.

To authenticate with the server, users are expected to send an Authorization header, containing ``Bearer <token>``.

How the server distributes these tokens is up to them, but they are to be used to authenticate who is making what request.

Other data requested depends on each individual endpoint.

=================
Response Format
=================

All data returned from the server is in JSON format.

.. note::
    As noted above, if the server returns anything other than ``200 OK`` , the response body should not be parsed or consumed.

Given that we use HTTP status codes to determine whether our *request* succeeded, USC-IR implements two digit integer ``statusCode`` s to indicate the status of a request.

==============  =======
``statusCode``  Meaning
==============  =======
20              Request successful!
40              Bad/Malformed Request.
41              Unauthorised.
42              Server Refusing to accept score for this chart.
43              Requesting user is banned.
50              Internal Server Error.
==============  =======

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
     - "string"
     - A user-readable description of the server response. Doubles as a human readable error message.

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

