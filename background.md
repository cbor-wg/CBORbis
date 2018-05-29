# Background (non-normative)

>>> _Those who cannot remember the past are condemned to repeat it,_
>>>
>>> George Santayana.

There is a meaningful (semantic) difference between data and information.  While
they are often used interchangabily, for data to be informative, it is generally
accepted that it _must not_ be unexpected, and _should_ answer a question,
or perhaps ask one.  Information generally has more structure than a simple
sequence of numbers or characters.

There are hundreds if not thousands of standardized formats for representation
of structured information in more or less generic computer-readable formats.
The background of IETF related efforts in this area can help put CBOR better
context.

## ASCII (1960-1969)

In 1960, the American Standards Associatio's X3 committee begin work on the
American Standard Code for Information Interchange (ASCII) standard.  Before
ASCII was first published as ASA X3.4-1963, and it's first commercial use (1963
in AT&T's TWX teleprinter network) character codes used by computers and
telprinters were frequently 5- or 6-bits and not standardized between competing
computer vendors.

The term "byte" did not imply an 8-bits unit of addressable memory, only a small
contigious set of bits, often 6 particularly on 36-bit word on computers (e.g.
the DECsystem 10, which was used by many early ARPAnet members.)  For this
reason it was not uncommon for computers to be restricted to upper case
characters.

ASCII was a 7-bit code, permitting 128 symbols without resoring to "shift"
codes, and this permitted lower case, although the first ASCII teleprinters
remained upper-case only devices.  The X3 committee consider an 8-bit code, but
rejected this because it would require all transmission to use 8 bits, when 7
would do, a 14% speed penalty.

By 1969, ASCII was young but maturing standard, having been through one major
version change in 1967, and certainly not universially implemented. In fact,
while IBM was active in the ASCII working group, their long-lived System/360
line of computers designed in 1963-1964 insted ended up using IBMs own their own
8-bit code EBCIDIC, based on their own earlier 6-bit codes, becaues it permitted
the use of older peripherals.  For decades, ASCII and EBCDIC were considerd
rival standards.   Both system goals were to encode information, not just
data.

The original ASCII code charts reserved 32 of the 128 bit combinations possible
in 7 bits as "control characters".  These controls did not have straightforward
printed representations, for example one named BEL defined as a character " for
use when there is a need to call for human attention." Some of these control
characters were designated as "information seperators", specifically those
called FS, GS, RS, US.

50 years later, there has bee a lot of progress.  The internet has standardised
on octets as the unit of data, now calling them bytes.  And more recently
standardized on ASCII-based Unicode for characters. So today, saying 8-bit bytes
may now seem redundant, but the subtle distinction between octets as a unit of
data, and byte as information (unsigned 8-bit binary number), and characters
(which may now occupy a variable number of octets) may still be useful to
express the diffeence between data and information.

But the ASCII information seperators, along with most of the other control characters are
now virtually unused. Unicode retains all of the orignal ASCII control
characters, but only U+0000 (NUL),U+0009 (HT or TAB), U+000A (LF or NL), and
U+000D (CR) remain in common use. Much of this has happened because layered
protocols have enabled a seperat of concerns such that the original purpose of
the control character is no longer pertinant at the application level.

The interpretation of U+000A as NL (newline) vs. the older convention of using a
the combination CR LF remains a common interperabililty issue, despite Unicode's
incorporation of the supplemental characters U+0085 (NEL), U+2028 (line
separator), and U+2029 (paragraph separator) intended to address the matter.
Some applications use U+001B (ESC), typically for compatability with legacy
VT-100 series video terminals.

## ARPANET, RFC-5 and RFC-20 (1969)

Many information representation standards use a textual format easing human
editing and review, such IETF related standards efforts date back to some of the
earliest internet RFCs, including _{{RFC-5}} The Decode-Encode Language (DEL)_,
published in 1969, which was summarized at the time as a machine independent
language tailored for both interactive consoles [human use] and
[machine-to-machine] communications.  Decode-Encode Language is now primarily of
historical interest only, but it's very title forms the of core terminology we
continue use today, specifically:

* Decoding is translating a data in the form of a stream of octets into
  higher-level information.

* Encoding is translating information into a stream of octets suitable for
  network communicationa or storage.

Also in 1969, {{RFC-20}} suggested use of 7-bit ASCII characters embedded in octets
with the high order bit set to 0, and encorporating the ASCII code chart into
its text, along with some notes on its use.  On the topic of information
seperators however, little was said.   Other then their "hierarchical
relationship", RFC-20 failed to define specifics, none the less, RFC-20 has had
a lasting impact on the world.  ASCII and its derrivtaves (including Unicode and
UTF-8) are broadly accepted planet-wide, and modern computer architectures
generally based on multiples of 8-bit bytes, for reasons arguabally related to
RFC-20.

Thus the terms "octet" and "octet stream" eventually adopted in the
internet community to describe data in-flight between communicating systems,
rather than information in a machine-dependent format in memory.

## Memos, messages, RFC 524 vs RFC 561 (1973)

In the meantime, effots to define better information encoding standards
continued, often building on ASCII, but they generally have not used the
information seperator control characters for their intended purposes. Instead
these plain-text information structure encodings abounded. Plain-text
information standard formats typically limit themselves to the printable
characters, space, CR, and LF, and can in general be expressed using only the
printable subset of ASCII, with the MSB set as RFC-20 suggested.

In 1973, rapid innovation around messaging lead to e-mail, the first killer
application of the internet.   RFC-524 suggested an extensive set of e-mail
specific _commands_ for structuring information about a message.   This model
was based on the commands of an imparitive programming languages.

In response the same year RFC-561 noted that "many systems send that
information, but each in a different format", and suggested as a short-term
alternative a simpler text-bsaed syntax framework for structured headers
preceding on a message, which looked similar to office memos commonly used
before e-mail.   RFC 561 was more based on a declarative model, addressing what
the message was, rather than how to process it.

RFC 561's framework for structured information was to define a `header` as a
sequence of `item`s, seperated by line breaks in the form CRLF.  Each
`item` in the header was a name-value pair composed of a `keyword` name, the
two-character sequence ": " as a seperator, and the remainder of the `line`
was treated as the value,  defined to allow  "any of the 128 ASCII
character except CR and LF."

In addition to the syntactic conventions for as sequence of name/value pairs,
RFC 561 defined a some additional semanic requirements for messages, a few of
which paraphrased below.   Text in brackets is to explain the context of the
time.

  1. The FROM, DATE", and SUBJECT items must each appear at most once in
     the header.  The order of these is insignificant however they must proceed
     all other items. \[There were various different permitted formats of office
     memorandum in different organizations, but these threee fields, if used,
     were considered essential to achieving an interoperable an e-mail message.]

  2. The case (upper or lower) of keywords is insignificant.  The keyword FROM
     may appear as "From", "from" or "FrOm". \[Console devices of the day might
     still be only uppercase, and organizations had different conventions for
     memos.]

RFC 561 _could_ have instead used the ASCII information seperator control
characters, e.g. RS instead of the sequence CRLF, and US instead
of ": ", but it didn't! A few contextual explanations may be helpful
in 2018.   1) Raw messages looked like typewritten memos using these seperators.
2) the ASCII console devices of the day couldn't represent the information
seperators visually. 3) RFC-20 was only a suggestion, computers of the day
didn't all store text in ASCII and might not be able to compactly store all
control characters.  4) The combination of both CR and LF was needed for
some console devices of the day, specifically teletypes.  Choice of syntax rules
may have seem unimportant because RFC 561 was explicitly intended as a
short-term solution, acknowledging the more complicated apparoach in RFC-524
might be better.  But RFC-561's memo-like _framework_ was extensible has
survived the test of time, forming the basis not only for the internet e-mail
system used well into the next century, but also for HTTP and thus the
world-wide web.

## ASN.1, ITU X.400  messaging and X.509 certificates (1984-1988)

in 1984, the International Telegraph Union (ITU, ITU-T or CCITT), in conjunction
with the International Organization for Standardization (ISO),began publishing
the X.400 series of reccomendations for messagge handling systems (MHS).  In
1988 ITU/ISO also begain publishing the X.500 series of reccomendations on
"directory services", which included X.509 reccomendations on public key
certificates.   As part of these effots, was not based on IETF related work, it
was designed instead run on top of a sperate ISO protocol stack, rather than
TCP/IP.

As part of this, effort a joint ISO/IEC and ITU-T published X.409:1984, which
described Abstract Syntax Notation One (ASN.1), which provides a set of rules
for describing the structure of ojects that are independent of machine-specific
encoding techniques in a precise,formal form designed to avoid ambiguities.
ASN.1 itself does not define concrete encodings, but instead layed the
groundwork for a series of different encodings known by names like Distinguished
Encoding Rules (DER), Basic Encoding Rules (BER), Cannonical Encoding Rules
(CER), XML Encoding Rules (XER), Packed Encoding Rules (PER, UPER, CPER, CUPPER
etc). Each of these seemingly unending set of encoding "rules" was distinct, and
typically based a different set of goals.  Together with the fact that the core
ASN.1 documents were  propriatary the ASN.1 standards were  hard-to-find for
volunteer implementers, particularly when compared to IETF's RFC standards.

Because the ITU was technically an agency of the United Nations and both it and
ISO could have substantial impact on international treaties on
telecommunications, X.400 received a lot of attention.  However, with most ISO
standards dealing with application-level networking, X.400 failed to compete
successfully with the Internet-based standards commercial applications in North
America.

One exception that ASN.1 was used to define the format of public key
certificates and related information storage containers, and these are generally
applicable to implementations of secured SSL/TLS (HTTPS) and related protocols.
Some IETF RFC's jumped on the ASN.1 bandwagon, for example RFC 3641 - Generic
String Encoding Rules (GSER) for ASN.1.

CBOR offers an alternative to ASN.1, and in particular RFC 8152 - CBOR Object
Signing and Encryption (COSE) published in 2017 addresses a non-ASN.1 approach
to protcol security.

## SGML (1986) and HTML (circa 1991)

## JavaScript and VBScript (1995-1999)

## XML (circia 1997)

XML was, like HTML, originally defined as profile of ISO standard SGML.   XML
takes a minimalist approach, dropping a number of grammar features that HTML
included, A related specification for namespaces incorporated an extensibility
mechanism permitting names to include a colon-delimited tag that was associated
with a URL prefix.  ...

XML and its extensions have regularly been criticized for verbosity and
complexity.   It is not viewed by some particularly human friendly, and may
distract from the information content.  The HTML 4.0 specification adopted an
XML basis, but HTML 5 effectively reversed this and took a different direction.
While XML 1.1 is clearly a successful information representation format, updated
versions, such as a sometimes discussed XML 2.0 have never materalized.

## YAML (2001) and JSON (2002)

In 2001, three individuals with interests in machine and language independent
information models first published documents on YAML at yaml.org.  The first
draft(s) were initially based on XML, but by the end of the year YAML had
evolved into a hybrid syntax which incoprated the memo-like formatting of RFC
561 and RFC 822, the core datatypes of Perl (scalars, maps, and lists), an
indentation-based block nesting notation like Python, whitespace conventions
from HTML, comments and string escapes syntax from C, aspects of MIME includnig
support for binary entitties, and concepts from XML.   In recognition of the
difference from XML, YAML was made a recursive retronym for “YAML Ain’t Markup
Language.”

In 2002, another interchange standard called JavaScript Object Notation (JSON)
started publishing posts at json.org.   JSON was almost immediately a very
mature approach, because it was based on ECMA-262 ECMAScript 3rd Edition of
December 1999. JSON, like YAML was based on a very similar identical information
model, but used different terms and notation for virtually identical concepts.

| YAML name  | JSON name                                 | Notes                                    |
| ---------- | ----------------------------------------- | ---------------------------------------- |
| `node`     | `value`                                   | Abstract data type, can be any of below. |
| `scalar`   | `string, number, true, false,` or  `null` | Virtually identical semantics.           |
| `mapping`  | `object`                                  | YAML is more flexible about key types.   |
| `sequence` | `array`                                   | Virtually identical                      |

=== Notable differences between YAML and JSON syntax and model

In YAML, `scalar` includes `strings`, `number`, `true`, `false`, `null` as well
as binary data in formats like base64.   YAML strings may be quoted with either
single, or double quotes, or strings may appear plain (unquoted) with certain
restrictions.   YAML is also extensible to support other scalar and compound
types using tags, with an option to perform identification of datatype of plain
(unquoted strings) using regular expressions, though the regex matching this may
not be implemented.

YAML supports a block mode format that traces it heritiage to e-mail headers
{{RFC822}}.  To this it adds the use of  indentation of blocks to represent
heirarichical nesting and permit multi-line strings in several ways.   While
these features are very convienent, they do not alter data model consisting of
scalar, mapping, and sequences.

JSON keys are restricted to `strings` and called `names`, while in YAML any
`node` may be a `key`.   In both YAML and JSON, the values may themselves be
made of nested complex `node`/`value`s, permitting heirarchical information
structures.  Conceptually both mappings and objects are described as unordereed.
In practices however, JSON `object`s often behaved as ordered collection of
name/value pairs, but this may be platform-dependent.

By the end of 2002, YAML working drafts had incorporated a "flow style" notation
nearly identical to JavaScript's object literals.   At some point the authors of
YAML and JSON became aware of each others efforts, and arranged syntax changes
with each other.   JSON started out as a subset of JavaScript, and tweaks were
made so that it was also a subset of YAML.   The concepts and data model in the
final draft of YAML 1.0 (in 2004) have remained reasonable stable since, and
minor updates since have dealt with issues.

YAMLs syntax was written to be decidedly more human-friendly that JSON, with
convienences like comments, optional quotaation marks  around plain text
strings, and the ability to place as trailing comma in a JavaScript style object
or array.  JSON on the other hand takes a minimalist approach to data
serialzation, omitting even comments in its formal syntax, (some
implementations permit C style comments `/*` ... `*/`.)   YAML supports both C
style comments and shell style comments `#` ....

## CBOR's history

RFC 7159 first introduced a stable approach to CBOR, including addressing some
requirements not well met by current formats.  The underlying information model
is quite similar to that of YAML, but RFC 7159 describes it as being based the
JSON data model {{RFC7159}}.  It is important to note that this is not a
proposal that the grammar of CBOR in RFC 7159 be extended in general, since
doing so would cause a significant backwards incompatibility with already
deployed JSON documents. Instead, this document simply defines its own data
model that starts from JSON.   However, both YAML and CBOR support extensions
using tags, but a compact CBOR parser may have limited or no support for tags.

{{comparison-app}} lists some existing binary data formats  supporting similar
information models and discusses how well they do or do not fit the design
objectives of the Concise Binary Object Representation (CBOR).
