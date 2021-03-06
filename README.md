Why interface versioning?
=========================
When you have a server with an interface and clients that use that interface
you either have to update every client on every server-release
or you have to take care of backward-compatibility and interface versioning,
otherwise older clients will not work after the release.

How does this framework help?
=============================
When you try to implement a backward-compatible interface by yourself,
you have to write much boilerplate code.
With this framework you are able to replace most of the boilerplate code with some simple annotations.

Example:
--------------------------------
   "address": {
       "street": {
           "name": "Samplestreet",
           "number": "1"
       },
       "city": "12345 Samplecity"
   }
When you want to rename "name" of "street", you have to copy it to be backward-compatible like
   "address": {
       "street": {
           "name": "Samplestreet",
           "streetName": "Samplestreet",
           "number": "1"
       },
       "city": "12345 Samplecity"
   }
Of course your server has to deal with both kind of data, one that contains a "name"
(from older clients) and one that contains "streetName" (from newer clients).
When answering on such requests, both fields have to be filled.
This framework automatically supports such backward-compatibility mapping by Java Annotations.
Simply annotate the "streetName" field in Java with @MovedFrom

    public class Street {
        @Deprecated
        private String ~ ~name~ ~;
        @MovedFrom("name")
        private String streetName;
        private String number;
    }
