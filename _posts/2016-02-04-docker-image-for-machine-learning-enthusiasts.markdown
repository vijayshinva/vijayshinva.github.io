---
layout: postold
title:  "Docker image for machine learning enthusiasts"
date:   2016-02-04 12:00:00 +0530
categories: [machine learning, docker, Anaconda, Python, R, TensorFlow]
tags: [machine learning, docker, Anaconda, Python, R, TensorFlow]
---

It's 2016 and at the top of the hype cycle are Machine Learning, Statistical Computing, Data Visualization, Deep Learning.

If you are a Machine Learning noob you might be wondering where to start. 

* There is Anaconda Python with all its packages 'for science, math, engineering, and data analysis'
* Then we have R with its 'suite of software facilities for data manipulation, calculation and graphical display'
* Torch 'a scientific computing framework with wide support for machine learning algorithms'
* Google with its 'open source software library for numerical computation using data flow graphs' called TensorFlow
* Not far behind Microsoft with its 'Computational Network Toolkit'
* Theano ....

With all these nice to have open source toolkits, you will spend a lot of your time trying to figure how to install them.

Instead of trying to figure out installation steps for, say SciKit, R or TensorFlow; you can spin up a development environment using :

{% highlight docker %}
docker run -it vijayshinva/machinelearningultimate
{% endhighlight %}

`MachineLearningUltimate` is a docker image with a bunch of popular machine learning toolkits preinstalled. You can quickly spin up an instance of this docker image on Linux or Windows (using Docker ToolBox)

The goal of `MachineLearningUlitmate` is to have all popular machine learning / statistical computing toolkits ready for use. One image to rule them all !

Check out the [Github Repository][github-repo] for more info. You can also contribute to the project by filing bugs/feature requests. If you have questions, do leave a comment below.

[github-repo]: https://github.com/vijayshinva/MachineLearningUltimate


