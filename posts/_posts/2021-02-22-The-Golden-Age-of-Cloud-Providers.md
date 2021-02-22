---
layout: post
title: The Golden Age of Cloud Providers
tags: 
cover_url: https://source.unsplash.com/random?cloud
cover_meta: 
  (c) UNSPLASH
color_scheme: tango
mathjax: true
mathjax: True
---
<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>


<style>
blockquote.yellownote {
    border-left: 12px solid #dc0;
    background-color: #ffa;
    padding: 12px 12px 12px 0;
    margin-left: -48px;
    padding-left: 48px;
}
blockquote.sidenote {
    border-left: 12px solid #dc0;
    background-color: #ffa;
    padding: 12px 12px 12px 0;
    margin-left: -48px;
    padding-left: 48px;
}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>

On a busy Monday, I wanted to have a DB instance for my dev environment for a new application. It had been a while I used Postgres and I was working with JSONs and needed simplicity - Postgres was the obvious choice. It came across my mind to use AWS to setup a new DynamoDB instance on the fly but I was trying to save money and wanted something I could leave running overnight without having to worry.

As I was trying to setup up a local Postgres DB for this, I thought the process would be as simple as downloading a binary, running it which would open up some GUI, allow me to setup a new local DB and get it up and running with my application.

Only to realize (an hour later) that I was a fool believing that.

After 30 minutes of downloading and installing Postgres for Mac, creating a database on the PGAdmin tool, and looking at the beautiful UI

![](https://github.com/abhinandandubey/abhinandandubey.github.io/raw/master/assets/images/2021-02-22-18-31-50.png)

I added sbt dependencies to my application which was using an ORM called Slick and HickariCP for connection pools 

```scala
libraryDependencies ++= Seq(
    "com.typesafe.slick" %% "slick" % "3.3.0",
    "org.slf4j" % "slf4j-nop" % "1.6.4",
    "com.typesafe.slick" %% "slick-hikaricp" % "3.3.0",
    "org.postgresql" % "postgresql" % "9.4-1206-jdbc42" //org.postgresql.ds.PGSimpleDataSource dependency
)
```


In 15 mins I was ready with a boilerplate code to connect to the database, create a table and do an insert operation - thanks to the magic of ORMs. And as I was on my toes to see that little row of data inserted into a brand new table created by my application, I was greeted with throwing errors.


    An error has occurred: The authentication type 10 is not supported. Check that you have configured the pg_hba.conf file to include the client's IP address or subnet, and that it is using an authentication scheme supported by the driver.


As any sane developer would do I did a Google search for the error message. Seems like something from 2017 which was addressed by Postgres in their releases 3 years ago. Many <a href="https://stackoverflow.com/q/64210167/5102599">Stackoverflow answers</a> pointing to update the version in your `build.sbt`/`pom.xml` - so I did that.

And yet another error showed up in the terminal. `NoClassDefFoundError` - and then I realized the ORM must be using an older version of the driver, which was embarassing - I couldn't get away by removing the ORM because the application I was building needed to be DB agnostic to a certain extent. 


```bash
Exception in thread "pgsql1-1" java.lang.NoClassDefFoundError: org/postgresql/shaded/com/ongres/scram/client/ScramClient$ChannelBinding
	at org.postgresql.jre8.sasl.ScramAuthenticator.processServerMechanismsAndInit(ScramAuthenticator.java:74)
	at org.postgresql.core.v3.ConnectionFactoryImpl.doAuthentication(ConnectionFactoryImpl.java:621)
	at org.postgresql.core.v3.ConnectionFactoryImpl.openConnectionImpl(ConnectionFactoryImpl.java:207)
	...
Caused by: java.lang.ClassNotFoundException: org.postgresql.shaded.com.ongres.scram.client.ScramClient$ChannelBinding
	at java.net.URLClassLoader.findClass(URLClassLoader.java:381)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:331)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
```

## The Realization

After wasting an hour on setting it up, going through documentation, and then hitting a wall I realized how much time I'd have saved had I set up one on AWS. How effortless, easy and intuitive! No need to go through documentation, all that instrumentation done for you behind the scenes. 

For a corporate - you have to move fast and time is everything. I can wonder why cloud providers such as AWS and GCP are having a 'Golden Age' of sorts.




<br/>
Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
<br/>
<br/>


