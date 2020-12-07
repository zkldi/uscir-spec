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

| If this chart is not, and will not be tracked, the server will respond with 42.
| Otherwise, the server will respond with 20, and the following information in the body:

.. list-table:: Body
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``record``
        - Server Score Object
        - The current server record. In the event of this chart being tracked but having no scores, this may be null.

.. note::
    Please see the Score Submission endpoint page for documentation on Server Score structure.
