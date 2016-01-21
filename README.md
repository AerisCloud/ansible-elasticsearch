ElasticSearch Role
==================

This role will install ElasticSearch and ElasticDump on your servers.

Configuration
-------------

### Inventory

```ini
#
# Here, you put the list of your Couchbase cluster
#
[elasticsearch:children]
elasticsearch-somedc-prod

[elasticsearch-somedc-prod:children]
elasticsearch1-somedc-prod

#
# And you create one children per cluster
#
[elasticsearch1-somedc-prod]
node1.elasticsearch.somedc.prod1 ansible_ssh_host=172.16.0.80
node2.elasticsearch.somedc.prod1 ansible_ssh_host=172.16.0.81
node3.elasticsearch.somedc.prod1 ansible_ssh_host=172.16.0.82

#
# Configure the RAM size and # of replicates accordingly
#
[elasticsearch1-somedc-prod:vars]
elasticsearch_cluster_name = elasticsearch1-somedc-prod
#
# Auto-discovery is enabled by default. However,
# in some environment such as AWS, this will not
# work as intended. By setting autodiscovery
# to false, we will use unicast discovery
# instead (as in, we will add all nodes)
# to configuration instead of relying on multicasting
# For production, set to false.
# Default: false.
elasticsearch_autodiscovery = false

# Default multicast port
elasticsearch_autodiscovery_port = 54328

# ElasticSearch heap size (default: 256m)
# http://www.elasticsearch.org/guide/en/elasticsearch/guide/current/heap-sizing.html
elasticsearch_heap_size = 1g

# Version of ElasticDump to install
elasticdump_version = v0.12.0

# Default Elasticsearch version.
# Note: Ensure the meta/main.yml file uses the corresponding repository. (e.g. `elasticsearch-2.x`, `elasticsearch-1.7`, etc.)
# See https://github.com/AerisCloud/ansible-repos for corresponding repositories.
elasticsearch_version = 2.1.1
```



### Execution

You will need to run this on the whole cluster every time you
add or remove nodes to it if you are running this with autodiscovery
set to false. However, you may provision only the machine you are
adding if you are using autodiscovery.

See also
--------

* [ElasticSearch](https://www.elastic.co/products/elasticsearch)
* [ElasticDump](https://github.com/taskrabbit/elasticsearch-dump)
