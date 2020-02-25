Stolon + Consul roles
=====

Роли для разворачивания кластера PostgreSQL + Stolon & Consul

Предполагается 6 узлов с СУБД (2 региональные площадки).

В инвентори необходимо добавить узлы consul, stolon примерно следующим образом:
``
[db_servers]
sdfA ansible_host="db-node1"
sdfB ansible_host="db-node2"
sdfC ansible_host="db-node3"

[db_servers:vars]
consul_bootstrap_count=5

[db_servers_replica]
sdfAreplica ansible_host="db-node4"
sdfBreplica ansible_host="db-node5"
sdfCreplica ansible_host="db-node6"

[db_servers_replica:vars]
department='depB'
consul_bootstrap_count=5

[consul_servers:children]
db_servers
db_servers_replica

[consul_clients:children]
web_servers

[consul_members:children]
consul_servers
consul_clients

[all_servers:vars]
department='depA'
platform_type='live'
neighbor_department='depB'

``
