# Get the code #
Get the code from [Git](http://git.or.cz/)

`git clone git://github.com/raykrueger/hibernate-memcached.git`
Alternatively, visit my [repository at git hub](http://github.com/raykrueger/hibernate-memcached/tree/master) and download the source in zip or tar.gz form.

# Install Maven 2 #
If you already have maven installed you can skip this. If you need maven2 you can get it from http://maven.apache.org/ (use 2.0.9 or better).
Extract Maven and add it to your path so that you can run mvn from the command line. If you can't figure this out, you're on your own.

# Build with Maven 2 #
`mvn install`

Please note that the unit tests expect to find a single memcached running at localhost:11211. If one is not running the unit tests will fail. To get around this if necessary run `mvn install -Dmaven.test.skip=true`.