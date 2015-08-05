BOSH release to run logstash
=======================

Background
----------

### What is logstash?

Logstash is a tool for receiving, processing and outputting logs. All kinds of logs. System logs, webserver logs, error logs, application logs, and just about anything you can throw at it.

Usage
-----

To use this bosh release, first upload it to your bosh:

```
bosh upload release https://github.com/hybris/logstash-boshrelease
```

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a cluster:

```
templates/make_manifest warden
bosh -n deploy
```

For AWS EC2, create a single VM:

```
templates/make_manifest aws-ec2
bosh -n deploy
```

### Override security groups

For AWS & Openstack, the default deployment assumes there is a `default` security group. If you wish to use a different security group(s) then you can pass in additional configuration when running `make_manifest` above.

Create a file `my-networking.yml`:

```yaml
---
networks:
  - name: logstash1
    type: dynamic
    cloud_properties:
      security_groups:
        - logstash
```

Where `- logstash` means you wish to use an existing security group called `logstash`.

You now suffix this file path to the `make_manifest` command:

```
templates/make_manifest openstack-nova my-networking.yml
bosh -n deploy
```
