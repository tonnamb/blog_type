---
layout: post
title: Scala - Apache Spark DataFrame API Cheatsheet
featured: true
tags: [spark]
image: '/images/posts/spark_logo.png'
---

Recently, I've came to a realization that having a good cheatsheet at hand can significantly speed up how fast you can program.
The cheatsheet that changed my life is <a href="https://ugoproto.github.io/ugo_r_doc/sparklyr-cheatsheet.pdf" target="_blank">sparklyr's cheatsheet</a>.

For my work, I'm using Spark's DataFrame API in Scala to do data wrangling and creating data transformation pipelines. These are some functions and design patterns that I've found to be extremely useful.

Load data
{% highlight scala %}
val df = spark.read.parquet("filepath")
{% endhighlight %}

Get SparkContext information
{% highlight scala %}
println(sc.getConf.getAll.mkString("\n"))
{% endhighlight %}

Get Spark version
{% highlight scala %}
sc.version
{% endhighlight %}

Get number of partitions
{% highlight scala %}
df.rdd.getNumPartitions
{% endhighlight %}

Count number of rows
{% highlight scala %}
df.count
{% endhighlight %}

Print schema
{% highlight scala %}
df.printSchema
{% endhighlight %}

Preview top 20 rows
{% highlight scala %}
df.show
{% endhighlight %}

Design pattern for constructing as data transformation pipeline
{% highlight scala %}
import org.apache.spark.sql.DataFrame
 
def filter_slim_cat_dog(df: DataFrame): DataFrame = {
  df.filter(($"animal_type" isin ("cat", "dog")) && ($"weight" <= 100))
}
 
def join_vet(df: DataFrame): DataFrame = {
  df.join(vet_provider, Seq("VETID", "ANIMALID"))
}
 
val animal_slim_cat_dog = 
  animal.transform(filter_slim_cat_dog)
        .transform(join_vet)
{% endhighlight %}

Drop duplicate rows
{% highlight scala %}
df.dropDuplicates(Seq("VETID", "ANIMALID"))
{% endhighlight %}

Hope you've found this cheatsheet useful. If you did, please feel free to share it with your friends. Thank you!
