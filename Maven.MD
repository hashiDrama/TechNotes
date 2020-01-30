##Installation of dependency present in a local path##

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
    <generatePom>false</generatePom>
    <pomFile>${basedir}/dependencies/someartifact-1.0.pom</pomFile>
 </configuration>
 <executions>
    <execution>
       <id>install-jar-lib</id>
       <goals>
          <goal>install-file</goal>
       </goals>
       <phase>validate</phase>
    </execution>
 </executions>
</plugin>
```
Note, here the name of `.pom` file should be same as the name of the jar. Also, the name of jar should be `<artifactId>-<version>.jar`.
