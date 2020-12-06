GET /charts/:chartHash
==================================

Checks if a chart is supported on the server.

################
Expected Request
################

Request data is entirely in the URL; viz. the value of ``:chartHash`` should be the chartHash you are requesting.

#################
Expected Response
#################

The server should return JSON with a ``statusCode`` of 42, if the chart is not supported, or 20 if the chart is supported.