# elasticsearch-slides

!SLIDE

# 10+10 minutes intro to Elasticsearch

}}} images/tree.jpg

!SLIDE left

# Elasticsearch <span style="color:gray">is a</span>

* multitenant,
* (near to) real-time,
* distributed

index engine based on **Lucene**, that

* adapts to your domain model (not the other way around)
* is easy to setup and
* is fully configurable through an HTTP API

!SLIDE left

# Elasticsearch

* <span style="color:gray">was started</span> by the author of Compass, Shay Banon
* <span style="color:gray">is written</span> in Java
* <span style="color:gray">is actively developed</span> @[github.com/elasticsearch/elasticsearch](https://github.com/elasticsearch/elasticsearch)
    * 0.5 
    * 0.18 
    * 0.19 
    * 0.19.4 
    * 0.19.5 
    * 0.19.8 (July 2, 2012)

!SLIDE left
# Anatomy of elastic search

}}} images/LondonEye.jpg

!SLIDE left
# Anatomy 1/3

* ElasticSearch runs as a service on your webserver.
* If you are running a single server you are running a single ElasticSearch **cluster**. 
* A single cluster can house the search for more than one site, each of which is stored in an **index**. 
* Within an index are **types**. These map nicely onto Symphony sections, e.g. articles, products or comments. 
* ElasticSearch stores **documents** (Symphony entries) which are made up of **fields**.

!SLIDE left
# Anatomy 2/3

* Fields within a document can be strings, numbers, dates, arrays/collections, or several others. 
* Although ElasticSearch will automatically create a new type when you throw a new type of document at it. * The structure of a type is known as a **mapping**, and is formatted in a JSON file.

!SLIDE left
# Anatomy 3/3

* A **query type** is how to query ElasticSearch e.g. text, boolean, wildcard, fuzzy. 
	* two main types: **query_string** and **match_all**.
* **Analysers** are the logic that is run against both the content you are indexing (an entry) and what you are searching for (a keyword). 
* An analyser comprises a **tokeniser**, which specifies how the tokens (usually words) are broken up (usually based on spaces between words), and **filters**, which work their magic on each word (such as removing stop words, reducing a word to its stem, or replacing with a synonym).

!SLIDE left
# Multitenancy

* have as many indexes as you want
* graylog uses one index per day
* finc uses one index per data source

``` sh
$ curl -XPUT    localhost:9200/sample_index # create index
$ curl -XPUT    localhost:9200/throwaway    # create another index
$ curl -XDELETE localhost:9200/throwaway    # destroy index
```

* search over one or more indexes

``` sh
$ curl -XGET    localhost:9200/idx1,idx2/_search?q=Leipzig # search for Leipzig in idx1 and idx2
```

!SLIDE left
# Real-time / `refresh_interval` at 1s by default

}}} images/rt.jpg

!SLIDE left
# Distributed

* start with one node, add more on the go
* control the number of replicas

``` sh
$ curl -XPUT localhost:9200/sample_index/_settings -d '{ "index" : { "number_of_replicas" : 2 }}'
```

* automatic load balancing (index, search)
* replicas are near real-time too (Push replication)

!SLIDE left
# Adapts to your domain

}}} images/domain.png

!SLIDE left
# Adapts to your domain

* JSON documents
* dynamic mapping (without overhead)
* mapping templates

!SLIDE left

# Easy to setup

* you'll need a Java Virtual Machine
* then

``` shell
$ wget https://github.com/downloads/elasticsearch/elasticsearch/elasticsearch-0.19.4.zip
$ unzip elasticsearch-0.19.4.zip
$ cd elasticsearch-0.19.4
$ bin/elasticsearch -f
[16:25:21,766][INFO ][node      ] [Dracula] {0.19.4}[4028]: initializing ...
[16:25:21,775][INFO ][plugins   ] [Dracula] loaded [], sites []
[16:25:24,015][INFO ][node      ] [Dracula] {0.19.4}[4028]: initialized
[16:25:24,015][INFO ][node      ] [Dracula] {0.19.4}[4028]: starting ...   
...
[16:25:27,324][INFO ][gateway   ] [Dracula] recovered [3] indices into cluster_state
```

!SLIDE left

# Easy to setup

* index a doc

``` shell
$ curl -XPUT 'http://localhost:9200/twitter/tweet/1' -d '{
    "user" : "kimchy",
    "post_date" : "2009-11-15T14:12:12",
    "message" : "trying out Elastic Search"
}'
```

* search a doc

``` shell
$ curl -XGET 'http://localhost:9200/twitter/tweet/_search?q=user:kimchy'
```


!SLIDE left

# Configuration via HTTP API

* prominent settings:
    * `index.number_of_replicas`
    * `index.refresh_interval`
    * `index.blocks.read_only`
    * `index.merge.policy.merge_factor`

* and a lot more (ES + exposed Lucene settings)

!SLIDE left

# What others say

* Solr may be the weapon of choice when building standard search applications, but Elasticsearch takes it to the next level with an architecture for creating modern realtime search applications. 

* Percolation is an exciting and innovative feature that singlehandedly blows Solr right out of the water. Elasticsearch is scalable, speedy and a dream to integrate with. (Ryan Sonnek, socialcast)

* more: [http://stackoverflow.com/questions/10213009/solr-vs-elasticsearch](http://stackoverflow.com/questions/10213009/solr-vs-elasticsearch), [http://www.quora.com/What-are-the-main-differences-between-ElasticSearch-Apache-Solr-and-SolrCloud](http://www.quora.com/What-are-the-main-differences-between-ElasticSearch-Apache-Solr-and-SolrCloud)

!SLIDE left

# Percolation?

}}} images/percolation.jpg

!SLIDE left

# Percolation

* Think of it as the reverse operation of indexing and then searching 
* Instead of sending docs, indexing them, and then running queries
* One sends queries, registers them, and then sends docs and finds out which queries match that doc


!SLIDE left

# Wrap up

* more features not covered:
    * search / query language
    * facets
    * scrolls

* language bindings:
    * Elasticsearch.pm, PHP, Ruby, Python, Java (native), .NET, Clojure, Erlang

!SLIDE left

# PHP binding:

* [https://github.com/ruflin/Elastica/wiki/Links](https://github.com/ruflin/Elastica/wiki/Links)

# Symfony bundle:

* [http://symphonyextensions.com/extensions/elasticsearch](http://symphonyextensions.com/extensions/elasticsearch)

# A must plugin:
* [http://localhost:9200/_plugin/head](http://localhost:9200/_plugin/head)

!SLIDE left

# Demo?

}}} images/shutterstock_86440180.jpg
!SLIDE left
}}} images/palazzotoschi.jpg

# Attachment Type in Action [https://gist.github.com/1075067](https://gist.github.com/1075067)
!SLIDE left


* Download some data

``` sh
curl -C - -O http://www.intersil.com/data/fn/fn6742.pdf
```

* Attachments plugin can parse and index documents in many formats. Check Tika web page for list of supported formats.
!SLIDE left


* Setup mapping (aka prepare new index for the data)

``` sh
curl -X DELETE "localhost:9200/test"

curl -X PUT "localhost:9200/test" -d '{
  "settings" : { "index" : { "number_of_shards" : 1, "number_of_replicas" : 0 }}
}'
```


Before we can use the attachments plugin we need to create correct mapping for the attachment type.

``` sh
curl -X PUT "localhost:9200/test/attachment/_mapping" -d '{
  "attachment" : {
    "properties" : {
      "file" : {
        "type" : "attachment",
        "fields" : {
          "title" : { "store" : "yes" },
          "file" : { "term_vector":"with_positions_offsets", "store":"yes" }
        }
      }
    }
  }
}'
```
!SLIDE left

Indexing the Data
We are ready to index the data. We just need to encode the content of the file with Base64 for which we will use a simpe Perl script.

``` sh
#!/bin/sh

coded=`cat fn6742.pdf | perl -MMIME::Base64 -ne 'print encode_base64($_)'`
json="{\"file\":\"${coded}\"}"
echo "$json" > json.file
curl -X POST "localhost:9200/test/attachment/" -d @json.file
```

!SLIDE left

Search for Highlighted results
* We are done indexing the data and we are now free to search it. To make it even more cool we want the document title and some highlighted results back (note how we setup mapping for the title and file content).

``` sh
curl "localhost:9200/_search?pretty=true" -d '{
  "fields" : ["title"],
  "query" : {
    "query_string" : {
      "query" : "amplifier"
    }
  },
  "highlight" : {
    "fields" : {
      "file" : {}
    }
  }
}'
```

!SLIDE left

# Credits

* http://www.flickr.com/photos/catdancing/4652800293/
* http://www.flickr.com/photos/andreaskoller/154706613/
* http://www.flickr.com/photos/ddebold/3738709697/

