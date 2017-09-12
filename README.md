# Zipkin Cassandra example

This is example of Zipkin server using Cassandra storage. This example 
also uses Span job [zipkin-depencies](https://github.com/openzipkin/zipkin-dependencies) 
for processing dependency links.

Create Cassandra and run Zipkin server:
```bash
ccm create test -v 2.2.10 -n 1 -s
./mvnw clean install
java -jar target/zipkin-cassandra-0.0.1-SNAPSHOT.jar
```

Get `zipkin-dependencies` binary and run it:
```bash
wget -O zipkin-dependencies.jar 'https://search.maven.org/remote_content?g=io.zipkin.dependencies&a=zipkin-dependencies&v=LATEST'
SPARK_LOCAL_IP="127.0.0.1" STORAGE_TYPE=cassandra CASSANDRA_KEYSPACE=zipkin3 java -jar zipkin-dependencies.jar `date -u -d '1 day ago' +%F`
```

Verify Zipkin schema in Cassandra:
```bash
ccm node1 cqlsh 
select * from system.schema_keyspaces;
```
