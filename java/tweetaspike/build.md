### How to build
This example requires a working Java development environment (Java 6 and above) including Maven (Maven 2). The Aerospike Java client will be downloaded from Maven Central as part of the build.
After cloning the repository, use maven to build the jar files. From the root directory of the project, issue the following command:
```bash
mvn clean package
```
A JAR file will be produced in the directory `target`: `tweetaspike-example-application-1.0.0-full.jar`. This is a runnable jar complete with all the dependencies packaged

### Run
To run the example, you will specify the address of the cluster, and the set to delete. The following command connects to a cluster located at the server ‘192.168.1.15’, a namespace of 'test and a set named ‘demo’.
```bash
java -jar tweetaspike-example-application-1.0.0-full.jar.jar -h 192.168.1.15 -p 3000 -n test -s demo
```

