# CBOR data models

CBOR is explicit about its generic data model, which defines the set
of all data items that can be represented in CBOR.  Its basic generic
data model is extensible by the registration of simple type values and
tags.  Applications can then subset the resulting extended generic
data model to build their specific data models.

Within environments that can represent the data items in the generic
data model, generic CBOR encoders and decoders can be implemented
(which usually involves defining additional implementation data types
for those data items that do not already have a natural representation
in the environment).  The ability to provide generic encoders and
decoders is an explicit design goal of CBOR; however many applications
will provide their own application-specific encoders and/or decoders.

In the basic (un-extended) generic data model, a data item is one of:

* an integer in one of the ranges 0..2\*\*64-1 ("unsigned") and
  -2\*\*64..-1 ("negative")
* a simple value, identified by a number
  between 0 and 255, but distinct from that number
* a floating point value, distinct from an integer, out of the set
  representable by IEEE 754 binary64 (including non-finites)
* a sequence of zero or more bytes ("byte string")
* a sequence of zero or more Unicode code points ("text string")
* a sequence of zero or more data items ("array")
* a mapping (mathematical function) from zero or more data items
  ("keys") each to a data item ("values"), ("map")
* a tagged data item, comprising a tag (an integer in the range
  0..2\*\*64-1) and a value (a data item)

Note that integer and floating-point values are distinct in this
model, even if they have the same numeric value.

This basic generic data model comes pre-extended by the registration
of a number of simple values and tags right in this document, such as:

* `false`, `true`, `null`, and `undefined` (simple values identified by 20..23)
* integer and floating point values with a larger range and precision
  than the above (tags 2 to 5)
* application data types such as a point in time or an RFC 3339
  date/time string (tags 1, 0)

Further elements of the extended generic data model can be (and have
been) defined via the IANA registries created for CBOR.  Even if such
an extension is unknown to a generic encoder or decoder, data items
using that extesion can be passed to or from the application by
representing them at the interface to the application within the basic
generic data model, i.e., as generic values of a simple type or
generic tagged items.

In other words, the basic generic data model is stable as defined in
this document, while the extended generic data model expands by the
registration of new simple values or tags, but never shrinks.

While there is a strong expectation that generic encoders and decoders
can represent `false`, `true`, and `null` in the form appropriate for
their programming environment, implementation of the data model
extensions created by tags is truly optional and a matter of
implementation quality.

A specific data model usually subsets the extended generic data model
and assigns application semantics to the data items within this subset
and its components.  When documenting such specific data models,
where it is desired to specify the types of data items, it is
preferred to identify the types by their names in the generic data
model ("negative integer", "array") instead of by referring to aspects
of their CBOR representation ("major type 1", "major type 4").
