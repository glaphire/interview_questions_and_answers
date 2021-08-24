# Basic Data Types

1. [Binary-safe strings](#some-markdown-heading)
2. Lists
3. Sets
4. Sorted sets
5. Hashes
6. Bit Arrays

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
- LRANGE - O(S+N) where S is the distance of start offset from HEAD for small lists, from nearest end (HEAD or TAIL) for large lists; and N is the number of elements in the specified range.
- LINSERT - O(N) where N is the number of elements to traverse before seeing the value pivot.
- LINDEX -  O(N) where N is the number of elements to traverse to get to the element at index.
- LREM - O(N+M) where N is the length of the list and M is the number of elements removed.
- LMOVE - O(1)

### Sets

Useful examples:

Simplest example:

### Sorted sets

Useful examples:

Simplest example:

### Hashes

Useful examples:

Simplest example:

### Bit Arrays

Useful examples:

Simplest example: