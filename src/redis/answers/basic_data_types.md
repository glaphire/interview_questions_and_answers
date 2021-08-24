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

### Lists

### Sets

### Sorted sets

### Hashes

### Bit Arrays