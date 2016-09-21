---
layout: protocol
active_page: protocol
title: Link Protocol Documentation
---

# Network Protocol

## Introduction

Link uses UDP multicast for broadcast over the local Network.

This describes version 1 of the protocol (only one supported at this time)

# concepts


## UDP Multicast group

224.76.78.75

# Protocol

## Packet

| Value         | Length      | Comment              |
|:--------------|------------:|:---------------------|
| ```_asdp_v``` | 7 bytes     | Protocol Recognition |
|    ```0x01``` | 1 byte      | Protocol version (1) |
|        Header | 12 bytes    | Message Header       |
|         Body  | ...         |                      |


## Message Header

| Value          | Length      | Comment              |
|:---------------|------------:|:---------------------|
| MessageType    | 1 byte      |                      |
| TTL            | 1 byte      | Time To Live         |
| SessionGroupId | 2 bytes     |                      |
| NodeId         | 8 bytes     | Random generated identifier for this node |

Note: NodeId always consists of printable characters (ASCII character range 33-126)

## MessageType

| Value | Comment  |
| ----- | -------- |
|  0x00 | ```INVALID```  |
|  0x01 | ```ALIVE```    |
|  0x02 | ```RESPONSE``` |
|  0x03 | ```BYEBYE```   |

## Body

Body is different depending on the MessageType

### ALIVE

The body consists of 3 blocks:

| Value | Key | Length | Comment |
| ----- | --- | ------ | ------- |
| Timeline | ```tmln``` | 32 bytes | |
| Session | ```sess``` | 16 bytes | |
| measurement endpoint v4 | ```mep4``` | 14 bytes | |

Each of these blocks starts with a key of 4 bytes, followed by 4 bytes indicating the length of the value which follows

| Value | Length | Comment |
| ----- | ------ | ------- |
| Key | 4 bytes | |
| Length | 4 bytes | |
| Value | Depending on ```Length``` field |

#### Timeline

| Value | Length | Comment |
| ----- | ------ | ------- |
| Tempo | 1 byte | ```bpm``` = 60 * ( 1e6 / ```Tempo``` ) |
| Beats | 1 byte | |
| TimeOrigin | 1 byte | |

#### Session

#### Measurement Endpoint v4
