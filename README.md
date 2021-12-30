![zenanaboards](zenarmor-kibana-dashboards.png "zenanaboards")
# Zenarmor Kibana Dashboards

Here are dashboards for Kibana from the Elastic Stack, which you can use to visualize the different outputs from the Zenarmor (Sensei) plugin in OPNsense, through an external Elasticsearch database. This greatly leveraged CPU usage on my OPNsense firewall, although I do have to buy some more RAM because the swap usage is through the roof. 

## Prerequisites

OPNsense
- 21.7.3_3-amd64

Sensei 
- Engine Version: 1.9.3 
- UI Version:	21.9.14
- Database Version:	1.9.21091012

Elastic Stack:
- kibana 7.15.0
- elasticsearch 7.15.0

## Installation:
### Configure Elasticsearch
Create a user with Role permissions to create Indicies in Elasticsearch. You can do this through Kibana. 

#### Role
Role name:
- zenarmor_write

Cluster privileges:
- manage_index_templates
- manage_ilm
- monitor

Under Index privileges add Indicies as custom option:
- alert*
- conn*
- dns*
- http*
- sip*
- tls*

With these Privileges: 
- all

*- write*
*- delete*
*- manage*
*- manage_ilm*
*- create_index*
*- auto_configure*


#### User
Add a user with the role `zenarmor_write`. Note the password, you will use it in OPNsense. 

#### elasticsearch.yml
Remember to change `network.host` in `elasticsearch.yml` to an appropriate value to allow external connections. 

### Configure Zenarmor (Sensei)
Configure Zenarmor (Sensei) in OPNsense to use an external database (elasticsearch). If you have Zenarmor (Sensei) already installed, uninstall and install again to be able to select an external database. 

A check to see if everything is working OK is to see if you are able to view any Reports or click on any Dashboards from OPNsense, since it will then grab data from Elasticsearch. Another thing to look for is to look after indicies through Kibana, which will have been created from the Zenarmor plugin. The indicies will look like something like this: 

- alert-211018
- conn-211017

etc. .

### Configure Kibana
Create Index patterns in Kibana (Stack Management > Kibana > Index Patterns > "Create index pattern") and match it:
- alert-*
- conn-*
- dns-*
- http-*
- sip-*
- tls-*

Use `start_time` as Timestamp field. Select Create index pattern. 

Import the dashboards through Kibana (Stack Management > Kibana > Saved Objects > "Import")

- dashboards/zenarmor-dashboard-blocks-7.15.0.ndjson
- dashboards/zenarmor-dashboard-connections-7.15.0.ndjson
- dashboards/zenarmor-dashboard-dns-7.15.0.ndjson
- dashboards/zenarmor-dashboard-threats-7.15.0.ndjson
- dashboards/zenarmor-dashboard-tls-7.15.0.ndjson
- dashboards/zenarmor-dashboard-web-7.15.0.ndjson

After each consequent import, you'll notice you have some overwrites - but that is only the tags beiing overwritten.
After some time, your dashboards will start working. 

### Changelog

Trying to just smooth things over as I work with this.

## How to use it:

Analytics > Dashboard 

## Documentation

For documentation and a more thorough installation guide (manual installation on Ubuntu), see (coming soon - I hope).


## Contributing

There are many ways to contribute:
- Fix and [report bugs]
- Improve documentation

## Disclaimer

While I try to keep the information timely and accurate, I make no guarantees. I will make an effort to correct errors brought to my attention. Probably there are some logical errors already in the dashboards. 