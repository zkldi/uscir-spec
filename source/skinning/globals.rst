IR Globals
============

The following constants are accessible under the ``ir`` table, which correspond to the USC-IR status codes for use in skins.
There are also three extended codes, which are not sent by the server but are instead used by USC. The meaning is expressed below.

============== =====
Name           Value
============== =====
Unused         0
Pending        10
Success        20
BadRequest     40
Unauthorized   41
ChartRefused   42
Forbidden      43
ServerError    50
RequestFailure 60
============== =====

* Unused: IR is not being used by the client (no base URL has been specified, etc.)
* Pending: Request has not yet received a response.
* RequestFailure: The request failed for a generic reason (non-200 HTTP code, malformed response, etc.)
