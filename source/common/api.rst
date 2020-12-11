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
     - A human-readable message, which can be displayed to the user.

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
     - Must be present on ``statusCode 20``. Contains the results of your request (such as score data, chart data, etc.)


=======================
Endpoint Commonalities
=======================

All endpoints must obey the following assumptions:

1. All endpoints are authenticated. This means the Authorization header must be provided in the request, and that the server must respond with ``41 Unauthorized`` or ``43 Forbidden`` as and when appropriate.
2. In the case of an endpoint concerning a chart, ``42 Chart Refused`` means explicitly that the server will refuse to store a score for this chart if it were to receive one. It does not just mean that the server has no scores for this chart. The below bullet point list clarifies the reason for this behavior by using the Record endpoint as an example.

    * If the player makes a request for the server record, and receives a 42, the player may reasonably assume that they do not want to play this chart, as their score would not be saved by the IR.
    * If the player makes a request for the server record, and the server has no scores, but would accept this player's score as the new record if they were to play it, the correct response is 20, with a null record object. The client may choose to display this as a greyed out ``00000000``, as SDVX does itself.

======================
Shared Structures
======================

Several structures will be reused multiple times in this specification. Any such structures are detailed below, to avoid repetition.

Server Score
############

This structure is the common format for a 'score' that the server will respond with whenever a score is requested.

.. list-table:: Server Score Object
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``score``
        - Integer [0, 10'000'000]
        - The numeric score the user achieved.
    *   - ``lamp``
        - 0 | 1 | 2 | 3 | 4 | 5
        - The lamp for this score. They correspond to the following; "NO PLAY", "FAILED", "CLEAR", "EXCESSIVE CLEAR", "ULTIMATE CHAIN", "PERFECT ULTIMATE CHAIN".
    *   - ``timestamp``
        - Integer
        - Time in seconds elapsed since Unix Epoch, indicating when this score was achieved.
    *   - ``crit``
        - Integer
        - Hits inside the critical window.
    *   - ``near``
        - Integer
        - Hits inside the near window.
    *   - ``error``
        - Integer
        - Missed notes.
    *   - ``ranking``
        - Integer
        - The ranking for this score, (i.e. #5).
    *   - ``gaugeMod``
        - "NORMAL" | "HARD",
        - The gauge used for this score.
    *   - ``noteMod``
        - "NORMAL" | "MIRROR" | "RANDOM" | "MIR-RAN"
        - The note modifier used for this score.
    *   - ``username``
        - String
        - The username who achieved this score.
