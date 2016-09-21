---
layout: protocol
active_page: protocol
title: Link Protocol Documentation
---

*WORK-IN-PROGRESS*

# Network Protocol

## Introduction

Ableton Link uses UDP multicast for broadcast over the local Network.

This describes version 1 of the protocol (only one supported at this time)

# concepts


## UDP Multicast group

Hostname: 224.76.78.75

Port: 20808

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

| Value    | Key          | Length   | Comment |
| :------- | :----------- | -------: | :------ |
| Timeline | ```tmln```   | 32 bytes | |
| Session  | ```sess```   | 16 bytes | |
| Measurement Endpoint v4 | ```mep4``` | 14 bytes | |

Each of these blocks starts with a key of 4 bytes, followed by 4 bytes indicating the length of the value which follows

| Value  | Length                          | Comment |
| :----- | ------------------------------: | :------ |
| Key    | 4 bytes                         |         |
| Length | 4 bytes                         |         |
| Value  | Depending on ```Length``` field |         |

#### Timeline

| Value      | Length  | Comment |
| :--------- | ------: | :------ |
| Tempo      | 8 byte  | ```bpm``` = 60 * ( 1e6 / ```Tempo``` ) |
| Beats      | 8 byte | |
| TimeOrigin | 8 byte | |

#### Session

| Value      | Length  | Comment |
| :--------- | ------: | :------ |
| NodeId     | 8 byte  | NodeId of the initiating session |

#### Measurement Endpoint v4

| Value        | Length  | Comment |
| :----------- | ------: | :------ |
| IPv4 address | 8 byte  | |
| Port number  | 2 byte  | |
