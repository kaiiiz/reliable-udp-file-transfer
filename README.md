# Reliable UDP File Transfer

This is the final exam problem 1 of Intro. to Network Programming at NCTU in Spring 2020.

## Problem Statement

Implement a UDP Server and UDP Client. Your client should be able to send a file to the server and your server should be able to receive multiple files simultaneously.

Command format:

* Server
    * `./server {number-of-client-connections} {port}`
* Client
    * `./client {server-ip} {server-port} {file-name}`

Notes:

* The specified file will locate on the same directory as client program
* Clients will use ordered port to send multiple files:
    * `./client {server-ip} {server-port} {file-name}`
    * `./client {server-ip} {server-port+ 1} {file-name}`
    * `./client {server-ip} {server-port+ 2} {file-name}`
    * `./client {server-ip} {server-port+ 3} {file-name}`
    * `./client {server-ip} {server-port + 4} {file-name}`

## Implementation

### Client

Client use the class `pkt` to split one file to multiple chunk, add seqence number and checksum in the packet to ensure the correctness of the payload. After sending one packet, the client will wait the ACK response from server, if server not response in `timeout` second, resend the packet again.

### Server

Server calculate the sha1 hash of payload, compare with the checksum. If the packet pass the hash check, send ACK back to client. After receive all packet, re-construct the whole file.
