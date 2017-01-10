# Processing Solr Cloud logs using logstash 

## How to process Solr Logs with Logstash.

Grok expression that extract the collection field.

	filter {
	    if [type] == "solr-log" {
		grok {
		  match => [ "message", "%{LOGLEVEL:loglevel}  - %{TIMESTAMP_ISO8601:timestamp}; \[c:%{WORD:clustername} s:%{WORD:shardname} r:%{WORD:replicaname} x:%{WORD:collectionname}\] %{NOTSPACE:javamethod}; slow: \[%{WORD:collectionname2}\] webapp=/%{WORD:webapp} path=/%{WORD:path} params=%{DATA:rawrequest} hits=%{INT:hits} status=%{INT:status} QTime=%{INT:qtime}" ]
		  overwrite => [ "message" ]
		}
		mutate {
		  remove => [ "rawrequest" ]  # Removes the 'client' field
		}
	    }
	}

