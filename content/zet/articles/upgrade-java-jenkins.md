---
date: "2022-07-19T18:01:26Z"
tags:
- DevOps
- Linux
title: How to Upgrade Java and Jenkins on Ubuntu 18.04
url: /how-to-upgrade-java-and-jenkins-on-ubuntu-18-04/
---

Last week I upgraded Jenkins to the latest version on the server infrastructure at work. Starting with the Jenkins 2.357 release, Java 11 or Java 17 [will be required to run Jenkins](https://www.jenkins.io/blog/2022/06/28/require-java-11/). Also, the upcoming LTS release will require Java 11.

This means that I also needed to update Java on our Jenkins servers. Here are the steps that I did to perform the Jenkins and Java upgrade.

SSH into the server and stop the service. Then get the latest upgrades for your server, which is good practice:

```bash
service jenkins stop
apt-get update
apt-get upgrade
```

Depending on your setup, the apt-get upgrade command might upgrade Jenkins to the latest version that does not require Java 11+. In my case, that was 3.346.

**When you get a question about updating your current config file, take the default option. This option keeps your current configuration.**

However, if your Jenkins is installed from a binary or another source, you might need to upgrade Jenkins to 3.346 using the Jenkins.war file:

```bash
cd /usr/share/jenkins
mv jenkins.war jenkins.war.old
wget https://updates.jenkins-ci.org/latest/jenkins.war
service jenkins start
```

When you start Jenkins, it will be updated to the latest version that does not require Java 11 or higher. You will notice that there will be a new folder called migrate in /usr/share/jenkins , and the jenkins.war is now located in /usr/share/java

This is where I got confused because it did not patch to the latest version, only up to 3.346 and the jenkins.war file was no longer being updated from the /usr/share/jenkins folder.

The reason is that this update moves the .war file to the /usr/share/java directory.

# java

To get Jenkins to the latest version, we need to install or update Java and check if it has worked:

```bash
apt-get install default-jre
java -version
```

Now that you have updated the java version, you are ready to update Jenkins to the latest version.

Notice that we use the /usr/share/java folder now, instead of /usr/share/jenkins

```bash
service jenkins stop
cd /usr/share/java
mv jenkins.war jenkins.war.old
wget https://updates.jenkins-ci.org/latest/jenkins.war
service jenkins start
```

# nodes

When I accessed the Jenkins GUI, everything seemed fine, and my version was up to 3.358.

However, I noticed that the build nodes were all offline. When inspecting the logs, I saw the following error:

```bash
java.io.EOFException
	at java.base/java.io.ObjectInputStream$PeekInputStream.readFully(ObjectInputStream.java:2905)
	at java.base/java.io.ObjectInputStream$BlockDataInputStream.readShort(ObjectInputStream.java:3400)
	at java.base/java.io.ObjectInputStream.readStreamHeader(ObjectInputStream.java:936)
	at java.base/java.io.ObjectInputStream.<init>(ObjectInputStream.java:379)
	at hudson.remoting.ObjectInputStreamEx.<init>(ObjectInputStreamEx.java:49)
	at hudson.remoting.Command.readFrom(Command.java:142)
	at hudson.remoting.Command.readFrom(Command.java:128)
	at hudson.remoting.AbstractSynchronousByteArrayCommandTransport.read(AbstractSynchronousByteArrayCommandTransport.java:35)
	at hudson.remoting.SynchronousCommandTransport$ReaderThread.run(SynchronousCommandTransport.java:61)
Caused: java.io.IOException: Unexpected termination of the channel
	at hudson.remoting.SynchronousCommandTransport$ReaderThread.run(SynchronousCommandTransport.java:75)
```

Observing that the error had something to do with Java, I ssh’d into the build nodes and updated Java there as well with the same command:

```bash
apt-get install default-jre
```

After updating Java on the build node, head back to the GUI on the master node and restart the build node.

It should now be online again.
