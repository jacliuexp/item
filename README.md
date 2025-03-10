Python
```
import requests
response = requests.get('https://www.google.com', verify=False)
print(response.status_code)
print(response.text)
```

```
terraform plan  -destroy --target=module.mymodule.aws_instance.postgres
```

```
export TF_LOG=DEBUG  (RACE, DEBUG, INFO, WARN or ERROR)
export TF_LOG_PATH=./terra.log
terraform init
terraform plan --target=module.mymodule -no-color > plan.txt
```

```
ansible-playbook install_pip.yml --limit server1
ansible-playbook update_configuration_hosts.yml
```

```
locate bin/postgres
postgres --version or -V
psql --version
posgresql dir structure:
/usr/pgsql-13   # installation directory
/var/lib/pqsql/  # data directory

pg_ctl start | stop | status | reload | restart

psql → show data_directory

postgres=# show data_directory;
     data_directory
------------------------
 /lib/pgsql/13/data
(1 row)

postgres=# show port;
 port
------
 5432
(1 row)

postgres=# show work_mem;
 work_mem
----------
 4MB
(1 row)

postgres=# show max_connections;
 max_connections
-----------------
 150
(1 row)

postgres=# select name, source, boot_val, sourcefile, pending_restart from pg_settings where name='max_connections';
      name       |       source       | boot_val |               sourcefile               | pending_restart
-----------------+--------------------+----------+----------------------------------------+-----------------
 max_connections | configuration file | 100      | /mydata/lib/pgsql/13/data/postgresql.conf | f
(1 row)

postgres=#

postgres=# \d pg_settings
               View "pg_catalog.pg_settings"
     Column      |  Type   | Collation | Nullable | Default
-----------------+---------+-----------+----------+---------
 name            | text    |           |          |
 setting         | text    |           |          |
 unit            | text    |           |          |
 category        | text    |           |          |
 short_desc      | text    |           |          |
 extra_desc      | text    |           |          |
 context         | text    |           |          |
 vartype         | text    |           |          |
 source          | text    |           |          |
 min_val         | text    |           |          |
 max_val         | text    |           |          |
 enumvals        | text[]  |           |          |
 boot_val        | text    |           |          |
 reset_val       | text    |           |          |
 sourcefile      | text    |           |          |
 sourceline      | integer |           |          |
 pending_restart | boolean |           |          |

postgres=#  select * from pg_file_settings;

postgres=# \q
-bash-4.2$
\? list all the commands
\l list databases
\conninfo display information about current connection
\c [DBNAME] connect to new database, e.g., \c template1
\dt list tables of the public schema
\dt <schema-name>.* list tables of certain schema, e.g., \dt public.*
\dt *.* list tables of all schemas
Then you can run SQL statements, e.g., SELECT * FROM my_table;(Note: a statement must be terminated with semicolon ;)
\q quit psql
```
```
Ansible role for postgres
ANXS.postgresql
```

```
nc -vz  zk-0 2181
echo ruok | nc zk-0 2181 ; echo
echo stat | nc zk-0 2181 ; echo
echo conf | nc zk-0 2181 ; echo
echo cons | nc zk-0 2181 ; echo

./kafka-topics.sh --bootstrap-server zk-0:9092 --list topic 
./kafka-topics.sh --bootstrap-server zk-0:9092 --describe --topic mytopic
./kafka-topics.sh --bootstrap-server zk-0:9092 --delete --topic mytopic
./kafka-topics.sh --create --bootstrap-server zk-0:9092 --replication-factor 3 --partitions 1 --topic mytopic

./kafka-console-producer.sh --bootstrap-server zk-0:9092 --topic aaa
./kafka-console-consumer.sh --bootstrap-server zk-0:9092 --topic aaa --from-beginning

/kafka/kafka-logs/mytopic-0
```
```
    receivers:
    - name: 'default'
      msteams_configs:
      - send_resolved: true
        webhook_url: 'https://mycompany.webhook.office.com/webhookb2/824d277e-7b66-465c-acfe-ed5a247699e1@be67623c-1932-42a6-9d24-6c359fe5ea71/IncomingWebhook/8c05bae9d4a24fbd8bc48e50dda9c1fb/412501ee-8dcf-4aff-a499-9a56098e769f/V2x9qQyhy0SPhw4CHujS7IQa3g5li5JaKDrjgAstn5wms1'
        title: '{{ if eq .Status "firing" }}🚨 **FIRING** 🔥{{- else -}}🙌 **RESOLVED** 🍻{{- end -}}'
        text: '{{ template "msteams.text" . }}'
    - name: 'ops-critical'
      msteams_configs:
      - send_resolved: true
        webhook_url: 'https://mycompany.webhook.office.com/webhookb2/824d277e-7b66-465c-acfe-ed5a247699e1@be67623c-1932-42a6-9d24-6c359fe5ea71/IncomingWebhook/8c05bae9d4a24fbd8bc48e50dda9c1fb/412501ee-8dcf-4aff-a499-9a56098e769f/V2x9qQyhy0SPhw4CHujS7IQa3g5li5JaKDrjgAstn5wms1'
        title: '{{ if eq .Status "firing" }}🚨 **FIRING** 🔥{{- else -}}🙌 **RESOLVED** 🍻{{- end -}}'
        text: '{{ template "msteams.text" . }}'
    - name: 'ops-warning'
      msteams_configs:
      - send_resolved: true
        webhook_url: 'https://mycompany.webhook.office.com/webhookb2/824d277e-7b66-465c-acfe-ed5a247699e1@be67623c-1932-42a6-9d24-6c359fe5ea71/IncomingWebhook/8c05bae9d4a24fbd8bc48e50dda9c1fb/412501ee-8dcf-4aff-a499-9a56098e769f/V2x9qQyhy0SPhw4CHujS7IQa3g5li5JaKDrjgAstn5wms1'
        title: '{{ if eq .Status "firing" }}🚨 **FIRING** 🔥{{- else -}}🙌 **RESOLVED** 🍻{{- end -}}'
        text: '{{ template "msteams.text" . }}'

  templateFiles:
       msteams.tmpl: |-
      {{ define "msteams.text" -}}
      **Alert:** {{ .CommonLabels.alertname }}\
      **Service:** {{ .CommonLabels.service }}\
      **Severity:** {{ .CommonLabels.severity | title -}}\
      {{- if (index .Alerts 0).Annotations.summary }}
      **Summary:** {{ (index .Alerts 0).Annotations.summary }}
      {{- end }}
      {{ range .Alerts }}
      {{- if .Annotations.description }}{{ .Annotations.description }}\{{- end }}
      {{- if .Annotations.message }}{{ .Annotations.message }}\{{- end }}
      {{- end }}
      {{- end }}  
```
