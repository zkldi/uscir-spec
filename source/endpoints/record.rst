Record - GET /charts/:chartHash/record
==================================

Used to retrieve the current server record for the chart with the specified hash.

################
Expected Request
################

No data is expected.

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
    *   - ``record``
        - Server Score Object
        - The current server record.
