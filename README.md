# Caching Learning Notes

## Valkey

## Redis Internals

[Redis Internals- Build Redis from Scratch](https://www.youtube.com/playlist?list=PLsdq-3Z1EPT0eElcdOON9fdaeaQjlyXDt) by Arpit Bhayani

[Why and How Is Single-Threaded Redis Fast and Can Handle Multiple Connections?](https://youtu.be/h30k7YixrMo?si=A6W1b8d0ydo0hcap)

[Writing a Simple TCP Echo Server - Step 0 to Build Your Own Redis](https://youtu.be/zlxdX9f4l50?si=iDos7c6LSnlTQBHE) 

[Wire Protocols, Why Are They Needed, and Redis' Wire Protocol - RESP](https://youtu.be/PtJl3jtmqgE?si=tdF0Ypx-jCagasXg)

"For a Redis Server to understand what a client wants, the client needs to follow a protocol": [Redis Serialization Protocol](https://redis.io/docs/latest/develop/reference/protocol-spec/)

<!--
https://valkey.io/topics/protocol/
-->

RESP supports common datatypes ("Redis is an extremely simple database")
* int (:)
* string (+), bulk string ($)
* arrays
* a way to convey errors

Serializing an array of strings using RESP: PUT K V ["PUT", "K", "V"] 

RESP is a request-response protocol (client sends request in RESP, server sends response in RESP)

Datatype starts with special character and ends with \r\n (CLRF)

Examples:
* Simple string: +PONG\r\n\ (very efficient, very low memory overhead, requires n+3 bytes in response)
* Integer: :1729\r\n
* Bulk strings: $4\r\n\PONG\r\n\ (PONG is 4 bytes, binary safe)

Importance of bulk strings
* Simple strings are not binary safe (can contain any byte)
* Simple strings cannot contain \r\n (also a terminator)
* It knows exactly how many bytes to read. Can store and send any binary info. 
* Can be used to store any binary data (even a PNG image)

Some special strings
* Empty string: $0\r\n\r\n
* Null value: $-1\r\n (length -1 is special value indicating lack of data)

Arrays

["a", 200, "cat"] becomes:
*3\r\n (asterisk, number of elements, CLRF, RESP-enoded elements)
$1\r\na\r\n
:200\r\n
$3\r\ncat\r\n

Some special arrays
* Empty array: *0\r\n
* Null array: *-1\r\n (length -1 is special value indicating lack of data)
  
Note: can also have nested arrays

[Implementing Redis' Wire Protocol - RESP](https://youtu.be/wnHZHGc8tW8?si=025wLUAUFmtE4B7q)

[Implementing Redis' PING Command](https://youtu.be/70UJ1QTQj5Y?si=foFDCG3F1he8pTkB)

[Event Loops Internals And How Redis Handles Multiple Connects on a Single Thread](https://youtu.be/U2IFZ_hw91o?si=VzZ55CHC4z1cf0hc)

[Implementing Event Loops](https://youtu.be/SMmLVHgE4pM?si=zBAklRoupJpBKkLA)

[Implementing GET, SET, and TTL Commands in Redis](https://youtu.be/1rnEpo56LFk?si=mQ_wlfl0z_aDEMam)

[Implementing DEL, EXPIRE, and Cleanup in Redis](https://youtu.be/yGdk0hmvkgo?si=yDESzMspeziXVFht)

[Key Eviction Strategies in Redis and Implementing Simple First Eviction](https://youtu.be/EkaTFT9ox-I?si=a_JL5bK2PUm4d-Tf)

[Implementing Command Pipelining](https://youtu.be/2q7RuEb9z-M?si=SYLiGS1yRsKAfXX5)

[Implementing AOF Persistence](https://youtu.be/5SnkVoatBpA?si=iisw--vzrqNDbCrO)

[Object, Encodings, and Implementing INCR](https://youtu.be/Dl9h6sQAFfk?si=VAM1SrLDVxqAin64)

[Implementing INFO and allkeys-random Eviction](https://youtu.be/jmN3VRsUmRs?si=OhqFJnW29r0OH4w-)

[The Approximated LRU Algorithm](https://youtu.be/0kOCyjFKKL4?si=cszXqNpgAbnJI3xl)

[Implementing the Approximated LRU Algorithm](https://youtu.be/FvGGkzcKKFA?si=shVnUABoIb8DBT8t)

[How Redis Caps Its Memory Usage](https://youtu.be/xNfoeZuK3Mc?si=RrX-BRTiPInpC2bR)



