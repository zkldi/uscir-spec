Replay Submission - POST /replays
==================================

Used to submit the replay for a given score when requested by the server.

################
Expected Request
################

.. note::

    This endpoint is expected to receive data with a Content-Type of ``multipart/form-data``, as a result of the fact that it sends a file.
    Regardless, the server is expected to **respond** with a Content-Type of ``application/json``.

The data will contain ``identifier``, which is the identifier sent by the server under ``sendReplay`` after score submission.
It will also contain the replay file under the key ``replay``.

#################
Expected Response
#################

| If the identifier does not correspond to one of the requesting player's scores, the server should respond with ``statusCode 44``.
| Otherwise, if the identified score already has a replay, the server should respond with ``statusCode 40``.
| Otherwise, the server can respond with ``statusCode 20``, and the regular format thereof. No particular data is required in the body response.
