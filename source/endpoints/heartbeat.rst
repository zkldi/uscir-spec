Heartbeat - GET /
==================================

Used to check your connection to the server, and receive some basic information.

################
Expected Request
################

No data is expected.

#################
Expected Response
#################

The server should respond with 20, and some basic information in the ``body``:

.. list-table:: Body
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``serverTime``
        - Integer (Unix Seconds)
        - The current time according to the server.
    *   - ``serverName``
        - String
        - The name of the server. This may be displayed to the user.
    *   - ``irVersion``
        - String
        - The version of this spec implemented by the IR.
