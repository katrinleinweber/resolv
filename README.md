resolv
======

ldns DNS resolver wrapper libary for R

Needs `ldns` - http://www.nlnetlabs.nl/projects/ldns/ - which is `apt`-able and `brew`-able.

    library(devtools)
    install_github("hrbrmstr/resolv")
    
[These folks](http://dev.telnic.org/trac/wiki/DotTelUtils) seem to have done some work getting the `ldns` library to work under Windows, but this particular package only works (for now on Linux/Mac OS X.

Bug reports (esp from ppl with more C++/Rcpp experience), feature requests & pull requests welcome/encouraged. The code/package is documented pretty well (esp for me). Hopefully this library can replace `system` calls for folks who need to "do DNS stuff" from R.

### Description

Provides functions to perform robust DNS lookups from R. Uses the `ldns` library which provides support for IPv4 & IPv6 addresses as well as DNSSEC. This library currently exposes the functions indicated below.

### Details

    Package:     resolv
    Type:        Package
    Title:       Wrapper to ldns library for DNS calls from R
    Version:     0.1
    Date:        2014-04-26
    Author:      Bob Rudis (@hrbrmstr)
    Maintainer:  Bob Rudis <bob@rudis.net>
    Description: Wrapper to ldns library for DNS calls from R
    License:     MIT
    Imports:     Rcpp (>= 0.11.1)
    LinkingTo:   Rcpp

Direct `ldns` wrappers:

- `resolv_txt()` - perform TXT lookups
- `resolv_mx()` - perform MX lookups (returns list)
- `resolv_cname()` - perform CNAME lookups
- `resolv_srv()` - perform SRV lookups (returns list)
- `resolv_a()` - perform A lookups
- `resolv_ptr()` - perform PTR lookups

(TODO: to add "SOA", "NS", and other record retrieval functions as well as a `dig`-like one which returns the full response for a query)

Ancillary/"fun"ctions

These show off some of what you can do with DNS

- `ip2asn()` - interface to http://www.team-cymru.org/Services/ip-to-asn.html#dns
- `asninfo()` - interface to http://www.team-cymru.org/Services/ip-to-asn.html#dns
- `wikidns()` - interface to https://dgl.cx/wikipedia-dns
- `dnscalc()` - interface to http://www.isi.edu/touch/tools/dns-calc.html

### Examples

    require(resolv)
    library(plyr)

    ## google talk provides a good example for this
    ldply(resolv_srv("_xmpp-server._tcp.gmail.com."), unlist)
    priority weight port                         target
    1        5      0 5269      xmpp-server.l.google.com.
    2       20      0 5269 alt1.xmpp-server.l.google.com.
    3       20      0 5269 alt2.xmpp-server.l.google.com.
    4       20      0 5269 alt3.xmpp-server.l.google.com.
    5       20      0 5269 alt4.xmpp-server.l.google.com.
     
    ## where www.nasa.gov hosts
    resolv_a("www.nasa.gov")
    [1] "69.28.187.45"    "208.111.161.110"
    
    resolv_ptr("69.28.187.45")
    [1] "cds355.iad.llnw.net."
    
    ## DNS seekrit TXT URLs
    browseURL(gsub("\"", "", resolv_txt("google-public-dns-a.google.com")))


### Author(s)

   boB Rudis (@hrbrmstr) <bob@rudis.net>

### References

   http://www.nlnetlabs.nl/projects/ldns/

### See Also

- http://www.nlnetlabs.nl/projects/ldns/
- http://dds.ec/blog/posts/2014/Apr/making-better-dns-txt-record-lookups-with-rcpp/
