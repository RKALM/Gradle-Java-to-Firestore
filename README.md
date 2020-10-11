
**Connection between a Gradle Java Instance and a Firestore Database** 

 - Ingemar Lundh 
 - Lavdim Imeri 
 - Robert Alm

Our project is based on the following guide:

https://firebase.google.com/docs/firestore/quickstart

For the project we are working on, we decided to use Firestore as our database and for convenience (we are java programmers), we decided to use Java as our programming language for the server.

The logical step for our efforts was to make sure we will achieve a handshake between our Java instance and the Firebase, but this has proven to be bit more difficult than it was expected.

Becuase we faced difficulties, as well as the kind of difficulties that we faced, we assumed that many other java programmers may face the same challenges so we had to share our findings with others.

We used intellij idea to build our code. In any other case we could have a simple java project, but in order to connect to Firestore the use of automation like "Gradle" or "Maven" is necessary and although Maven is more popular for that purpose, our familiarity with gradle influenced us to use gradle.

We created (using intellij idea) a gradle instance which is integrated with java and inside the depedencies cluster in the build.gradle together with the line:

    compile 'com.google.firebase:firebase-admin:7.0.0'

We added the following line as well.

    compile "org.slf4j:slf4j-simple:1.6.1"
    testCompile group: 'org.slf4j', name: 'slf4j-simple', version: '1.8.0-beta4'

To avoid exceptions related with SLF4J. Check the links to find more about the issue:
http://www.slf4j.org/codes.html#StaticLoggerBinder
https://stackoverflow.com/questions/7421612/slf4j-failed-to-load-class-org-slf4j-impl-staticloggerbinder

The link:
https://mvnrepository.com/artifact/org.slf4j/slf4j-simple
is a source for finding SLF4J libraries.

Another issue that we discovered was how to tackle the deprecated parts of the code. Those parts of the code, are getting improved easily and they don't cause any harm. The reason why we stuck with the code as it is was because it is closer to the initial instructions and we have a point of reference.

For more details about the issue with the deprecated code please visit the following link:
https://stackoverflow.com/questions/52772199/credentials-failed-to-obtain-metadata-error-when-trying-to-auth-java-client-to

(we had that error message but it wasn't about the deprecated code)

In our project we have a folder which is called "theKey"; inside we paste the generated key from our service account which is .json and we rename it as key.json
(this is so we don't have to struggle with the path each time; it is a gradle project)

The reason why we had the error message “Credentials failed to obtain metadata,” was because of the 5 minutes difference in the internal clock of the physical machine which the java instance was inside that made firestore to refuse to validate our credentials.
We fixed it by correcting the clock of the computer.

Together with anything else, small changes were added to the code, mostly libraries and exceptions that had to be added to make the whole system work. We also we had to modify the variables and the print statements a bit for reasons of convenience and for testing purposes.

**If you are planning to use a gradle java instance to get connected with a firestore database our advice is:**

1)Add the lines that we added in the dependencies on build.gradle

    dependencies {
        testCompile group: 'junit', name: 'junit', version: '4.12'
        compile 'com.google.firebase:firebase-admin:7.0.0'
        compile "org.slf4j:slf4j-simple:1.6.1"
        // https://mvnrepository.com/artifact/org.slf4j/slf4j-simple
        testCompile group: 'org.slf4j', name: 'slf4j-simple', version: '1.8.0-beta4'
    }

2)Have in mind that parts of the code are deprecated, that is the reason as well for the compile warnings that you get everytime you compile the project.

3)Have your eyes to the clock of your computer. if the time is off, your authentication will fail.

4)When you have a gradle project, open it in intellij idea. Trying to import will give you undesired results.
