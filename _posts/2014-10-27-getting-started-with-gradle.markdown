---
layout: post
title:  "Getting started with Gradle"
date:   2014-10-27 00:28:00
categories: gradle java jar
comments: yes
description: "Getting started with Gradle. Basic Gradle project with dependiencies"
---

Gradle is a project automation tool that builds upon the concepts of Ant and Maven but instead of using XML to write the build script it uses a Groovy-based DSL

Gradle can automate the building, testing, publishing, deployment and a lot more. 

Having Groovy based configuration makes it easier to write custom tasks in language more like what programmers are used to.

But lets dive into a quick tutorial of using Gradle to build a Java project. This simple project will use library to get a custom Chuck Norris Joke and print it to the console.

Installation
--------

First thing you have to do it to install Gradle. Currently Gradle 2.1 is the newest version and I recommend using that version. The following are examples of how you can install Gradle (there are lot of other ways this can be done)

####Windows####
Download latest binaries from http://www.gradle.org/

####Mac OS X####
Using Homebrew (make sure homebrew is up to date)
{% highlight sh %}
brew install gradle 
{% endhighlight %}

####Linux####
Download latest binaries from http://www.gradle.org/ or using package manager. Following is an example for Debian/Ubuntu based distributions to get the latest version of Gradle since many distributions have old versions.

{% highlight sh %}
sudo add-apt-repository ppa:cwchien/gradle
sudo apt-get update
sudo apt-get install gradle
{% endhighlight %}

To verify that the installation worked you can call gradle from the command line

{% highlight sh %}
gradle -v
{% endhighlight %}

This should return the version of Gradle installed (and some other information about the Gradle install). If this command returned error make sure that you have GRADLE_HOME/bin folder in your PATH environment variable. If your installation still does not work then consult the <a href="http://www.gradle.org/docs/2.1/userguide/installation.html" target="_blank">installation guide</a>

Example output: 
<pre>
------------------------------------------------------------
Gradle 2.1
------------------------------------------------------------

Build time:   2014-09-08 10:40:39 UTC
Build number: none
Revision:     e6cf70745ac11fa943e19294d19a2c527a669a53

Groovy:       2.3.6
Ant:          Apache Ant(TM) version 1.9.3 compiled on December 23 2013
JVM:          1.7.0_51 (Oracle Corporation 24.51-b03)
OS:           Mac OS X 10.9.5 x86_64
</pre>

Creating a project
-------------

With newer version of Gradle creating a new project with everything you need is very easy. For our example we want to create a Java project so we use the following command

{% highlight sh %}
gradle init --type java-library
{% endhighlight %}

This creates the basic project structure that we need. There are other types of projects like groovy, scala and basic. There are plans to add other types in future releases of Gradle. For more details see <a href="http://www.gradle.org/docs/current/userguide/build_init_plugin.html" target="_blank">http://www.gradle.org/docs/current/userguide/build_init_plugin.html</a>

After running the init command for java project we have basic structure for Gradle project. The files and folders created are the following: 

- **build.gradle** The gradle build file
- **gradle** Folder containing the gradle wrapper (see <a href="http://www.gradle.org/docs/current/userguide/gradle_wrapper.html" target="_blank">http://www.gradle.org/docs/current/userguide/gradle_wrapper.html</a>)
- **gradlew** Linux/Unix script to run the gradle wrapper
- **gradlew.bat** Windows script to run the gradle wrapper
- **settings.gradle** Settings file for gradle, not used in this example, can for example contain definitions if we have multiple projects
- **src** Source folder for our code and tests


Basic Gradle tasks
---------------

Not since we created a java project we have by default a lot of java related tasks. Lets checkout the list of **gradle tasks** (./gradlew tasks if we use the wrapper).

<pre>
-----------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
classes - Assembles classes 'main'.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes.
testClasses - Assembles classes 'test'.

Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]

Documentation tasks
-------------------
javadoc - Generates Javadoc API documentation for the main source code.

Help tasks
----------
components - Displays the components produced by root project 'tmp'.
dependencies - Displays all dependencies declared in root project 'tmp'.
dependencyInsight - Displays the insight into a specific dependency in root project 'tmp'.
help - Displays a help message
projects - Displays the sub-projects of root project 'tmp'.
properties - Displays the properties of root project 'tmp'.
tasks - Displays the tasks runnable from root project 'tmp'.

Verification tasks
------------------
check - Runs all checks.
test - Runs the unit tests.
</pre>

This list gives us good idea about the tasks that grade can run with only the default java setup. If you add plugins or write custom tasks they will be added to this list.
Note that is does not show us complete lists of available tasks, to get complete list use **gradle tasks -all**



Add code
-----------
Now lets start by creating a simple project. Navigate to src/main/java. There you can see that Gradle has created an example class called Library.java, you can delete this class (or keep it in place if you like). Navigate to scr/test/java and there gradle created LibraryTest.java example file. You can also delete this class (and you need to if you deleted Library.java).

Again navigate to src/main/java and add folders net/joningi/helloworld  (on unix based platform you can use mkdir -p net/joningi/helloworld) Navigate to the helloworld folder and create a new file called HelloWorld.java. Add the following code to the file.

{% highlight java %}
package net.joningi.helloworld;

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello world");
    }
}
{% endhighlight %}

Now from the project root run ** gradle check ** command to build and test the project. You should see the following output if everything is as it should

<pre>
:compileJava
:processResources UP-TO-DATE
:classes
:compileTestJava
:processTestResources UP-TO-DATE
:testClasses
:test
:check

BUILD SUCCESSFUL
</pre>


Now to be able to run the program using Gradle we add the application plugin. Open the **build.gradle** file in your favorite editor add locate the **apply plugin: 'java'** line and just below it add the following lines:

{% highlight groovy %}
apply plugin: 'application'

mainClassName = "net.joningi.helloworld.HelloWorld"
{% endhighlight %}

This adds the <a href="http://www.gradle.org/docs/current/userguide/application_plugin.html" target="_blank">application plugin</a> to our project.

Now you can use **gradle run** command to run your project. Try it out and it should output **Hello world**




Testing
---------

Now lets create unit tests and see how gradle handles testing. To make our project more testable lets create a new class under the helloworld directory, lets call it **World.java**. Add the following code to the file.

{% highlight java %}
package net.joningi.helloworld;

public class World {
    public String greet() {
        return "Hello world!";
    }
}
{% endhighlight %}

And now lets change our **Helloworld.java** file to be like this:

{% highlight java %}
package net.joningi.helloworld;

public class HelloWorld {
    public static void main(String[] args) {
        World world = new World();
        System.out.println(world.greet());
    }
}
{% endhighlight %}

Now we have extracted the "Hello world" message out of the class and can unit test the World class.


Navigate to src/test/java and  create the following folders net/joningi/helloworld  (again on unix based platform you can use mkdir -p net/joningi/helloworld).
Now go to the helloworld folder and add a file called **WorldTest.java**.
Lets add a single test to check if the string we get when we call the greet() method is **Hello world**

{% highlight java %}
package net.joningi.helloworld;

import static org.junit.Assert.assertEquals;

import org.junit.Test;

public class WorldTest {

    @Test
    public void greetResultsInHello() {
        World world = new World();
        assertEquals("Hello world!", world.greet());
    }

}
{% endhighlight %}

Now from the project root run the new unit test using **gradle test**

You should see the following output.
<pre>
:compileJava UP-TO-DATE
:processResources UP-TO-DATE
:classes UP-TO-DATE
:compileTestJava UP-TO-DATE
:processTestResources UP-TO-DATE
:testClasses UP-TO-DATE
:test UP-TO-DATE

BUILD SUCCESSFUL
</pre>

As an excersie, change the test and see the build fail.


Add Internet Chuck Norris db java api library to dependencies
--------------
Now to the fun part. We will get a glimpse of how Gradle handles dependincies by adding dependency to a java api library that connects to the Internet Chuck Norris database.

Navigate to <a href="http://search.maven.org/" target="_blank">http://search.maven.org/</a> and search for icndb-java-api. That should return a single search result (at the time of this writing). Now click on the version number. This brings up view where you can select to get the dependency string in multiple formats. For our example we click on "Gradle/Grails" and that returnes the following string

{% highlight groovy %}
compile 'net.joningi:icndb-java-api:1.0'
{% endhighlight %}

Here we can see that we are about to add dependency on version 1.0 of this package, and that it has group id net.joningi and artifact id is icndb-java-api.

Now open up the build.gradle file and look for the following part and add your new depenency

{% highlight groovy %}
// In this section you declare the dependencies for your production and test code
dependencies {
    // The production code uses the SLF4J logging API at compile time
    compile 'org.slf4j:slf4j-api:1.7.5'

    // This is our new depenency on the chuck norris database.
    compile 'net.joningi:icndb-java-api:1.0'

    // Declare the dependency for your favourite test framework you want to use in your tests.
    // TestNG is also supported by the Gradle Test task. Just change the
    // testCompile dependency to testCompile 'org.testng:testng:6.8.1' and add
    // 'test.useTestNG()' to your build script.
    testCompile 'junit:junit:4.11'
}
{% endhighlight %}


Adding only this single line makes this library available to us in our code. Gradle downloads everything that we need, the dependency itself and all other dependencies that it relies on. Doing this without a tool like Gradle can be a lot of work, expecially when you have a long list of dependencies and they depend og different verions of the same libraries. The following is a list of libraries that the icndb lib depends on, and you can see that it's very nice to have Gradle manage this for us.

<pre>
+--- net.joningi:icndb-java-api:1.0
|    +--- com.google.guava:guava:16.+ -> 16.0.1
|    +--- com.google.code.gson:gson:2.+ -> 2.3
|    \--- org.glassfish.jersey.core:jersey-client:2.+ -> 2.13
|         +--- org.glassfish.jersey.core:jersey-common:2.13
|         |    +--- javax.ws.rs:javax.ws.rs-api:2.0.1
|         |    +--- javax.annotation:javax.annotation-api:1.2
|         |    +--- org.glassfish.jersey.bundles.repackaged:jersey-guava:2.13
|         |    +--- org.glassfish.hk2:hk2-api:2.3.0-b10
|         |    |    +--- javax.inject:javax.inject:1
|         |    |    +--- org.glassfish.hk2:hk2-utils:2.3.0-b10
|         |    |    |    \--- javax.inject:javax.inject:1
|         |    |    \--- org.glassfish.hk2.external:aopalliance-repackaged:2.3.0-b10
|         |    +--- org.glassfish.hk2.external:javax.inject:2.3.0-b10
|         |    +--- org.glassfish.hk2:hk2-locator:2.3.0-b10
|         |    |    +--- org.glassfish.hk2.external:javax.inject:2.3.0-b10
|         |    |    +--- org.glassfish.hk2.external:aopalliance-repackaged:2.3.0-b10
|         |    |    +--- org.glassfish.hk2:hk2-api:2.3.0-b10 (*)
|         |    |    +--- org.glassfish.hk2:hk2-utils:2.3.0-b10 (*)
|         |    |    \--- org.javassist:javassist:3.18.1-GA
|         |    \--- org.glassfish.hk2:osgi-resource-locator:1.0.1
|         +--- javax.ws.rs:javax.ws.rs-api:2.0.1
|         +--- org.glassfish.hk2:hk2-api:2.3.0-b10 (*)
|         +--- org.glassfish.hk2.external:javax.inject:2.3.0-b10
|         \--- org.glassfish.hk2:hk2-locator:2.3.0-b10 (*)
</pre>




The next thing is to change our **Helloworld.java** to the following


{% highlight java %}
package net.joningi.helloworld;

import net.joningi.icndb.ICNDBClient;
import net.joningi.icndb.Joke;

public class HelloWorld {
    public static void main(String[] args) {
        ICNDBClient client = new ICNDBClient();

        Joke joke = client.getById(232);
        return joke.getJoke();
    }
}
{% endhighlight %}


Gradle downloads the icndb librari for us so now you can just use the run command from the application framework. 

{% highlight java %}
gradle run
{% endhighlight %}

This will return the following.

{% highlight sh %}
Chuck Norris uses a night light. Not because Chuck Norris is afraid of the dark, but the dark is afraid of Chuck Norris.
{% endhighlight %}


Summing up
---------

This was a very quick overview of what Gradle can do for us. For more information look at my other posts here or go to the <a href="http://www.gradle.org/docs/current/userguide/userguide.html" target="_blank">Gradle userguide</a>
