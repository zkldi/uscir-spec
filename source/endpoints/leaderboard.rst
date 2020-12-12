Leaderboard - GET /charts/:chartHash/leaderboard
==================================

Used to retrieve some particular useful subset of the scores from the server.

################
Expected Request
################

The request is expected to include query parameters ``mode`` and ``n``, where n is the limit of scores requested and ``mode`` is one of the following:

============== =======
mode           Meaning
============== =======
best           Return the top n personal bests for this chart.
rivals         Return the top n personal bests by players designated as this player's rivals. (server implementation dependent)
============== =======

#################
Expected Response
#################

| If the server refuses to track this chart, e.g. because it is blacklisted, it should respond with ``statusCode 42``
| Otherwise, if the server does not know of this chart, it should respond with ``statusCode 44``.
| Otherwise, the server will respond with 20, and the following information in the body:

.. list-table:: Body
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``scores``
        - Array<Server Score Object>
        - The requested scores, sorted in descending order by their ``score`` field.
