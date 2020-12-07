POST /score/submit
==================================

Sends a score to the server.

################
Expected Request
################

.. note::

    As always, all requests are expected to be JSON, with Content-Type of ``application/json``,

.. list-table:: Root
   :widths: 25 25 50
   :header-rows: 1

   * - Key
     - Type
     - Description
   * - ``chart``
     - Chart Object
     - Contains information about the chart being played. This may be used by the server to accept new charts onto the IR.
   * - ``score``
     - Score Object
     - Contains information about the users' score.


************
Chart Object
************

.. list-table:: Chart Object
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``artist``
        - String
        - The artist who created the song.
    *   - ``title``
        - String
        - The song title.
    *   - ``level``
        - Integer (0,20]
        - The difficulty level assigned to this chart.
    *   - ``difficulty``
        - 0 | 1 | 2 | 3
        - The difficulty of the chart. 0 = NOV, 1 = ADV, 2 = EXH, 3 = INF.
    *   - ``effector``
        - String
        - The effector (charter) for the chart.
    *   - ``illustrator``
        - String
        - The illustrator for the chart jacket.
    *   - ``bpm``
        - String
        - A string representing BPM. For charts with multiple bpms, they are separated by a hyphen, like x.xx-y.yy.

************
Score Object
************

.. list-table:: Score Object
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``score``
        - Integer [0, 10'000'000]
        - The numeric score the user achieved.
    *   - ``chartHash``
        - String
        - The unique identifier for the chart the user played.
    *   - ``gameflags``
        - Integer [0, 63]
        - The flags this score was achieved with. This is a bitwise combination of 6 flags. In smallest-to-largest order, these are Hard Gauge, Mirror, Random, Auto-BT, Auto-Hold, Auto-Laser.
    *   - ``gauge``
        - Float
        - The gauge the user had at the end of the chart. Depending on gameflags, this should be used by the server to determine the clear type on the chart.
    *   - ``timestamp``
        - Integer (unix_seconds)
        - Time in seconds elapsed since Unix Epoch, indicating when this score was achieved.
    *   - ``crit``
        - Integer
        - Hits inside the critical window.
    *   - ``almost``
        - Integer
        - Hits inside the near window.
    *   - ``miss``
        - Integer
        - Missed notes.
    *   - ``windows``
        - Object: {``perfect`` , ``good`` , ``hold`` , ``miss`` }
        - Indicates what the hit windows were for this score. The defaults are; 46, 92, 138 and 250, respectively.

.. warning::
    It is highly advised for servers to reject scores with non-standard ``score.windows`` unless specifically implementing a hard-mode option.

.. warning::
    ``score.timestamp`` is in unix seconds, which is different to the default in languages of the JavaScript family (unix_miliseconds) and the .NET family (Ticks).
    Make sure to account for this if your server expects a different format for time!

################
Response Data
################

Returns the standard API response, with ``body`` as follows:

.. list-table:: Body
    :widths: 25 25 50
    :header-rows: 1

    *   - Key
        - Type
        - Description
    *   - ``score``
        - Server Score Object
        - A Server Score object representing the user's personal best score.
    *   - ``serverRecord``
        - Server Score Object
        - A Server Score object representing the current server record.
    *   - ``adjacentAbove``
        - Array<Server Score Object>
        - An array of 0 to N Server Scores adjacently above the user's PB.
    *   - ``adjacentBelow``
        - Array<Server Score Object>
        - An array of 0 to N Server scores adjacently below the user's PB.
    *   - ``isPB``
        - Boolean
        - True if the score sent in the request is the user's new PB.
    *   - ``isServerRecord``
        - Boolean
        - True if the score sent in the request is the new server record.

.. warning::
    ``body.score`` **always returns the users PB**. It does **NOT** necessarily return the score you sent.

.. warning::
    Several key assumptions are made about the response by the client, which must be upheld by the server. They are as follows:

    * ``adjacentAbove`` will never contain the current server record.
    * The returned scores will always descend in the set [... ``adjacentAbove``, ``score``, ... ``adjacentBelow``]. For clarification, see the note below.
    * An individual user should only have a maximum of one score in the above set. This is because the scores sent should always be personal bests, not any stored score.
    * As a corollary to the above, the requesting user's scores can never appear in the adjacent scores, since their personal best will always be contained in ``score``.

.. note::
    The server may decide on the value of N to use for adjacentAbove/Below. However, there is limited space to display the scores. For maximum compatibility with skins, a value of 2 or 3 is recommended.

.. note::
    The use for ``score.adjacent[Above|Below]`` and ``score.serverRecord`` is illustrated in the table below.

    .. list-table::
        :header-rows: 1

        *   - Element
            - Score
            - Ranking
        *   - ``serverRecord``
            - LV.MINI 10,000,000
            - #1
        *   -
            - ...
            -
        *   - ``adjacentAbove[0]``
            - zkldi 95,753,163
            - #8
        *   - ``adjacentAbove[1]``
            - NEIL.C 94,472,194
            - #9
        *   - ``score``
            - YOU 93,193,547
            - #10
        *   - ``adjacentBelow[0]``
            - POG 92,541,147
            - #11
        *   - ``adjacentBelow[1]``
            - CHAMP 91,260,754
            - #12


*******************
Server Score Object
*******************

The format of scores returned by the server are as follows:

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
        - Integer (unix_seconds)
        - Time in seconds elapsed since Unix Epoch, indicating when this score was achieved.
    *   - ``crit``
        - Integer
        - Hits inside the critical window.
    *   - ``near``
        - Integer
        - Hits inside the near window.
    *   - ``miss``
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