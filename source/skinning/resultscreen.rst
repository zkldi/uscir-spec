Result Screen
=====================

The following fields are added to the ``result`` table on the results screen.

.. code-block:: c#

    string chartHash //the hash of the chart that was just played
    int irState //current state of the IR score submission request (a USC-IR code, including extensions 0/10/60)
    string irDescription //the description in the IR response (nil if irState is 0 or 10)
    ServerScore[] irScores //more details below, nil if irState != 20

.. note::
        This screen is a special case where the request will be automatically performed by the game, rather than being requested in Lua.


ServerScore
***********

irScores is an array of ServerScores, whose structure matches the ServerScore structure detailed in the score submission endpoint page, with these two additions:

.. code-block:: c#

    bool yours //this score belongs to the current player
    bool justSet //this score belongs to the current player, and is the score that was just achieved.

An example usage of these extra values can be found in the default skin: if ``yours`` is true, the score has a gold border. If ``justSet`` is true, the timestamp is 'Now'.

| The array of scores is constructed according to specific rules which make it as easy as possible to display.
| In almost all cases, the scores can simply be displayed in order.
| It is constructed as follows:

* The first element is the server record, unless the server record should be omitted.

    * The server record is omitted if the player's PB is the server record, as in this case ``score`` is the server record.

* The next 0-N elements are the scores in ``adjacentAbove`` from the IR response.
* The next element is ``score``, i.e. the current player's PB. This element has some special added values, detailed at the bottom of this page, for ease of use.
* The remaining 0-N elements are the scores in ``adjacentBelow`` from the IR response.
