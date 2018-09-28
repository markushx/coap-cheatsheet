# A Cheatsheet for the Constrained Application Protocol (CoAP)

>(RFC 6690, RFC 7252, RFC 7959, RFC 7641)

The Constrained Application Protocol (CoAP) is a specialized web
transfer protocol for use with constrained nodes and constrained (e.g.,
low-power, lossy) networks.

CoAP Message Format
===================

     0                   1                   2                   3
     0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |Ver| T |  TKL  |      Code     |          Message ID           |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |   Token (if any, TKL bytes) ...                               |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |   Options (if any) ...                                        |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |1 1 1 1 1 1 1 1|    Payload (if any) ...                       |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

Ver: Version, T: Type, TKL: Token Length

Message types
=============

    +------+-----------------+
    | Type | Name            |
    +------+-----------------+
    |    0 | CONfirmable     |
    |    1 | NON-confirmable |
    |    2 | ACKnowledgement |
    |    3 | ReSeT           |
    +------+-----------------+

Method codes
============

    +------+--------+
    | Code | Name   |
    +------+--------+
    | 0.00 | EMPTY  |
    +------+--------+
    | 0.01 | GET    |
    | 0.02 | POST   |
    | 0.03 | PUT    |
    | 0.04 | DELETE |
    +------+--------+

Response codes
==============

     0
     0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+
    |class|  detail |
    +-+-+-+-+-+-+-+-+

    +------------------+------------------------------+--------------+
    | Code             | Description                  |              |
    +------------------+------------------------------+--------------+
    | 2.01 (65, 0x41)  | Created                      | Success      |
    | 2.02 (66, 0x42)  | Deleted                      |              |
    | 2.03 (67, 0x43)  | Valid                        |              |
    | 2.04 (68, 0x44)  | Changed                      |              |
    | 2.05 (69, 0x45)  | Content                      |              |
    | 2.31 (95, 0x5F)  | Continue                     |              |
    +------------------+------------------------------+--------------+
    | 4.00 (128, 0x80) | Bad Request                  | Client Error |
    | 4.01 (129, 0x81) | Unauthorized                 |              |
    | 4.02 (130, 0x82) | Bad Option                   |              |
    | 4.03 (131, 0x83) | Forbidden                    |              |
    | 4.04 (132, 0x84) | Not Found                    |              |
    | 4.05 (133, 0x85) | Method Not Allowed           |              |
    | 4.06 (134, 0x86) | Not Acceptable               |              |
    | 4.08 (136, 0x88) | Request Entity Incomplete    |              |
    | 4.12 (140, 0x8C) | Precondition Failed          |              |
    | 4.13 (141, 0x8D) | Request Entity Too Large     |              |
    | 4.15 (143, 0x8F) | Unsupported Content-Format   |              |
    +------------------+------------------------------+--------------+
    | 5.00 (160, 0xA0) | Internal Server Error        | Server Error |
    | 5.01 (161, 0xA1) | Not Implemented              |              |
    | 5.02 (162, 0xA2) | Bad Gateway                  |              |
    | 5.03 (163, 0xA3) | Service Unavailable          |              |
    | 5.04 (164, 0xA4) | Gateway Timeout              |              |
    | 5.05 (165, 0xA5) | Proxying Not Supported       |              |
    +------------------+------------------------------+--------------+

Options
=======

      0   1   2   3   4   5   6   7
    +---------------+---------------+
    |  Option Delta | Option Length |   1 byte
    +---------------+---------------+
    /         Option Delta          /   0-2 bytes
    \          (extended)           \
    +-------------------------------+
    /         Option Length         /   0-2 bytes
    \          (extended)           \
    +-------------------------------+
    /         Option Value          /   0 or more bytes
    +-------------------------------+

    +-----+----+---+---+---+----------------+--------+--------+-------------+
    | No. | C  | U | N | R | Name           | Format | Length | Default     |
    +-----+----+---+---+---+----------------+--------+--------+-------------+
    |   1 | x  |   |   | x | If-Match       | opaque | 0-8    | (none)      |
    |   3 | x  | x | - |   | Uri-Host       | string | 1-255  | (see note 1)|
    |   4 |    |   |   | x | ETag           | opaque | 1-8    | (none)      |
    |   5 | x  |   |   |   | If-None-Match  | empty  | 0      | (none)      |
    |   7 | x  | x | - |   | Uri-Port       | uint   | 0-2    | (see note 1)|
    |   8 |    |   |   | x | Location-Path  | string | 0-255  | (none)      |
    |  11 | x  | x | - | x | Uri-Path       | string | 0-255  | (none)      |
    |  12 |    |   |   |   | Content-Format | uint   | 0-2    | (none)      |
    |  14 |    | x | - |   | Max-Age        | uint   | 0-4    | 60          |
    |  15 | x  | x | - | x | Uri-Query      | string | 0-255  | (none)      |
    |  17 | x  |   |   |   | Accept         | uint   | 0-2    | (none)      |
    |  20 |    |   |   | x | Location-Query | string | 0-255  | (none)      |
    |  28 |    |   | x |   | Size2          | uint   | 0-4    | (none)      |
    |  35 | x  | x | - |   | Proxy-Uri      | string | 1-1034 | (none)      |
    |  39 | x  | x | - |   | Proxy-Scheme   | string | 1-255  | (none)      |
    |  60 |    |   | x |   | Size1          | uint   | 0-4    | (none)      |
    +-----+----+---+---+---+----------------+--------+--------+-------------+

C=Critical, U=Unsafe, N=No-Cache-Key, R=Repeatable\
Note 1: taken from destination address/port of request message

Content-Formats
===============

    +--------------------------+-----+
    | Media type               | Id. |
    +--------------------------+-----+
    | text/plain;charset=utf-8 |   0 |
    | application/link-format  |  40 |
    | application/xml          |  41 |
    | application/octet-stream |  42 |
    | application/exi          |  47 |
    | application/json         |  50 |
    | application/cbor         |  60 |
    +--------------------------+-----+

URI schemes
===========

    coap-URI = "coap:" "//" host [ ":" port ] path-abempty [ "?" query ]
    coaps-URI = "coaps:" "//" host [ ":" port ] path-abempty [ "?" query ]

Transmission parameters
=======================

    +-------------------+---------------+
    | name              | default value |
    +-------------------+---------------+
    | ACK_TIMEOUT       | 2 seconds     |
    | ACK_RANDOM_FACTOR | 1.5           |
    | MAX_RETRANSMIT    | 4             |
    | NSTART            | 1             |
    | DEFAULT_LEISURE   | 5 seconds     |
    | PROBING_RATE      | 1 Byte/second |
    +-------------------+---------------+

Link Format .well-known/core
============================

Link format can be used to describe hosted resources, their attributes,
and other relationships between links.

Example:

    REQ: GET /.well-known/core

    RES: 2.05 Content

    </sensors>;ct=40;title="Sensor Index",
    </sensors/temp>;rt="temperature-c";if="sensor",
    </sensors/light>;rt="light-lux";if="sensor",
    <http://www.example.com/sensors/t123>;anchor="/sensors/temp";rel="describedby",
    </t>;anchor="/sensors/temp";rel="alternate"

Block
=====

In order to transfer larger payloads with CoAP $-$ for instance, for
firmware updates $-$ the Block option can be used.

    +-----+---+---+---+---+----------------+--------+--------+----------+
    | No. | C | U | N | R | Name           | Format | Length | Default  |
    +-----+---+---+---+---+----------------+--------+--------+----------+
    |  23 | x | x | - | - | Block2         | uint   | 0-3 B  | (none)   |
    |  27 | x | x | - | - | Block1         | uint   | 0-3 B  | (none)   |
    +-----+---+---+---+---+----------------+--------+--------+----------+

     0
     0 1 2 3 4 5 6 7
    +-+-+-+-+-+-+-+-+
    |  NUM  |M| SZX |
    +-+-+-+-+-+-+-+-+

     0                   1
     0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |          NUM          |M| SZX |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

     0                   1                   2
     0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
    |                   NUM                 |M| SZX |
    +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

Observe
=======

In order to follow state changes of CoAP resources the Observe option
can be used.

    +-----+---+---+---+---+---------+------------+-----------+---------+
    | No. | C | U | N | R | Name    | Format     | Length    | Default |
    +-----+---+---+---+---+---------+------------+-----------+---------+
    |   6 |   | x | - |   | Observe | uint       | 0-3 B     | (none)  |
    +-----+---+---+---+---+---------+------------+-----------+---------+

References
==========

This cheatsheet is based on and heavily stole from the following
documents:\

<span>Link-format: <http://tools.ietf.org/html/rfc6690>\
CoAP: <http://tools.ietf.org/html/rfc7252>\
Block: <http://tools.ietf.org/html/rfc7959>\
Observe: <http://tools.ietf.org/html/rfc7641>\
</span>
