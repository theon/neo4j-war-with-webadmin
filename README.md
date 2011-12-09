Spring Data Neo4J with Web Admin for Tomcat Example
===================================================

Intro
-----

This example skeleton project was made as a result of the following mailing list thread which outlines a work around for a issue experienced whilst trying to use spring data neo4j with the neo4j web admin in tomcat.

https://groups.google.com/forum/#!topic/neo4j/1-NUcYu8dLw

Reproduce the issue
-------------------

Build with

> mvn -f pom-broken.xml clean install

Then deploy target/neo4j.war to tomcat (note: I do not get this problem when the war is deployed to jetty. Only tomcat)
Hit the url localhost:7474 and hit the refresh button in chrome. You should see exceptions in the tomcat logs of the form:

<pre>
SEVERE: /webadmin/js/require.js
java.lang.IllegalStateException: zip file closed
	at java.util.zip.ZipFile.ensureOpen(ZipFile.java:415)
	at java.util.zip.ZipFile.access$100(ZipFile.java:31)
	at java.util.zip.ZipFile$2.hasMoreElements(ZipFile.java:315)
	at java.util.jar.JarFile$1.hasMoreElements(JarFile.java:222)
	at org.mortbay.resource.JarFileResource.exists(JarFileResource.java:157)
	at org.mortbay.jetty.servlet.DefaultServlet.doGet(DefaultServlet.java:388)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:617)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:717)
	at org.mortbay.jetty.servlet.ServletHolder.handle(ServletHolder.java:511)
	at org.mortbay.jetty.servlet.ServletHandler.handle(ServletHandler.java:390)
	at org.mortbay.jetty.security.SecurityHandler.handle(SecurityHandler.java:216)
	at org.mortbay.jetty.servlet.SessionHandler.handle(SessionHandler.java:182)
	at org.mortbay.jetty.handler.ContextHandler.handle(ContextHandler.java:765)
	at org.mortbay.jetty.webapp.WebAppContext.handle(WebAppContext.java:440)
	at org.mortbay.jetty.handler.HandlerCollection.handle(HandlerCollection.java:114)
	at org.mortbay.jetty.handler.HandlerWrapper.handle(HandlerWrapper.java:152)
	at org.mortbay.jetty.Server.handle(Server.java:326)
	at org.mortbay.jetty.HttpConnection.handleRequest(HttpConnection.java:542)
	at org.mortbay.jetty.HttpConnection$RequestHandler.headerComplete(HttpConnection.java:926)
	at org.mortbay.jetty.HttpParser.parseNext(HttpParser.java:549)
	at org.mortbay.jetty.HttpParser.parseAvailable(HttpParser.java:212)
	at org.mortbay.jetty.HttpConnection.handle(HttpConnection.java:404)
	at org.mortbay.io.nio.SelectChannelEndPoint.run(SelectChannelEndPoint.java:410)
	at org.mortbay.thread.QueuedThreadPool$PoolThread.run(QueuedThreadPool.java:582)
</pre>

Working around the issue
------------------------

Build with

> mvn clean install

Then deploy target/neo4j.war to tomcat. The problem should no longer exist.

So what is the difference? Diff pom.xml and pom-broken.xml to see.

Basically by default the neo4j-server static-web jar gets included inside the war file at WEB-INF/lib. The workaround is to extract this JAR into the WAR at WEB-INF/classes.


