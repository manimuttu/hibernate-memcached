# What is a dogpile? #
Let's say you have 50 instances of your application running. Each instance is using 200 threads to process requests. Your probably not using 200 threads actively, so let's call it 100 active threads `*` 50 instances. That means you could be handling 500 simultaneous requests. Now let's say that you have a query that is very heavily used, maybe it's data for the home page, maybe it's data for a stock ticker bar, whatever. You, like a good engineer, are caching that data using memcached to avoid pounding on your database.

What happens when memcached expires the data in the cache? Well if 10, 20, or even 50 of your request threads are just coming alive at that point they will all see (at the same time) a cache-miss for this data. Each one of those threads will do the responsible thing and try and go get the data from the database to update the cache. And now, you have a dogpile on your database.

# How can I stop it? #
Well, we can't really stop it without adding some massive synchronized lock that all your request threads have to wait for. That would be a massively un-scalable way to do it, but it sure would stop dogpiles from happening.

One simple thing to do is try to reduce the amount of time that request threads have to catch the cache without data. The window we need to reduce is the amount of time it takes for one thread to see the data is missing, load it, and put it in the cache. The hibernate-memcached library does this by lying to hibernate. :)

When the _hibernate.memcached.dogpilePrevention_ property is set to _true_ hibernate-memcached will cache two pieces of information. One is a small token in the cache based on your "real" expected cache time, the second is the actual data you want to cache which is cached for slightly longer. When the "dogpile token" is null we simply turn around and put a new one in immediately and return a null to hibernate. This way we reduce the window of opportunity for the dogpile to almost nothing and hibernate sees a null from the cache so it in turn loads the data and updates our cache.

# Configuration #
| **Property** | **Type** | **Default** |
|:-------------|:---------|:------------|
| hibernate.memcached.dogpilePrevention | boolean  | false       |
| hibernate.memcached.dogpilePrevention.expirationFactor | integer  | 2           |

Dogpile prevention is obviously disabled by default, enable it by setting hibernate.memcached.dogpilePrevention to true. The expirationFactor is used to multiply the normal cacheSeconds when storing the "real" data. The dogpile token is stored for the normal amount of time as specified by the cacheSeconds property in the [Configuration](Configuration.md) page.