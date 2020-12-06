GET /charts/:chartHash
==================================

Used to check if the server will accept a score for a given chart in advance of submitting it.

################
Expected Request
################

Request data is entirely in the URL; viz. the value of ``:chartHash`` should be the chartHash you are interested in.

#################
Expected Response
#################

The server will respond with a ``statusCode`` of 42, if the server will refuse a score for this chart.
If the server is currently tracking this chart, OR if the server will begin tracking this chart upon receiving a score,
the correct response is 20. These cases may be distinguished for presentation to the user by a differing description.
