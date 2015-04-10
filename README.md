
Automation to setup a performance test cluster on AWS with:

* Couchbase Server nodes
* Sync Gateway nodes
* Gateload and/or Gatling nodes

Uses a Cloudformation template to spin up all instances.

## How to generate CloudFormation template

This uses [troposphere](https://github.com/cloudtools/troposphere) to generate the Cloudformation template (a json file).

The Cloudformation config is declared via a Python DSL, which then generates the Cloudformation Json.

One time setup:

```
$ pip install troposphere
$ pip install boto
```

Generate template after changes to the python file:

```
$ python cloudformation_template.py > cloudformation_template.json
```

## Install steps

* `ansible-playbook install-go.yml` 
* `ansible-playbook build-sync-gateway.yml`
* `ansible-playbook build-gateload.yml`  
* `ansible-playbook install-sync-gateway-service.yml`
* Manually update via Couchbase web ui 
    * Join all couchbase server nodes into cluster
    * Rebalance
    * Create buckets: bucket-1 and bucket-2
* Manually update cache reader configs in github to have couchbase ip 
    * Cache reader config
* `ansible-playbook get-sync-gateway-configs.yml` # TODO -- will pull from github
* Manually Ssh into cache writer machine, change config to cache writer == true 
* `ansible-playbook start-sync-gateway.yml`
* `ansible-playbook get-gateload-configs.yml`  # TODO
* Hand modify each gateload config to have correct sync gateway ip
* Kick off gateloads

