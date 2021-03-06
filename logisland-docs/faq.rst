
Frequently Asked Questions.
===========================


I already use ELK, why would I need to use LogIsland ?
------------------------------------------------------
Well, at first one could say that that both stacks are overlapping, 
but the real purpose of the LogIsland framework is the abstraction of scalability of log aggregation.

In fact if you already have an ELK stack you'll likely want to make it scale (without pain) in both volume and features ways. 
LogIsland will be used for this purpose as an EOM (Event Oriented Middleware) based on Kafka & Spark, where you can plug advanced features
with ease.

So you just have to route your logs from the Logstash (or Flume, or Collectd, ...) agents to Kafka topics and launch parsers and processors.


Do I need Hadoop to play with LogIsland ?
-----------------------------------------

No, if your goal is simply to aggregate a massive amount of logs in an Elasticsearch cluster, 
and to define complex event processing rules to generate new events you definitely don't need an Hadoop cluster. 

Kafka topics can be used as an high throughput log buffer for sliding-windows event processing. 
But if you need advanced batch analytics, it's really easy to dump your logs into an hadoop cluster to build machine learning models.


How do I make it scale ?
------------------------
LogIsland is made for scalability, it relies on Spark and Kafka which are both scalable by essence, to scale LogIsland just have to add more kafka brokers and more Spark slaves.
This is the *manual* way, but we've planned in further releases to provide auto-scaling either Docker Swarn support or Mesos Marathon.


What's the difference between Apache NIFI and LogIsland ?
---------------------------------------------------------
Apache NIFI is a powerful ETL very well suited to process incoming data such as logs file, process & enrich them and send them out to any datastore.
You can do that as well with LogIsland but LogIsland is an event oriented framework designed to process huge amount of events in a Complex Event Processing
manner not a Single Event Processing as NIFI does. **LogIsland** is not an ETL or a DataFlow, the main goal is to extract information from realtime data.

Anyway you can use Apache NIFI to process your logs and send them to Kafka in order to be processed by LogIsland


Error : realpath not found
--------------------------

If you don't have the ``realpath`` command on you system you may need to install it::

    brew install coreutils
    sudo apt-get install coreutils

How to deploy LogIsland as a Single node Docker container
----------------------------------------------------------
The easy way : you start a small Docker container with all you need inside (Elasticsearch, Kibana, Kafka, Spark, LogIsland + some usefull tools)

`Docker <https://www.docker.com>`_ is becoming an unavoidable tool to isolate a complex service component. It's easy to manage, deploy and maintain. That's why you can start right away to play with LogIsland through the Docker image provided from `Docker HUB <https://hub.docker.com/r/hurence/logisland/>`_

.. code-block:: sh

    # Get the LogIsland image
    docker pull hurence/logisland

    # Run the container
    docker run \
          -it \
          -p 80:80 \
          -p 9200-9300:9200-9300 \
          -p 5601:5601 \
          -p 2181:2181 \
          -p 9092:9092 \
          -p 9000:9000 \
          -p 4050-4060:4050-4060 \
          --name logisland \
          -h sandbox \
          hurence/logisland:latest bash

    # Connect a shell to your LogIsland container
    docker exec -ti logisland bash


How to deploy LogIsland in an Hadoop cluster ?
----------------------------------------------
When it comes to scale, you'll need a cluster. **logisland** is just a framework that facilitates running sparks jobs over Kafka topics so if you already have a cluster you just have to get the latest logisland binaries and unzip them to a edge node of your hadoop cluster.

For now Log-Island is fully compatible with HDP 2.4 but it should work well on any cluster running Kafka and Spark.
Get the latest release and build the package.

You can download the `latest release build <https://github.com/Hurence/logisland/releases/download/v0.9.5/logisland-0.9.5-bin.tar.gz>`_

.. code-block:: bash

    git clone git@github.com:Hurence/logisland.git
    cd logisland-0.9.5
    mvn clean install -DskipTests

This will produce a ``logisland-assembly/target/logisland-0.9.5-bin.tar.gz`` file that you can untar into any folder of your choice in a edge node of your cluster.



Please read this excellent article on spark long running job setup : `http://mkuthan.github.io/blog/2016/09/30/spark-streaming-on-yarn/ <http://mkuthan.github.io/blog/2016/09/30/spark-streaming-on-yarn/>`_

