# Caching Learning Notes

## Valkey

## Redis Internals

[Redis Internals- Build Redis from Scratch](https://www.youtube.com/playlist?list=PLsdq-3Z1EPT0eElcdOON9fdaeaQjlyXDt) by Arpit Bhayani

[Why and How Is Single-Threaded Redis Fast and Can Handle Multiple Connections?](https://youtu.be/h30k7YixrMo?si=A6W1b8d0ydo0hcap)

[Writing a Simple TCP Echo Server - Step 0 to Build Your Own Redis](https://youtu.be/zlxdX9f4l50?si=iDos7c6LSnlTQBHE) 

[Wire Protocols, Why Are They Needed, and Redis' Wire Protocol - RESP](https://youtu.be/PtJl3jtmqgE?si=tdF0Ypx-jCagasXg)

<!--
https://valkey.io/topics/protocol/
-->

"For a Redis Server to understand what a client wants, the client needs to follow a protocol": [Redis Serialization Protocol](https://redis.io/docs/latest/develop/reference/protocol-spec/)

RESP supports common datatypes ("Redis is an extremely simple database")
* int (:)
* string (+), bulk string ($)
* arrays (*)
* a way to convey errors (-)

Serializing an array of strings using RESP: PUT K V ["PUT", "K", "V"] 

RESP is a request-response protocol (client sends request in RESP, server sends response in RESP)

Datatype starts with special character and ends with \r\n (CLRF)

Examples:
* Simple string: +PONG\r\n\ (very efficient, very low memory overhead, requires n+3 bytes in response)
* Integer: :1729\r\n
* Bulk strings: $4\r\n\PONG\r\n\ (PONG is 4 bytes, binary safe, see explanation)
* Array (see explanation)
* Errors: -Key not found\r\n

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

RESP highlights:
* Human readable
* Simple, fewer bugs
* Performant
* Uses prefixed lengths (from beginning, it knows exactly how many bytes to read, even for arrays, memory optimization)

Why not use JSON
* They are very "bulky"

[Implementing Redis' Wire Protocol - RESP](https://youtu.be/wnHZHGc8tW8?si=025wLUAUFmtE4B7q)

[Implementing Redis' PING Command](https://youtu.be/70UJ1QTQj5Y?si=foFDCG3F1he8pTkB)

[Event Loops Internals And How Redis Handles Multiple Connects on a Single Thread](https://youtu.be/U2IFZ_hw91o?si=VzZ55CHC4z1cf0hc)

[Implementing Event Loops](https://youtu.be/SMmLVHgE4pM?si=zBAklRoupJpBKkLA)

[Implementing GET, SET, and TTL Commands in Redis](https://youtu.be/1rnEpo56LFk?si=mQ_wlfl0z_aDEMam)

[Implementing DEL, EXPIRE, and Cleanup in Redis](https://youtu.be/yGdk0hmvkgo?si=yDESzMspeziXVFht)

[Key Eviction Strategies in Redis and Implementing Simple First Eviction](https://youtu.be/EkaTFT9ox-I?si=a_JL5bK2PUm4d-Tf)

<!--
https://valkey.io/topics/lru-cache/
https://redis.io/docs/latest/develop/reference/eviction/
https://docs.digitalocean.com/products/databases/valkey/how-to/choose-eviction-policies/
-->

Keys Eviction

Redis is RAM-bound. We cannot have unbounded storage. To accomodate newer data, we need to evict older data. 

When reach limit and RAM is full, if insert large number of keys, and not enough RAM to allocate, process would crash. Out-of-memory error. 1 GB limit and exhuasted. 

How Redis evicts?
maxmemory configuration that limits the max amount of data it can hold. When it reaches, it evicts old data. 

Some common strategies
* noeviction- new values aren't saved when memory limit is reached
* allkeys-lru- removes least recently used keys (recommended in Digital Ocean Valkey docs)
* allkeys-lfu- removes least frequently used keys
* volatile-lru- removes LRU keys with EXPIRE set
* volatile-lfu- removes LFU keys with EXPIRE set
* allkeys-random- picks some keys at random and evicts them (uniform access across keys)
* volatile-random- picks some keys with EXPIRE set at random to evict (uniform access across keys)
* volatile-ttl- picks key with shortest TTL and evicts it

Use lfu when keys that are used often need to be kept in memory. 

Approximated LRU (Redis 3.0)

Redis LRU is approximated, not an exact algorithm. Satisficing, very memory-efficient. 

Core idea: take "n" samples of "k" keys, evict LRU

Exact LRU requires extra memory. Which key accessed when? Imagine a DLL (double linked list), additional memory overhead of maintaining pointers is overkill. 

Redis uses that memory for data instead of pointers. 

New LFU Mode (Redis 4.0)

Storing a huge integer value is costly. An integer for every key (0-1 million). Requires 4 bytes across all keys. Every time we get or update, it does frequency++

Approximately instead

Morris Counter- does not use regular integer and frequency++, it is space-efficient counting

You can store 1 million value in 2 or 3 bytes. 

frequency is not ever-increasing. Decay over time reduces value. 
* Saturate counter at 1 million requests
* Decay the value every 1 minute
* You can configure this in redis.conf file

Even if dip in key access, it should not be evicted? Comparison to stock market. 

<!--
How probabilitistic data structures are build
See research paper
https://arxiv.org/abs/2010.02116
https://arpitbhayani.me/blogs/morris-counter
-->

[Implementing Command Pipelining](https://youtu.be/2q7RuEb9z-M?si=SYLiGS1yRsKAfXX5)

[Implementing AOF Persistence](https://youtu.be/5SnkVoatBpA?si=iisw--vzrqNDbCrO)

[Object, Encodings, and Implementing INCR](https://youtu.be/Dl9h6sQAFfk?si=VAM1SrLDVxqAin64)

[Implementing INFO and allkeys-random Eviction](https://youtu.be/jmN3VRsUmRs?si=OhqFJnW29r0OH4w-)

[The Approximated LRU Algorithm](https://youtu.be/0kOCyjFKKL4?si=cszXqNpgAbnJI3xl)

[Implementing the Approximated LRU Algorithm](https://youtu.be/FvGGkzcKKFA?si=shVnUABoIb8DBT8t)

[How Redis Caps Its Memory Usage](https://youtu.be/xNfoeZuK3Mc?si=RrX-BRTiPInpC2bR)



