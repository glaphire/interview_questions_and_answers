# Basic Data Types

1. [Binary-safe strings](#strings)
2. [Lists](#lists)
3. [Sets](#sets)
4. [Sorted sets](#sorted-sets)
5. [Hashes](#hashes)
6. [Bit Arrays](#bit-arrays)

## Intro

Key is always a string. Key is a binary-safe string.

Value might be the one of the listed types.

Empty string is a valid key.

Maximum key length is 512 MB.

### Strings

Simple string that holds a value. Max length 512 MB

Useful cases:
- caching rendered html pages
- caching serialized configs

Simplest example:
```
> set mykey somevalue
OK
> get mykey
"somevalue"
```

Example with expiration (in seconds):
```
> set key some-value
OK
> expire key 5
(integer) 1
> get key (immediately)
"some-value"
> get key (after some time)
(nil)
```

Time complexity:
- GET - O(1)
- SET - O(1)
- DEL - O(1)
- STRLEN - O(1)

### Lists

Lists in Redis are implemented as **linked lists**.

Useful examples:
- Queues
- Stacks
- Cyclic operations (via RPOPLPUSH)
- retrieving latest events (e.g. newsfeed, logs)

Simplest example:

```
> rpush mylist A
(integer) 1
> rpush mylist B
(integer) 2
> lpush mylist first
(integer) 3
> lrange mylist 0 -1
1) "first"
2) "A"
3) "B"
```

Time complexity:
- LPUSH/RPUSH - O(1) for each element added
- LPOP/RPOP - O(N) where N is the number of elements returned
- LRANGE - O(S+N) where S is the distance of start offset from HEAD for small lists, from nearest end (HEAD or TAIL)
O(1) for each element addedfor large lists; and N is the number of elements in the specified range.
- LINSERT - O(N) where N is the number of elements to traverse before seeing the value pivot.
- LINDEX -  O(N) where N is the number of elements to traverse to get to the element at index.
- LREM - O(N+M) where N is the length of the list and M is the number of elements removed.
- LMOVE - O(1)

### Sets

Redis Sets are unordered collections of strings.
Sets were introduced earlier than Hashes. Hashes are useful for more complex data than sets and are more space-efficient.

Useful examples:
- keeping relations ids';
- getting random member of collection;
- check that element is member of a collection;
- performing sets operations - intersection, union, difference.

Simplest example:
```
> sadd news:1000:tags 1 2 5 77
(integer) 4

> sadd tag:1:news 1000
(integer) 1
> sadd tag:2:news 1000
(integer) 1
> sadd tag:5:news 1000
(integer) 1
> sadd tag:77:news 1000
(integer) 1

> smembers news:1000:tags
1. 5
2. 1
3. 77
4. 2
```

Time complexity:
- SADD - O(N), N - number of elements
- SPOP - O(N), N - number of elements
- SISMEMBER - O(1)
- SINTER - O(N*M) worst case where N is the cardinality of the smallest set and M is the number of sets.
- SCARD - O(1)
- SRANDMEMBER - O(N), N - number of elements
- SREM - O(N), N - number of elements

### Sorted sets

Ordered sets are key-value like arrays. The keys are the floating point values called **scores**.

Useful examples:
- getting range of values.
(TODO: finish answer)

Simplest example:

Time complexity:

### Hashes

Hash in Redis is an array of values.

Useful examples:
- keeping objects' values
- keeping comments on page
- when using json or serialization is overhead

Simplest example:
```
> hmset user:1000 username antirez birthyear 1977 verified 1
OK
> hget user:1000 username
"antirez"
> hget user:1000 birthyear
"1977"
> hgetall user:1000
1) "username"
2) "antirez"
3) "birthyear"
4) "1977"
5) "verified"
6) "1"
```

Time complexity:
- HSET - O(N) where N is the number of fields being set.
- HGET - O(N) where N is the number of fields being requested.
- HLEN - O(1)
- HKEYS - O(N)
- HEXISTS - O(1)
- HDEL - O(N)
- HSCAN - O(1)
- HSTRLEN - O(1)

### Bit Arrays

Useful examples:

Simplest example:

Time complexity: