$ enter efk_elasticsearch
elasticsearch@b2299cf6b812:~$ bash bin/elasticsearch-setup-passwords auto
******************************************************************************
Note: The 'elasticsearch-setup-passwords' tool has been deprecated. This       command will be removed in a future release.
******************************************************************************

Initiating the setup of passwords for reserved users elastic,apm_system,kibana,kibana_system,logstash_system,beats_system,remote_monitoring_user.
The passwords will be randomly generated and printed to the console.
Please confirm that you would like to continue [y/N]y


Changed password for user apm_system
PASSWORD apm_system = D0rGdIaGzF974g3LTpix

Changed password for user kibana_system
PASSWORD kibana_system = PysF1tBAmkiEVmF0aZWd

Changed password for user kibana
PASSWORD kibana = PysF1tBAmkiEVmF0aZWd

Changed password for user logstash_system
PASSWORD logstash_system = zatCdp8VbcdKI6a7t5nI

Changed password for user beats_system
PASSWORD beats_system = i638GJhcVxBUoukKK135

Changed password for user remote_monitoring_user
PASSWORD remote_monitoring_user = Git3AUZMNMVYhzeMmyOV

Changed password for user elastic
PASSWORD elastic = g1BHQNhrYkxx4RTfzAPQ



# fluent.conf:

<match *.**>
  @type copy
  <store>
	...
    ...
    user elastic
    password <enter_the_generated_password_for_fluentd_user>  # Changed password for user elastic

  </store>
  <store>
    @type stdout
  </store>
</match>


# elastic check log indexing:

jarek@yoga:/mnt/c/MY/pliki/skrypty/docker/efk_stack$ enter efk_elasticsearch
elasticsearch@0fa4bf901389:~$ curl -u elastic:g1BHQNhrYkxx4RTfzAPQ -X GET "http://localhost:9200/_cat/indices?v"
health status index                                                        uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   .internal.alerts-observability.logs.alerts-default-000001    749hLlmISIu9bXGVuYDNVg   1   0          0            0       247b           247b
green  open   .internal.alerts-observability.uptime.alerts-default-000001  HvmbxsVCS2GuZsuUZQOUXQ   1   0          0            0       247b           247b
green  open   .fleet-file-data-agent-000001                                QVKBwgOnRKe8rhmc_lb53Q   1   0          0    
...

elasticsearch@0fa4bf901389:~$ curl -u elastic:g1BHQNhrYkxx4RTfzAPQ -X GET "http://localhost:9200/allogs-*/_count"
{"count":8222,"_shards":{"total":3,"successful":3,"skipped":0,"failed":0}}elasticsearch@0fa4bf901389:~$

# send log manually in elastic:

elasticsearch@0fa4bf901389:~$ curl -u elastic:g1BHQNhrYkxx4RTfzAPQ -X POST "http://localhost:9200/allogs-20240921/_doc/" -H 'Content-Type: application/json' -d '{
  "host": "yoga",
  "ident": "log-generator",
  "pid": "45149",
  "msgid": "log-generator",
  "extradata": "-",
  "message": "2024-09-21T19:16:35.000Z hostname testapp [warn]: This is some log message."
}'
