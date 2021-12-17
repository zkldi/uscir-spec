IR Globals
============

This page documents variables and functions added to the global scope which are accessible in every script.

IRData
##################

IRData contains values that are relevant to skins intending to make use of the IR.

States
*******

The following constants are accessible under the ``IRData.States`` table, which correspond to the USC-IR status codes for use in skins.
There are also three extended codes, which are not sent by the server but are instead used by USC. The meaning is expressed below.

============== =====
Name           Value
============== =====
Unused         0
Pending        10
Success        20
Accepted       22
BadRequest     40
Unauthorized   41
ChartRefused   42
Forbidden      43
NotFound       44
ServerError    50
RequestFailure 60
============== =====

* Unused: IR is not being used by the client (no base URL has been specified, etc.)
* Pending: Request has not yet received a response.
* RequestFailure: The request failed for a generic reason (non-200 HTTP code, malformed response, etc.)


Active
*******

The value of ``IRData.Active`` is ``true`` if an IR URL has been set in the config. Otherwise, it is false.

Request Functions
#################

The below functions are accessible under the ``IR`` table. They are used to make requests of the IR.

All of these functions are asynchronous and take a callback. This callback is called with the exact JSON returned by the server, as a Lua table. If the request fails, the table will have ``statusCode 60`` and a generic description.

Here is an example usage:

.. code-block:: lua

    function heartbeatResponse(res)
        if res.statusCode == IRState.Success then
            game.Log(string.format("Connected to %s", res.body.serverName), game.LOGGER_INFO)
        else
            game.Log("Can't connect to IR!", game.LOGGER_WARNING)
        end
    end

    IR.Heartbeat(heartbeatResponse)


Heartbeat(callback)
*******************

Performs a Heartbeat request.

ChartTracked(hash, callback)
****************************

Performs a Chart Tracked request for the chart with the provided hash.

Record(hash, callback)
*********************

Performs a Record request for the chart with the provided hash.

Leaderboard(hash, mode, n, callback)
*********************

Performs a Leaderboard request for the chart with the provided hash, with parameters mode and n.
