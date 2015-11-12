contrast-rO0
========

A lightweight Java agent for preventing attacks against object deserialization
like those discussed in [0] and [1], affecting WebLogic, JBoss, Jenkins and
more.

## Why did you make this?
This is the only way to hotpatch your application against this vulnerability. 
Patching the code is possible, but for many applications, patches will not be
available for a long time. Because of the nature of the vulnerability, some
applications will have to re-architect their messaging completely.

## How do I use the agent?
Add the following JVM option to your server or application:
```
-javaagent:/path/to/contrast-rO0.jar
```

## What does it do?
If the agent is enabled, it will throw a SecurityException if any of the following classes
are attempted to be deserialized. These classes represent the "last mile" of the exploit 
chain in the only publicly known proofs-of-concept, and are extremely unlikely to be used
for legitimate purposes during deserialization. 

* org.apache.commons.collections.functors.InvokerTransformer
* org.apache.commons.collections4.functors.InvokerTransformer
* org.apache.commons.collections.functors.InstantiateTransformer
* org.apache.commons.collections4.functors.InstantiateTransformer
* org.codehaus.groovy.runtime.ConvertedClosure
* org.codehaus.groovy.runtime.MethodClosure
* org.springframework.beans.factory.ObjectFactory

Though there likely exists other exploitable classes, they are difficult to find, and 
likely won't be part of any mass exploitation tool for a while.

## Supported systems
Although it's not tested on them all, the agent should work great on the following platforms:
* Java 5-8
* OpenJDK/HotSpot, JRockit, IBM