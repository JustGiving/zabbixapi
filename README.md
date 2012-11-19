#Ruby Zabbix Api Module

Simple and lightweight ruby module for work with zabbix api 

[![Build Status](https://travis-ci.org/vadv/zabbixapi.png)](https://travis-ci.org/vadv/zabbixapi)

## Installation
```
gem install zabbixapi
```

## Get Start

### Connect
```ruby
zbx = ZabbixApi.connect(
  :url => 'http://localhost/zabbix/api_jsonrpc.php',
  :user => 'Admin',
  :password => 'zabbix'
)
```
### Create Hostgroup
```ruby
zbx.hostgroups.create(:name => "hostgroup")
```

### Create Template
```ruby
zbx.templates.create(
  :host => "template",
  :groups => [:groupid => zbx.hostgroups.get_id(:name => "hostgroup")]
)
```

### Create Application
```ruby
zbx.applications.create(
  :name => application,
  :hostid => zbx.templates.get_id(:host => "template")
)
```

### Create Item
```ruby
zbx.items.create(
  :description => "item",
  :key_ => "proc.num[aaa]",
  :hostid => zbx.templates.get_id(:host => "template"),
  :applications => [zbx.applications.get_id(:name => "application")]
)
```

### Create host
```ruby
zbx.hosts.add(
  :host => "hostname",
  :groups => [ :groupid => zbx.hostgroups.get_id(:name => "hostgroup") ]
)
```

### Get all templates linked with host
```ruby
zbx.templates.get_ids_by_host( :hostids => [zbx.hosts.get_id(:host => "hostname")] )
returned hash:
{
  "Templatename" => "10",
  "Templatename" => "1021"
}
``` 

### Link host with templates
```ruby
zbx.hosts.unlink_templates(
  :hosts_id => [zbx.hosts.get_id(:host => "hostname")],
  :templates_id => [111, 214]
)
```

### Create trigger
```ruby
zbx.triggers.create(
  :description => "trigger",
  :expression => "{template:proc.num[aaa].last(0)}<1",
  :comments => "Bla-bla is faulty (disaster)",
  :priority => 5,
  :status     => 0,
  :templateid => 0,
  :type => 0
 )
````

### Create user
```ruby
zbx.users.create(
  :alias => "Test user",
  :name => "username",
  :surname => "usersername",
  :passwd => "password"
)
```

### Custom queries
```ruby
zbx.query(
  :method => "apiinfo.version", 
  :params => {}
)
```

## Dependencies

* net/http
* net/https
* json

## Zabbix documentation

* Zabbix Project Homepage -> http://zabbix.com/
* Zabbix Api docs -> http://www.zabbix.com/documentation/1.8/api