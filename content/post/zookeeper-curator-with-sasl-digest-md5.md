---
title: "Zookeeper Curator with SASL Digest-MD5"
author: "amdelamar"
category: "code"
tags: ["java", "zookeeper", "security"]
draft: false
date: 2023-12-30T15:46:46-08:00
thumbnail: ""
banner: ""
bannerCaption: ""
description: "How to connect to Zookeeper with Apache Curator with SASL Digest-MD5 enabled."
---

I finished a [book](https://www.oreilly.com/library/view/zookeeper/9781449361297/) recently about [Apache Zookeeper](https://zookeeper.apache.org)
and was playing around with various coordination tasks (Leader Election, Locks, etc) on my own time.
I like to get hands-on when learning new technologies, so this book was a decent introduction to Zookeeper. But I feel there wasn't enough emphaisis around security and authentication.

I was specifically interested in [SASL](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) with Zookeeper, and thankfully there was two pages in Chapter 10 dedicated to SASL with [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol)). But no other mechanisms were mentioned. There wasn't any guidance on how to setup Kerberos either, just "We assume you have Kerberos installed". 

For an introduction book, wouldn't it be simpler to explain SASL with [Digest-MD5](https://en.wikipedia.org/wiki/Digest_access_authentication)? A simple username and password is much more user-friendly than starting up a Kerberos server.

So after much digging and trial-and-error. I'd like to share how I managed to setup Zookeeper with SASL Digest enabled, as well as reject any unauthenticated connections to Zookeeper, and finally, connect to it from a Java application using Apache Curator as the client.

&nbsp;

### Zookeeper Server with SASL Digest

The simplest way to get Zookeeper up and running in this modern age is with Docker.

```bash
docker run --name=zookeeper --net=host --rm zookeeper
```

This starts a standalone Zookeeper at `localhost:2181` with logs streaming to your terminal.
Its very useful for testing and playing around with the recipes that the book covers.

But we're going to need to create a few config files to enable SASL with Digest-MD5.

First, create a `zoo.cfg` with:

```conf
dataDir=/data
dataLogDir=/datalog
clientPort=2181
tickTime=2000
initLimit=5
syncLimit=2
server.1=localhost:2888:3888
authProvider.1=org.apache.zookeeper.server.auth.SASLAuthenticationProvider
enforce.auth.enabled=true
enforce.auth.schemes=sasl
sessionRequireClientSASLAuth=true
```

The `1` after `authProvider.` could be any whole number, like `0, 1, 2, ... 100`, so long as its unique among other auth providers configured.

The `sessionRequireClientSASLAuth` is important, as it forces all session connections with the server to require SASL.
Without this, Zookeeper confusingly still allows clients to connect without providing credentials, even if you've configured
an auth provider.

The rest of the configs are explained in Zookeeper's official documentation, but the above configs shown are the minimal set required
to run Zookeeper in standalone mode with SASL auth enabled.

Next, create a `jaas.conf` with:

```js
Server {
    org.apache.zookeeper.server.auth.DigestLoginModule required
    user_super="1234";
};
```

I haven't mentioned [JAAS](https://en.wikipedia.org/wiki/Java_Authentication_and_Authorization_Service) yet, but its an authentication interface that was introduced in Java 1.4 that relies on `.conf` files set via the Java system properties, enabling various kinds of pluggable authentication mechanisms, like Kerberos, Digest, Certificates, and more.

Note: The `user_` prefix is a must. The actual username is `super` and the password is `1234`, as an example. If we wanted to define more than one set of credentials we can add those now along side this one.

Tip: Be careful not to expose this file to unauthorized users.

Finally, start up a new Zookeeper with these files:

```bash
docker run --name=zookeeper --net=host --rm \
-e JVMFLAGS="-Djava.security.auth.login.config=/conf/jaas.conf" \
-v $(pwd)/zoo.cfg:/conf/zoo.cfg \
-v $(pwd)/jaas.conf:/conf/jaas.conf \
zookeeper
```

Note: The additional JVM flag for pointing to our `jaas.conf` is required. We've given the file to the Docker container, but we didn't tell Java where to find it.

If all goes well, you should see a page full of logging information. I've copied some of it here:

```
...
2023-12-31 00:43:36,021 [myid:] - INFO  [main:o.a.z.s.q.QuorumPeerConfig@177] - Reading configuration from: /conf/zoo.cfg
2023-12-31 00:43:36,024 [myid:] - INFO  [main:o.a.z.s.q.QuorumPeerConfig@440] - clientPortAddress is 0.0.0.0:2181
...
2023-12-31 00:43:36,033 [myid:1] - INFO  [main:o.a.z.s.ZooKeeperServerMain@123] - Starting server
...
2023-12-31 00:43:36,058 [myid:1] - INFO  [main:o.a.z.ZookeeperBanner@42] -   ______                  _                                          
2023-12-31 00:43:36,058 [myid:1] - INFO  [main:o.a.z.ZookeeperBanner@42] -  |___  /                 | |                                         
2023-12-31 00:43:36,058 [myid:1] - INFO  [main:o.a.z.ZookeeperBanner@42] -     / /    ___     ___   | | __   ___    ___   _ __     ___   _ __   
2023-12-31 00:43:36,058 [myid:1] - INFO  [main:o.a.z.ZookeeperBanner@42] -    / /    / _ \   / _ \  | |/ /  / _ \  / _ \ | '_ \   / _ \ | '__|
2023-12-31 00:43:36,058 [myid:1] - INFO  [main:o.a.z.ZookeeperBanner@42] -   / /__  | (_) | | (_) | |   <  |  __/ |  __/ | |_) | |  __/ | |    
2023-12-31 00:43:36,059 [myid:1] - INFO  [main:o.a.z.ZookeeperBanner@42] -  /_____|  \___/   \___/  |_|\_\  \___|  \___| | .__/   \___| |_|
2023-12-31 00:43:36,059 [myid:1] - INFO  [main:o.a.z.ZookeeperBanner@42] -                                               | |                     
2023-12-31 00:43:36,059 [myid:1] - INFO  [main:o.a.z.ZookeeperBanner@42] -  
...
2023-12-31 00:43:36,065 [myid:1] - INFO  [main:o.a.z.s.AuthenticationHelper@66] - zookeeper.enforce.auth.enabled = true
2023-12-31 00:43:36,065 [myid:1] - INFO  [main:o.a.z.s.AuthenticationHelper@67] - zookeeper.enforce.auth.schemes = [sasl]
...
2023-12-31 00:43:36,205 [myid:1] - INFO  [main:o.e.j.s.Server@415] - Started @515ms
...
```

That's it! Two files and a start command and you have yourself a minimal, yet secure, zookeeper server.

If you don't see the [sasl] scheme logged, double-check you're running the docker command in the same directory as the `jaas.conf` file we created.


&nbsp;

### Curator Client with SASL Digest

For our Java client, we'll be using what the book mentioned, [Apache Curator](https://curator.apache.org). Its a nice library for managing the Zookeeper session connections with retries wrapped around commands.
Depending on your build system, you'll want to import it like so:

Maven:

```xml
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-client</artifactId>
    <version>5.5.0</version>
</dependency>
```

Gradle:

```js
dependencies {
    compile 'org.apache.curator:curator-client:5.5.0'
}
```

SBT:

```scala
libraryDependencies ++= Seq(
    "org.apache.curator" % "curator-client" % "5.5.0"
)
```

Tip: If you're going to experiment with distributed coordination tasks like I did, you'll also be interested in the companion
libaries `curator-recipes` and `curator-test` to get going.

Now that we have our dependency, we can create our `Application.java` file with:

```java
import org.apache.curator.framework.CuratorFramework;
import org.apache.curator.framework.CuratorFrameworkFactory;
import org.apache.curator.retry.ExponentialBackoffRetry;
import org.apache.zookeeper.CreateMode;

public class Application {

    public static void main(String args[]) {

        // Zookeeper client
        CuratorFramework curator = CuratorFrameworkFactory.newClient(
            "127.0.0.1:2181",
            new ExponentialBackoffRetry(1000, 3)
        );
        System.out.println("Connecting to Zookeeper...");
        curator.start();

        // Test out our connection
        try {
            curator.create().creatingParentsIfNeeded().withMode(CreateMode.EPHEMERAL_SEQUENTIAL).forPath("/data/client-");
            System.out.println("Successfully connected to Zookeeper!");
        } catch (Exception e) {
            System.out.println("Failed to connect to Zookeeper.");
            System.exit(1);
        }
    }

}
```

This works without SASL enabled on both the server and client. But that's not what we want. We need to give our client
the SASL Digest configuration it needs to connect with `super` user and a password.

There are two ways to setup your Java application to use SASL via JAAS.

The first way, involves creating a JAAS client `.conf` file, and specifying its filepath in a specific Java system property at runtime. Just as we did before with Zookeeper and Docker. I find this method to be very clumsy, and disassociates it from the place in our Java code that needs it. It might be nice for packaged code like Zookeeper, but for Applications we might not want that.

The second way, involves creating a custom Java class that extends the `Configuration` class. Which I find to be more modern, as well as more clear on what its actually for when it sits right next to the code that needs it.

I'll show you both ways in the next sections.

&nbsp;

#### Static JAAS File

The traditional way to configure client SASL via JAAS.

First create a `jaas-client.conf` file:

```js
Client {
    org.apache.zookeeper.server.auth.DigestLoginModule required
    username="super"
    password="1234";
};
```

Note: This is different than our ZK `jaas.conf` we created earlier. The format is not the same!

Tip: Don't git commit this file!

The next time we start our Java application, we need to pass a Java system property of `java.security.auth.login.config=jaas-client.conf`.
To be even more clear, we should also specify `zookeeper.sasl.client=true` and `zookeeper.sasl.clientconfig=Client` but those are the
default values given, so we don't need to change them here.

So, if the app was packaged as a runnable JAR we would pass this propery like so:

```bash
java -jar -Djava.security.auth.login.config="jaas-client.conf" myApp.jar
```

Note: The `-D` is required since this argument is not a JVM arg, but rather, an app context arg.

And that's it. There are no other Java code changes to make SASL via JAAS to work here. Underneath the hood, the Zookeeper client code inspects the Java system properties for this filepath, loads the file, and uses the JAAS configuration in the SASL connection automatically. Maybe most developers think this is fine, but I personally don't like implicit magic. I prefer clear code I can see with dynamic JAAS configuration.

&nbsp;

#### Dynamic JAAS Config

A more modern way to configure client SASL via JAAS.

Don't create a `jaas-client.conf`, as we will instead create a custom config class extending `Configuration` with:

```java
import org.apache.zookeeper.server.auth.DigestLoginModule;
import javax.security.auth.login.AppConfigurationEntry;
import javax.security.auth.login.Configuration;
import java.util.HashMap;
import java.util.Map;

public class ZookeeperSASLConfig extends Configuration {

    AppConfigurationEntry entry;

    public ZookeeperSASLConfig(String username,
                               String password) {
        Map<String,String> options = new HashMap<>();
        options.put("username", username);
        options.put("password", password);
        this.entry = new AppConfigurationEntry(
            DigestLoginModule.class.getName(),
            AppConfigurationEntry.LoginModuleControlFlag.REQUIRED,
            options
        );
    }

    @Override
    public AppConfigurationEntry[] getAppConfigurationEntry(String name) {
        AppConfigurationEntry[] array = new AppConfigurationEntry[1];
        array[0] = entry;
        return array;
    }
}
```

Then, we simply create an instance of this class and set it as our `Configuration`.

```java
import org.apache.curator.framework.CuratorFramework;
import org.apache.curator.framework.CuratorFrameworkFactory;
import org.apache.curator.retry.ExponentialBackoffRetry;
import org.apache.zookeeper.CreateMode;
import javax.security.auth.login.Configuration;

public class Application {

    public static void main(String args[]) {

        // String user = System.getenv("ZOOKEEPER_USERNAME");
        // String pass = System.getenv("ZOOKEEPER_PASSWORD");

        // Zookeeper client
        Configuration.setConfiguration(new ZookeeperSASLConfig("super", "1234"));
        CuratorFramework curator = CuratorFrameworkFactory.newClient(
            "127.0.0.1:2181",
            new ExponentialBackoffRetry(1000, 3)
        );
        System.out.println("Connecting to Zookeeper...");
        curator.start();

        // Test out our connection
        try {
            curator.create().creatingParentsIfNeeded().withMode(CreateMode.EPHEMERAL_SEQUENTIAL).forPath("/data/client-");
            System.out.println("Successfully connected to Zookeeper!");
        } catch (Exception e) {
            System.out.println("Failed to connect to Zookeeper.");
            System.exit(1);
        }
    }

}
```

Tip: Now its possible to use environment variables for our `super` username and password, with `System.getenv()`. This is great
for multi-environment deployments and cloud-based apps, but it also helps prevent accidentally git committing credentials to your repo.

And that's it for setup.

&nbsp;

## Running the Application

Go ahead and run `Application` main once you've finished setting up with either method above.
You should get this output if all goes well:

```bash
Connecting to Zookeeper...
Successfully connected to Zookeeper!

Process finished with exit code 0
```

Success! We have enabled SASL with Digest-MD5 via JAAS with Zookeeper server and client.

Now if you change the username and/or password on the client side, or change the allowed user/password configured in the server's `jaas.conf`, and restart Zookeeper, you should see an `Authentication failed` error when running our Application:

```bash
Connecting to Zookeeper...
22:05:13.023 [ERROR] org.apache.zookeeper.client.ZooKeeperSaslClient - SASL authentication failed using login context 'Client'.
javax.security.sasl.SaslException: Error in authenticating with a Zookeeper Quorum member: the quorum member's saslToken is null.
	at org.apache.zookeeper.client.ZooKeeperSaslClient.createSaslToken(ZooKeeperSaslClient.java:310)
	at org.apache.zookeeper.client.ZooKeeperSaslClient.respondToServer(ZooKeeperSaslClient.java:270)
	at org.apache.zookeeper.ClientCnxn$SendThread.readResponse(ClientCnxn.java:928)
	at org.apache.zookeeper.ClientCnxnSocketNIO.doIO(ClientCnxnSocketNIO.java:98)
	at org.apache.zookeeper.ClientCnxnSocketNIO.doTransport(ClientCnxnSocketNIO.java:350)
	at org.apache.zookeeper.ClientCnxn$SendThread.run(ClientCnxn.java:1283)
22:05:13.024 [ERROR] org.apache.curator.ConnectionState - Authentication failed
Failed to connect to Zookeeper.

Process finished with exit code 1
```

Failing as expected! Demonstrating our Zookeeper server is rejecting bad credentials too.

&nbsp;

### Summary

In conclusion, I think securing Zookeeper with SASL Digest-MD5 is a more appropriate introductory authentication mechanism than with SASL Kerberos. This blog post could have easily been a few pages of a large book. However, I'm not against using Kerberos, it definitely has its strengths and offers greater security than a username & password. Its just not for beginners.

&nbsp;

### Links:

- Apache Zookeeper https://zookeeper.apache.org
- Apache Curator https://curator.apache.org
- Docker Zookeeper https://hub.docker.com/_/zookeeper/
- O'REILLY Zookeeper: Distributed Process Coordination https://www.oreilly.com/library/view/zookeeper/9781449361297/
- SASL https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer
- Kerberos [https://en.wikipedia.org/wiki/Kerberos_(protocol)](https://en.wikipedia.org/wiki/Kerberos_(protocol))
- Digest-MD5 https://en.wikipedia.org/wiki/Digest_access_authentication
- JAAS https://en.wikipedia.org/wiki/Java_Authentication_and_Authorization_Service
