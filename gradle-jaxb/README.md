# gradle-xjb-xjc
Gradle with xjb and xjc examples

I just followed the examples from https://github.com/jacobono/gradle-jaxb-plugin , looks like this plugin is official added to gradle.
all you have to do is just added the plugin and declare the xjb and use the xjc to compile and convert the xsd to java files. its pretty simple.


To run the script.
gradle xjc

$ gradle xjc
:xsd-dependency-tree
jaxb: starting Namespace Task
There is no targetNamespace attribute for file '/home/murali/GITHUB/gradle-xjb-xjc/gradle-jaxb/schema/schema.xsd' (assigning filname as namespace to it). A Schema should ALWAYS include a targetNamespace attribute at its root element.  No targetNamespace are categorized as using the Chameleon Design Pattern, which is not an advisable pattern, AT ALL!
jaxb: generating xsd namespace dependency tree
:xjc
Download https://repo1.maven.org/maven2/com/sun/xml/bind/jaxb-xjc/2.2.7-b41/jaxb-xjc-2.2.7-b41.pom
Download https://repo1.maven.org/maven2/com/sun/xml/bind/jaxb-impl/2.2.7-b41/jaxb-impl-2.2.7-b41.pom
Download https://repo1.maven.org/maven2/javax/xml/bind/jaxb-api/2.2.7/jaxb-api-2.2.7.pom
Download https://repo1.maven.org/maven2/com/sun/xml/bind/jaxb-xjc/2.2.7-b41/jaxb-xjc-2.2.7-b41.jar
Download https://repo1.maven.org/maven2/com/sun/xml/bind/jaxb-impl/2.2.7-b41/jaxb-impl-2.2.7-b41.jar
Download https://repo1.maven.org/maven2/javax/xml/bind/jaxb-api/2.2.7/jaxb-api-2.2.7.jar

BUILD SUCCESSFUL

Total time: 3 mins 44.635 secs

This build could be faster, please consider using the Gradle Daemon: https://docs.gradle.org/2.9/userguide/gradle_daemon.html




