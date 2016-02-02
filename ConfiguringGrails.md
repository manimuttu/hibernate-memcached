# Just Too Easy #
Configuring your grails application to use the hibernate-memcached library is incredibly simple. You basically follow the [Configuration](Configuration.md) instructions but do it using Groovy in your grails-app/conf/Datasource.groovy file.

The absolute basic configuration looks like...
```
hibernate {
    cache.use_second_level_cache = true
    cache.use_query_cache = true
    cache.provider_class = 'com.googlecode.hibernate.memcached.MemcachedCacheProvider'
    memcached {
        servers = "localhost:11211"
    }
}
```

This is just like the [Configuration](Configuration.md) instructions really. Instead of saying "hibernate.memcached.servers=localhost:11211" we can nest it in blocks using Groovy. Instead of repeating "hibernate.memcached" every time you add a value you can just group them all up in the memcached block. Nice and easy :)