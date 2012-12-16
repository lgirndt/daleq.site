---
title: Download
layout: down
---

There are several ways to get Daleq.

## Maven

Daleq lives in the Maven Central Repository. Including Daleq into your project is as easy as just adding the dependency:

{% highlight xml %}
<dependency>
    <groupId>de.brands4friends.daleq</groupId>
    <artifactId>daleq-core</artifactId>
    <version>{{ site.version }}</version>
    <scope>test</scope>
</dependency>
{% endhighlight %}

or if you are a Spring junky and want to leverage Daleq's Spring capabilities add as well:

{% highlight xml %}
<dependency>
    <groupId>de.brands4friends.daleq</groupId>
    <artifactId>daleq-spring</artifactId>
    <version>{{ site.version }}</version>
</dependency>
{% endhighlight %}

## Download Jars

Or you might want to download the Jars directly.

| **Module** | | | | |
|:--|--|--|--|--|
| daleq-core   | {{ site.version }}   |  [Jar]() | [Sources]() | [Javadoc]() |
| daleq-spring | {{ site.version }} | [Jar]() | [Sources]() | [Javadoc]() |
{: .table}

## Get the Sources

Clone the Git Repository from GitHub

	$ git clone git://github.com/brands4friends/daleq.git
