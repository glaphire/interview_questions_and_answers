# Memcache VS Memcached

_Memcache module in PHP is for Memcached._
_Memcached module in PHP is for LibMemcached (API for Memcache)._


## Memcache

**Memcache** module provides handy procedural and object 
oriented interface to memcached, highly effective caching daemon, 
which was especially designed to decrease database load 
in dynamic web applications.

The Memcache module also provides a session handler (memcache).

[Official documentation for Memcached](http://www.memcached.org/).


## Memcached

**Memcached** is a high-performance, distributed memory object 
caching system, generic in nature, but intended for use 
in speeding up dynamic web applications by alleviating 
database load.

This extension uses the *libmemcached library* to provide an API 
for communicating with memcached servers. It also provides 
a session handler (memcached).

[Official documentation for Libmemcached](http://libmemcached.org/libMemcached.html)

## Differences

These two PHP extensions are not identical. 
PHP Memcache is older, very stable but has a few limitations. 
**The PHP memcache module utilizes the daemon directly 
while the PHP memcached module uses the libMemcached client library and also contains some added features.**

<table border=1 cellpadding=5>
    <tr>
        <th> </th>
        <th>[http://pecl.php.net/package/memcache pecl/memcache]</th>
        <th>[http://pecl.php.net/package/memcached pecl/memcached]</th>
    </tr>
    <tr>
        <th align="left">First Release Date</th>
        <td align="center">2004-06-08</td>
        <td align="center">2009-01-29 (beta)</td>
    </tr>
    <tr>
        <th align="left">Actively Developed?</th>
        <td align="center">Yes</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">External Dependency</th>
        <td align="center">None</td>
        <td align="center"><a href="http://tangent.org/552/libmemcached.html">libmemcached</a></td>
    </tr>
    <tr>
        <td colspan=3 align="center"><strong>Features</strong></td>
    </tr>
    <tr>
        <th align="left">Automatic Key Fixup<sup>1</sup></th>
        <td align="center">Yes</td>
        <td align="center">No</td>
    </tr>
    <tr>
        <th align="left">Append/Prepend</th>
        <td align="center">No</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">Automatic Serialzation<sup>2</sup></th>
        <td align="center">Yes</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">Binary Protocol</th>
        <td align="center">No</td>
        <td align="center">Optional</td>
    </tr>
    <tr>
        <th align="left">CAS</th>
        <td align="center">No</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">Compression</th>
        <td align="center">Yes</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">Communication Timeout</th>
        <td align="center">Connect Only</td>
        <td align="center">Various Options</td>
    </tr>
    <tr>
        <th align="left">Consistent Hashing</th>
        <td align="center">Yes</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">Delayed Get</th>
        <td align="center">No</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">Multi-Get</th>
        <td align="center">Yes</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">Session Support</th>
        <td align="center">Yes</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">Set/Get to a specific server</th>
        <td align="center">No</td>
        <td align="center">Yes</td>
    </tr>
    <tr>
        <th align="left">Stores Numerics</th>
        <td align="center">Converted to Strings</td>
        <td align="center">Yes</td>
    </tr>
</table>

[Table source: wiki](https://github.com/memcached/old-wiki/blob/master/PHPClientComparison.wiki)

## Pros & Cons (Memcached)
Pros: 
- Great scalability (because it's a separate service)
- Good for storing small objects (< 250 characters)
- Automatic serialization/deserialization for objects

Cons:
- Doesn't have atomic locks (original value loss due to 
overriding by several processes)
- Is bad for sessions in PHP (due to previous issue)
- Can't use "store forever" functionality - data will be lost 
if Memcached runs out of memory (automatic removal of stale data)
- Bad performance for large complex objects (serialization/deserialization costs)