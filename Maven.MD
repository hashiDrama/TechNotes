# Installation of Maven project dependency present in a local path

Below steps explains how to install a dependency which is not available on ***Maven Central*** and not on any ***remote repository*** as well. You may find full reference for this information [here](https://www.baeldung.com/install-local-jar-with-maven/).

1. Place the jar in a local folder of the system. It is advisable to keep it in a location which comes inside the application root folder.
2. Place the pom.xml of this jar in the same folder.
3. Update `pom.xml` file of your application by adding below mentioned code.
```
<plugin>
 <groupId>org.apache.maven.plugins</groupId>
 <artifactId>maven-install-plugin</artifactId>
 <version>2.5.1</version>
 <configuration>
    <groupId>org.somegroup</groupId>
    <artifactId>someartifact</artifactId>
    <version>1.0</version>
    <packaging>jar</packaging>
    <file>${basedir}/dependencies/someartifact-1.0.jar</file>
 </configuration>
 <executions>
    <execution>
       <id>install-jar-lib</id>
       <goals>
          <goal>install-file</goal>
       </goals>
       <phase>clean</phase>
    </execution>
 </executions>
</plugin>
```
Note, here the name of `.pom` file should be same as the name of the jar. Also, the name of jar should be `<artifactId>-<version>.jar`.


# Steps to create a fat jar with Maven

A Fat JAR is a single JAR file that contains all the compiled Java classes from your project, and all compiled Java classes from all JAR files your project depends on. Fat JARs are also handy if you need to package your application inside a Docker container. Instead of having to add each JAR file your application depends on to the Docker container, you only have to add the Fat JAR of our application. Again, this is much shorter, easier, and it makes your Dockerfile independent of what external dependencies (JAR files) your application uses.  
Below steps should be followed in order to build a fat jar with maven.

1. **Fat jar pom configuration**  
You configure Maven to build a Fat JAR from your project by including the maven-assembly-plugin in your POM file's plugin section. Maven refers to an output product that it builds as an assembly. Hence the name maven-assembly-plugin. Here is an example snippet from a POM file showing an example of a maven-assembly-plugin configuration:
```
<build>
  <finalName>my-project-name</finalName>
  <plugins>

      <!-- other Maven plugins ... -->

      <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>3.1.1</version>

          <configuration>
              <descriptorRefs>
                  <descriptorRef>jar-with-dependencies</descriptorRef>
              </descriptorRefs>
          </configuration>

          <executions>
              <execution>
                  <id>make-assembly</id>
                  <phase>package</phase>
                  <goals>
                      <goal>single</goal>
                  </goals>
              </execution>
          </executions>

      </plugin>
  </plugins>
</build>
```
The configuration XML element inside the maven-assembly-plugin configuration contains the descriptorRef which tells Maven what type of assembly this configuration should build. The value jar-with-dependencies tells Maven to build a JAR file with dependencies which is another term for a Fat JAR.

The executions XML element tells Maven which Maven build phase and goal this Maven plugin should be executed during. The maven-assembly-plugin should always be executed during the package phase.

2. **Maven command to build fat jars**  
The Maven command to make Maven build the Fat JAR for your project is:
```
mvn clean package
```
When you execute Maven package phase with the maven-assembly-plugin configuration shown earlier, Maven will output a Fat JAR in the target directory, into which Maven outputs all of its other build products (e.g. compiled classes, generated JavaDocs etc.). The Fat JAR will be named like this:
```
my-project-name-jar-with-dependencies.jar
```
The part my-project-name of the above Fat JAR file name comes from the finalName XML element included in the top of the build XML element in the example shown earlier in this tutorial.
