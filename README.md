# Run non-web Java processes on Heroku

## Sample Application

A simple app that demonstrates the one-off and worker process types can be created as two simple Java classes and a build file:

    sampleapp/
      pom.xml
      src/
        main/
          java/
            OneOffProcess.java

### src/main/java/OneOffProcess.java

    public class OneOffProcess 
    {
        public static void main(String[] args)
        {
            System.out.println("OneOffProcess executed.");
        }    
    }

    
### pom.xml

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.example</groupId>
  <artifactId>herokujavasample</artifactId>
  <version>1.0-SNAPSHOT</version>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <build>
    <plugins>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
          <artifactId>appassembler-maven-plugin</artifactId>
          <version>2.1.0</version>
          <configuration> 
            <assembleDirectory>target</assembleDirectory> 
            <programs>
                <program>
                    <mainClass>OneOffProcess</mainClass>
                    <name>oneoff</name>
                </program>
            </programs>
          </configuration>
          <executions>
              <execution>
                  <phase>package</phase><goals><goal>assemble</goal></goals>
              </execution>            
          </executions>
      </plugin>
    </plugins>
  </build>  

</project>

The app assembler plugin generates a  launch script for starting your application. A single `pom.xml` can define multiple web, worker or admin processes. 

## Run Locally

To build the application run:

    :::term
    $ mvn package

    ...

(use `target\bin\oneoff.bat` on Windows). Run the one-off process with:

    :::term
    $ sh target/bin/oneoff
    OneOffProcess executed.
    

## Deploy to Heroku


## One-Off processes

If your process is a one-off admin process that you wish to run manually on an as needed basis you can do so with the `heroku run` command:

    :::term
    $ heroku run "sh target/bin/oneoff"
    Running sh target/bin/oneoff attached to terminal... up, run.1
    OneOffProcess executed.

