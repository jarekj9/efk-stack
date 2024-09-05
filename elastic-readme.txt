$ enter efk_elasticsearch
elasticsearch@b2299cf6b812:~$ bash bin/elasticsearch-setup-passwords auto
******************************************************************************
Note: The 'elasticsearch-setup-passwords' tool has been deprecated. This       command will be removed in a future release.
******************************************************************************

Initiating the setup of passwords for reserved users elastic,apm_system,kibana,kibana_system,logstash_system,beats_system,remote_monitoring_user.
The passwords will be randomly generated and printed to the console.
Please confirm that you would like to continue [y/N]y


Changed password for user apm_system
PASSWORD apm_system = IHVBZR5wORAVSTJSjq6H

Changed password for user kibana_system
PASSWORD kibana_system = 5VUT57ctnkns5iBEDgzO

Changed password for user kibana
PASSWORD kibana = 5VUT57ctnkns5iBEDgzO

Changed password for user logstash_system
PASSWORD logstash_system = 3p9vPkGnWJ6rjRULWkZv

Changed password for user beats_system
PASSWORD beats_system = 3baJfAwhuCW4y2An8QdQ

Changed password for user remote_monitoring_user
PASSWORD remote_monitoring_user = WzrCfWJNW1SxES3u6uFy

Changed password for user elastic
PASSWORD elastic = J9tQFfhJ7uGZdw1qoeqi



# fluent.conf:

<match *.**>
  @type copy
  <store>
	...
    ...
    user elastic
    password <enter_the_generated_password_for_fluentd_user>

  </store>
  <store>
    @type stdout
  </store>
</match>
