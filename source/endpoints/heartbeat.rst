GET /
==================================

Used to check your connection to the server.

################
Expected Request
################

No data is expected, other than the Authorization header (which should be present on all requests.)

#################
Expected Response
#################

The server will return 20, and an empty object for ``body``, if the connection is okay.
The server will return 41 if Authorization fails.
The server will return 43 if the token provided is banned by the network.
