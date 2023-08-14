---
title: "Osi"
date: 2023-05-22T11:13:38+04:00
draft: true
tags:
  - "network"
---
# Open Systems Interconnection model (OSI mondel)

OSI split into 7 abstarction layers:
Physical, Data Link, Network, Transport, Session, Presentation, and Application.
Each layer serves a class of functionality to the layer above it and is served the layer below it.

OSI is the most popular network model, because of it user-friendly and PDUs.
PDU - single unit of information transmitted among peers of a network.

No 1 is the lowest layer and 7 is the highest layer.

| No  | Layer        | Protocol data unit (PDU) | Function                                                                                         |
| --- | ------------ | ------------------------ | ------------------------------------------------------------------------------------------------ |
| 7   | Application  | Data                     |                                                                                                  |
| 6   | Presentation | Data                     |                                                                                                  |
| 5   | Session      | Data                     |                                                                                                  |
| 4   | Transport    | Segment, Datagram        |                                                                                                  |
| 3   | Network      | Packet                   | Structuring and managing a multi-node network, including addressing, routing and traffic control |
| 2   | Data link    | Frame                    | Transmission of data frames between two nodes connected by a physical layer                      |
| 1   | Physical     | Bit, Symbol              | Transmission and reception of raw bit streams over a physical medium                             |
