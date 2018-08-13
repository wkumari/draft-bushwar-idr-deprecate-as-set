**Important:** Read CONTRIBUTING.md before submitting feedback or contributing
```




Network Working Group                                            R. Bush
Internet-Draft                                 Internet Initiative Japan
Intended status: Informational                                 W. Kumari
Expires: February 13, 2019                                        Google
                                                         August 12, 2018


             Deprecation of AS_SET and AS_CONFED_SET in BGP
                 draft-bushwar-idr-deprecate-as-set-00

Abstract

   RFC 6472, published in 2011, recommends against the use of the AS_SET
   and AS_CONFED_SET types of the AS_PATH in BGPv4.  This was done to
   simplify the design and implementation of BGP and to make the
   semantics of the originator of a route more clear.  This document
   deprecates both AS_SETs, and atomic aggregate from the BGPv4
   protocol.

   [ Ed note: Text inside square brackets ([]) is additional background
   information, answers to frequently asked questions, general musings,
   etc.  They will be removed before publication.]

   [ This document is being collaborated on in Github at:
   https://github.com/wkumari/draft-bushwar-idr-deprecate-as-set . The
   most recent version of the document, open issues, etc should all be
   available here.  The authors (gratefully) accept pull requests ]

Status of This Memo

   This Internet-Draft is submitted in full conformance with the
   provisions of BCP 78 and BCP 79.

   Internet-Drafts are working documents of the Internet Engineering
   Task Force (IETF).  Note that other groups may also distribute
   working documents as Internet-Drafts.  The list of current Internet-
   Drafts is at https://datatracker.ietf.org/drafts/current/.

   Internet-Drafts are draft documents valid for a maximum of six months
   and may be updated, replaced, or obsoleted by other documents at any
   time.  It is inappropriate to use Internet-Drafts as reference
   material or to cite them other than as "work in progress."

   This Internet-Draft will expire on February 13, 2019.







Bush & Kumari           Expires February 13, 2019               [Page 1]

Internet-Draft                  template                     August 2018


Copyright Notice

   Copyright (c) 2018 IETF Trust and the persons identified as the
   document authors.  All rights reserved.

   This document is subject to BCP 78 and the IETF Trust's Legal
   Provisions Relating to IETF Documents
   (https://trustee.ietf.org/license-info) in effect on the date of
   publication of this document.  Please review these documents
   carefully, as they describe your rights and restrictions with respect
   to this document.  Code Components extracted from this document must
   include Simplified BSD License text as described in Section 4.e of
   the Trust Legal Provisions and are provided without warranty as
   described in the Simplified BSD License.

Table of Contents

   1.  Introduction  . . . . . . . . . . . . . . . . . . . . . . . .   2
     1.1.  Requirements notation . . . . . . . . . . . . . . . . . .   3
   2.  MEasurements  . . . . . . . . . . . . . . . . . . . . . . . .   3
   3.  Updates . . . . . . . . . . . . . . . . . . . . . . . . . . .   3
     3.1.  Updates to RFC4271 to remove AS_SET . . . . . . . . . . .   3
     3.2.  Updates to RFC4271 to remove ATOMIC_AGGREGATE . . . . . .   5
   4.  new section . . . . . . . . . . . . . . . . . . . . . . . . .   6
   5.  IANA Considerations . . . . . . . . . . . . . . . . . . . . .   6
   6.  Security Considerations . . . . . . . . . . . . . . . . . . .   6
   7.  Acknowledgements  . . . . . . . . . . . . . . . . . . . . . .   7
   8.  References  . . . . . . . . . . . . . . . . . . . . . . . . .   7
     8.1.  Normative References  . . . . . . . . . . . . . . . . . .   7
     8.2.  Informative References  . . . . . . . . . . . . . . . . .   7
   Appendix A.  Changes / Author Notes.  . . . . . . . . . . . . . .   7
   Authors' Addresses  . . . . . . . . . . . . . . . . . . . . . . .   7

1.  Introduction

   [RFC6472], published in 2011, recommends against the use of the
   AS_SET and AS_CONFED_SET types of the AS_PATH in BGPv4.  This was
   done to simplify the design and implementation of BGP and to make the
   semantics of the originator of a route more clear.  [RFC6472],
   Section 4 also said:

      "Future work may update the protocol to remove support for the
      AS_SET path segment type of the AS_PATH attribute.  This future
      work will remove complexity and code that are not exercised very
      often, thereby decreasing the attack surface."






Bush & Kumari           Expires February 13, 2019               [Page 2]

Internet-Draft                  template                     August 2018


   This document completes that work by deprecating AS_SETs (and
   AS_CONFED_SETs) from the BGP protocol, allowing "compliant"
   implementations to not implement support for them.

   Here is a reference to an "external" (non-RFC / draft) thing:
   ([IANA.AS_Numbers]).  And this is a link to an
   ID:[I-D.ietf-sidr-iana-objects].

1.1.  Requirements notation

   The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
   "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this
   document are to be interpreted as described in [RFC2119].

2.  MEasurements

   Measurements performed in 2009 showed that "that aggregation that
   involves AS_SETs is very seldom used in practice on the public
   network [Analysis] and, when it is used, it is usually used
   incorrectly -- reserved AS numbers ([RFC1930]) and/or only a single
   AS in the AS_SET are by far the most common case. " [RFC6472]
   recommended that operators not perform aggregation that results in
   the creation of AS_SET or AS_CONFED_SET.

   Recent research [[ CITE ]] demonstrates that this RFC is largely
   being followed, and AS_PATHs containing these attributes are very
   rare, and / or nonsensical (e.g AS1 AS2 AS3 { AS3 AS3 AS3 } ).  This
   makes AS_SETS, AS_CONFED_SETS, and ATOMIC_AGGREGATE (which relies on
   AS_SETS) a vestigial feature.

3.  Updates

   As this document is deprecating (removing) a feature, the large
   majority of the updates involved simply deleting references to the
   feature in documents.

3.1.  Updates to RFC4271 to remove AS_SET

   From [RFC4271] Section 4.3 from












Bush & Kumari           Expires February 13, 2019               [Page 3]

Internet-Draft                  template                     August 2018


 b) AS_PATH (Type Code 2):

            AS_PATH is a well-known mandatory attribute that is composed
            of a sequence of AS path segments.  Each AS path segment is
            represented by a triple <path segment type, path segment
            length, path segment value>.

            The path segment type is a 1-octet length field with the
            following values defined:

               Value      Segment Type

               1         AS_SET: unordered set of ASes a route in the
                            UPDATE message has traversed

               2         AS_SEQUENCE: ordered set of ASes a route in
                            the UPDATE message has traversed

   delete:



             1         AS_SET: unordered set of ASes a route in the
                               UPDATE message has traversed

   Update the first paragraph of [RFC4271] Section 5.1.2 to read:

    5.1.2.  AS_PATH

     AS_PATH is a well-known mandatory attribute.  This attribute
     identifies the autonomous systems through which routing information
     carried in this UPDATE message has passed.  The components of this
     list are AS_SEQUENCEs.

   In [RFC4271] Section 9.1.2.2 replace:

           a) Remove from consideration all routes that are not tied for
          having the smallest number of AS numbers present in their
          AS_PATH attributes.  Note that when counting this number, an
          AS_SET counts as 1, no matter how many ASes are in the set.

   with:

           a) Remove from consideration all routes that are not tied for
           having the smallest number of AS numbers present in their
           AS_PATH attributes.

   In section 9.1.2.2 replace:



Bush & Kumari           Expires February 13, 2019               [Page 4]

Internet-Draft                  template                     August 2018


         Similarly, neighborAS(n) is a function that returns the
         neighbor AS from which the route was received.  If the route is
         learned via IBGP, and the other IBGP speaker didn't originate
         the route, it is the neighbor AS from which the other IBGP
         speaker learned the route.  If the route is learned via IBGP,
         and the other IBGP speaker either (a) originated the route, or
         (b) created the route by aggregation and the AS_PATH attribute
         of the aggregate route is either empty or begins with an
         AS_SET, it is the local AS.

   with:

         Similarly, neighborAS(n) is a function that returns the
         neighbor AS from which the route was received.  If the route is
         learned via IBGP, and the other IBGP speaker didn't originate
         the route, it is the neighbor AS from which the other IBGP
         speaker learned the route.  If the route is learned via IBGP,
         and the other IBGP speaker either (a) originated the route, or
         (b) created the route by aggregation and the AS_PATH attribute
         of the aggregate route is empty, it is the local AS.

3.2.  Updates to RFC4271 to remove ATOMIC_AGGREGATE

   From [RFC4271] Section 4.3 delete



          f) ATOMIC_AGGREGATE (Type Code 6)

             ATOMIC_AGGREGATE is a well-known discretionary attribute of
             length 0.

             Usage of this attribute is defined in 5.1.6.

   From [RFC4271] Section 5 delete

   ATOMIC_AGGREGATE   see Section 5.1.6 and 9.1.4

   Delete the entirety of[RFC4271] Section 5.1.6 ATOMIC_AGGREGATE.

   From [RFC4271] Section 9.1.4 replace the last paragraph:










Bush & Kumari           Expires February 13, 2019               [Page 5]

Internet-Draft                  template                     August 2018


    If a BGP speaker chooses to aggregate, then it SHOULD either include
    all ASes used to form the aggregate in an AS_SET, or add the
    ATOMIC_AGGREGATE attribute to the route.  This attribute is now
    primarily informational.  With the elimination of IP routing
    protocols that do not support classless routing, and the elimination
    of router and host implementations that do not support classless
    routing, there is no longer a need to de-aggregate.  Routes SHOULD
    NOT be de-aggregated.  In particular, a route that carries the
    ATOMIC_AGGREGATE attribute MUST NOT be de-aggregated.  That is, the
    NLRI of this route cannot be more specific.  Forwarding along such a
    route does not guarantee that IP packets will actually traverse only
    ASes listed in the AS_PATH attribute of the route.

   with

    If a BGP speaker chooses to aggregate, then it SHOULD include
    all ASes used to form the aggregate in an AS_SET. This attribute
    is now primarily informational.  With the elimination of IP routing
    protocols that do not support classless routing, and the elimination
    of router and host implementations that do not support classless
    routing, there is no longer a need to de-aggregate.  Routes SHOULD
    NOT be de-aggregated.

   From [RFC4271] Section 9.2.2.2 remove:

       ATOMIC_AGGREGATE:
            If at least one of the routes to be aggregated has
            ATOMIC_AGGREGATE path attribute, then the aggregated route
            SHALL have this attribute as well.

4.  new section

5.  IANA Considerations

   The IANA is requested to make the following changes:

   From the "BGP Path Attributes" sub-registry of the "Border Gateway
   Protocol (BGP) Parameters" (https://www.iana.org/assignments/bgp-
   parameters/bgp-parameters.xhtml) registry, replace value 6
   (ATOMIC_AGGREGATE) with "ATOMIC_AGGREGATE (deprecated)" and reference
   this RFC.

6.  Security Considerations

   By removing seldom used features (and seldom exercised code-paths)
   this document simplifies the implementation of the BGP protocol,
   thereby improving security.




Bush & Kumari           Expires February 13, 2019               [Page 6]

Internet-Draft                  template                     August 2018


7.  Acknowledgements

   The authors wish to thank some folk.

8.  References

8.1.  Normative References

   [IANA.AS_Numbers]
              IANA, "Autonomous System (AS) Numbers",
              <http://www.iana.org/assignments/as-numbers>.

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119,
              DOI 10.17487/RFC2119, March 1997,
              <https://www.rfc-editor.org/info/rfc2119>.

   [RFC4271]  Rekhter, Y., Ed., Li, T., Ed., and S. Hares, Ed., "A
              Border Gateway Protocol 4 (BGP-4)", RFC 4271,
              DOI 10.17487/RFC4271, January 2006,
              <https://www.rfc-editor.org/info/rfc4271>.

   [RFC6472]  Kumari, W. and K. Sriram, "Recommendation for Not Using
              AS_SET and AS_CONFED_SET in BGP", BCP 172, RFC 6472,
              DOI 10.17487/RFC6472, December 2011,
              <https://www.rfc-editor.org/info/rfc6472>.

8.2.  Informative References

   [I-D.ietf-sidr-iana-objects]
              Manderson, T., Vegoda, L., and S. Kent, "RPKI Objects
              issued by IANA", draft-ietf-sidr-iana-objects-03 (work in
              progress), May 2011.

Appendix A.  Changes / Author Notes.

   [RFC Editor: Please remove this section before publication ]

   From -00 to -01

   o  Nothing changed in the template!

Authors' Addresses








Bush & Kumari           Expires February 13, 2019               [Page 7]

Internet-Draft                  template                     August 2018


   Randy Bush
   Internet Initiative Japan
   5147 Crystal Springs
   Bainbridge Island, Washington  98110
   US

   Email: randy@psg.com


   Warren Kumari
   Google
   1600 Amphitheatre Parkway
   Mountain View, CA  94043
   US

   Email: warren@kumari.net



































Bush & Kumari           Expires February 13, 2019               [Page 8]
```
