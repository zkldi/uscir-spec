Chart Tracked - GET /charts/:chartHash
==================================

Used to check if the server will accept a score for a given chart in advance of submitting it.

################
Expected Request
################

Request data is entirely in the URL; viz. the value of ``:chartHash`` should be the chartHash you are interested in.

#################
Expected Response
#################

| If the server would refuse a score for this chart (e.g. it is blacklisted, or the server does not know it and does not accept unknown charts), respond with ``statusCode 42``.
| Otherwise, respond with ``statusCode 20``. The difference between a tracked chart and an unknown chart on a server that accepts unknown charts may be distinguished in the response description.
