---
layout: post
title: Releasing the Clarifai API Scala Client
---
This quarter I was enrolled CS130 - Software Engineering and had the chance to work with [Clarifai](https://www.clarifai.com), a NYC-based startup that provides a robust visual recognition API. The goal of the course is to deliver a complete product based on the company's need. After discussing with the Clarifai engineers about potential project ideas, my team decided to develop a Scala client wrapper for the Clarifai API and a data analytics web application using their API and [Spark MLlib](http://spark.apache.org/docs/latest/mllib-guide.html). We were also fortunate enough to have [Cassidy Williams](https://twitter.com/cassidoo) as our mentor and received help from her throughout the course of 10 weeks. 

Today, we are excited to announce that we have released the Clarifai API Scala client on [Sonatype](https://oss.sonatype.org/#nexus-search;quick~clarifai-scala). The Scala client allows the users to access all major Clarifai endpoints: tag, feedback, info, usage, and color (beta) and makes Clarifai API integration much easier for Scala users. To install the Scala client, the user only has to include one line in the *build.sbt* file:

{% highlight scala %}
libraryDependencies += "com.github.vic317yeh" %% "clarifai-scala" % "1.0.0"
{% endhighlight %}

If you are new to the Clarifai API, it is very easy to create an object and start using the visual recognition service. All you need is the *client id* and *client secret* from your Clarifai [developer page](https://developer.clarifai.com), and then instantiate the *ClarifaiClient* class in the following way:

{% highlight scala %}
import clarifai._
val client = new  ClarifaiClient("<client_id>", "<client_secret>")

// get the version of Clarifai API
val infoRet = client.info()
if (infoRet.isRight) {
    val info:InfoResp = infoRet.right.get
    println("API version is " + info.results.apiVersion)
} else {
    println("Error: " + infoRet.left.get)
}
{% endhighlight %}

The Clarifai API is very fast and accurate when compared to other existing visual recognition services in the market. I highly recommend you checking out some of the endpoints like tag, which allows you to get the content of your images and videos. It was fun messing with the image data in our application.

Finally, you can find the documentation and source code of the Scala client [here](https://github.com/vic317yeh/clarifai-scala). If you happen to catch any errors or would like to help improve this project, feel free to email me anytime.