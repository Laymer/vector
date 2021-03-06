---
delivery_guarantee: "at_least_once"
description: "The Vector `kafka` sink streams `log` events to Apache Kafka via the Kafka protocol."
event_types: ["log"]
issues_url: https://github.com/timberio/vector/issues?q=is%3Aopen+is%3Aissue+label%3A%22sink%3A+kafka%22
operating_systems: ["Linux","MacOS","Windows"]
sidebar_label: "kafka|[\"log\"]"
source_url: https://github.com/timberio/vector/tree/master/src/sinks/kafka.rs
status: "prod-ready"
title: "Kafka Sink"
unsupported_operating_systems: []
---

The Vector `kafka` sink [streams](#streaming) [`log`][docs.data-model.log] events to [Apache Kafka][urls.kafka] via the [Kafka protocol][urls.kafka_protocol].

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the template located at:

     website/docs/reference/sinks/kafka.md.erb
-->

## Configuration

import Tabs from '@theme/Tabs';

<Tabs
  block={true}
  defaultValue="common"
  values={[
    { label: 'Common', value: 'common', },
    { label: 'Advanced', value: 'advanced', },
  ]
}>

import TabItem from '@theme/TabItem';

<TabItem value="common">

import CodeHeader from '@site/src/components/CodeHeader';

<CodeHeader fileName="vector.toml" learnMoreUrl="/docs/setup/configuration/"/ >

```toml
[sinks.my_sink_id]
  type = "kafka" # must be: "kafka"
  inputs = ["my-source-id"] # example
  bootstrap_servers = "10.14.22.123:9092,10.14.23.332:9092" # example
  key_field = "user_id" # example
  topic = "topic-1234" # example
```

</TabItem>
<TabItem value="advanced">

<CodeHeader fileName="vector.toml" learnMoreUrl="/docs/setup/configuration/" />

```toml
[sinks.my_sink_id]
  # REQUIRED - General
  type = "kafka" # must be: "kafka"
  inputs = ["my-source-id"] # example
  bootstrap_servers = "10.14.22.123:9092,10.14.23.332:9092" # example
  key_field = "user_id" # example
  topic = "topic-1234" # example

  # OPTIONAL - General
  endpoint = "127.0.0.0:5000" # example, no default
  healthcheck = true # default
  region = "us-east-1" # example, no default

  # OPTIONAL - requests
  encoding = "json" # example, no default, enum

  # OPTIONAL - Buffer
  [sinks.my_sink_id.buffer]
    type = "memory" # default, enum
    max_events = 500 # default, events, relevant when type = "memory"
    max_size = 104900000 # example, no default, bytes, relevant when type = "disk"
    when_full = "block" # default, enum

  # OPTIONAL - Tls
  [sinks.my_sink_id.tls]
    ca_path = "/path/to/certificate_authority.crt" # example, no default
    crt_path = "/path/to/host_certificate.crt" # example, no default
    enabled = false # default
    key_pass = "PassWord1" # example, no default
    key_path = "/path/to/host_certificate.key" # example, no default
```

</TabItem>

</Tabs>

## Options

import Fields from '@site/src/components/Fields';

import Field from '@site/src/components/Field';

<Fields filters={true}>


<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["10.14.22.123:9092,10.14.23.332:9092"]}
  name={"bootstrap_servers"}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  >

### bootstrap_servers

A comma delimited list of host and port pairs that the Kafka client should contact to bootstrap its cluster metadata.


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[]}
  name={"buffer"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"table"}
  unit={null}
  >

### buffer

Configures the sink buffer behavior.

<Fields filters={false}>


<Field
  common={false}
  defaultValue={500}
  enumValues={null}
  examples={[500]}
  name={"max_events"}
  path={"buffer"}
  relevantWhen={{"type":"memory"}}
  required={false}
  templateable={false}
  type={"int"}
  unit={"events"}
  >

#### max_events

The maximum number of [events][docs.data-model#event] allowed in the buffer.


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[104900000]}
  name={"max_size"}
  path={"buffer"}
  relevantWhen={{"type":"disk"}}
  required={false}
  templateable={false}
  type={"int"}
  unit={"bytes"}
  >

#### max_size

The maximum size of the buffer on the disk.


</Field>


<Field
  common={false}
  defaultValue={"memory"}
  enumValues={{"memory":"Stores the sink's buffer in memory. This is more performant (~3x), but less durable. Data will be lost if Vector is restarted abruptly.","disk":"Stores the sink's buffer on disk. This is less performance (~3x),  but durable. Data will not be lost between restarts."}}
  examples={["memory","disk"]}
  name={"type"}
  path={"buffer"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### type

The buffer's type / location. `disk` buffers are persistent and will be retained between restarts.


</Field>


<Field
  common={false}
  defaultValue={"block"}
  enumValues={{"block":"Applies back pressure when the buffer is full. This prevents data loss, but will cause data to pile up on the edge.","drop_newest":"Drops new data as it's received. This data is lost. This should be used when performance is the highest priority."}}
  examples={["block","drop_newest"]}
  name={"when_full"}
  path={"buffer"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### when_full

The behavior when the buffer becomes full.


</Field>


</Fields>

</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={{"json":"Each event is encoded into JSON and the payload is represented as a JSON array.","text":"Each event is encoded into text via the `message` key and the payload is new line delimited."}}
  examples={["json","text"]}
  name={"encoding"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

### encoding

The encoding format used to serialize the events before outputting.


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["127.0.0.0:5000"]}
  name={"endpoint"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

### endpoint

Custom endpoint for use with AWS-compatible services. Providing a value for this option will make [`region`](#region) moot.


</Field>


<Field
  common={false}
  defaultValue={true}
  enumValues={null}
  examples={[true,false]}
  name={"healthcheck"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"bool"}
  unit={null}
  >

### healthcheck

Enables/disables the sink healthcheck upon start. See [Health Checks](#health-checks) for more info.


</Field>


<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["user_id"]}
  name={"key_field"}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  >

### key_field

The log field name to use for the topic key. If unspecified, the key will be randomly generated. If the field does not exist on the log, a blank value will be used.


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["us-east-1"]}
  name={"region"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

### region

The [AWS region][urls.aws_regions] of the target service. If [`endpoint`](#endpoint) is provided it will override this value since the endpoint includes the region.


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[]}
  name={"tls"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"table"}
  unit={null}
  >

### tls

Configures the TLS options for connections from this sink.

<Fields filters={false}>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["/path/to/certificate_authority.crt"]}
  name={"ca_path"}
  path={"tls"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### ca_path

Absolute path to an additional CA certificate file, in DER or PEM format (X.509).


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["/path/to/host_certificate.crt"]}
  name={"crt_path"}
  path={"tls"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### crt_path

Absolute path to a certificate file used to identify this connection, in DER or PEM format (X.509) or PKCS#12. If this is set and is not a PKCS#12 archive, [`key_path`](#key_path) must also be set.


</Field>


<Field
  common={false}
  defaultValue={false}
  enumValues={null}
  examples={[false,true]}
  name={"enabled"}
  path={"tls"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"bool"}
  unit={null}
  >

#### enabled

Enable TLS during connections to the remote.


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["PassWord1"]}
  name={"key_pass"}
  path={"tls"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### key_pass

Pass phrase used to unlock the encrypted key file. This has no effect unless [`key_pass`](#key_pass) above is set.


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["/path/to/host_certificate.key"]}
  name={"key_path"}
  path={"tls"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

#### key_path

Absolute path to a certificate key file used to identify this connection, in DER or PEM format (PKCS#8). If this is set, [`crt_path`](#crt_path) must also be set.


</Field>


</Fields>

</Field>


<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["topic-1234"]}
  name={"topic"}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  >

### topic

The Kafka topic name to write events to.


</Field>


</Fields>

## Env Vars

<Fields filters={true}>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["AKIAIOSFODNN7EXAMPLE"]}
  name={"AWS_ACCESS_KEY_ID"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

### AWS_ACCESS_KEY_ID

Used for AWS authentication when communicating with AWS services. See relevant [AWS components][pages.aws_components] for more info. See [AWS Authentication](#aws-authentication) for more info.


</Field>


<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["wJalrXUtnFEMI/K7MDENG/FD2F4GJ"]}
  name={"AWS_SECRET_ACCESS_KEY"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  >

### AWS_SECRET_ACCESS_KEY

Used for AWS authentication when communicating with AWS services. See relevant [AWS components][pages.aws_components] for more info. See [AWS Authentication](#aws-authentication) for more info.


</Field>


</Fields>

## How It Works

### AWS Authentication

Vector checks for AWS credentials in the following order:

1. Environment variables `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.
2. The [`credential_process` command][urls.aws_credential_process] in the AWS config file. (usually located at `~/.aws/config`)
3. The [AWS credentials file][urls.aws_credentials_file]. (usually located at `~/.aws/credentials`)
4. The [IAM instance profile][urls.iam_instance_profile]. (will only work if running on an EC2 instance with an instance profile/role)

If credentials are not found the [healtcheck](#healthchecks) will fail and an
error will be [logged][docs.monitoring#logs].

#### Obtaining an access key

In general, we recommend using instance profiles/roles whenever possible. In
cases where this is not possible you can generate an AWS access key for any user
within your AWS account. AWS provides a [detailed guide][urls.aws_access_keys] on
how to do this.

### Environment Variables

Environment variables are supported through all of Vector's configuration.
Simply add `${MY_ENV_VAR}` in your Vector configuration file and the variable
will be replaced before being evaluated.

You can learn more in the [Environment Variables][docs.configuration#environment-variables]
section.

### Health Checks

Health checks ensure that the downstream service is accessible and ready to
accept data. This check is performed upon sink initialization.
If the health check fails an error will be logged and Vector will proceed to
start.

#### Require Health Checks

If you'd like to exit immediately upon a health check failure, you can
pass the `--require-healthy` flag:

```bash
vector --config /etc/vector/vector.toml --require-healthy
```

#### Disable Health Checks

If you'd like to disable health checks for this sink you can set the
`healthcheck` option to `false`.

### Streaming

The `kafka` sink streams data on a real-time
event-by-event basis. It does not batch data.


[docs.configuration#environment-variables]: /docs/setup/configuration/#environment-variables
[docs.data-model#event]: /docs/about/data-model/#event
[docs.data-model.log]: /docs/about/data-model/log/
[docs.monitoring#logs]: /docs/administration/monitoring/#logs
[pages.aws_components]: /components?providers%5B%5D=aws/
[urls.aws_access_keys]: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html
[urls.aws_credential_process]: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sourcing-external.html
[urls.aws_credentials_file]: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html
[urls.aws_regions]: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html
[urls.iam_instance_profile]: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html
[urls.kafka]: https://kafka.apache.org/
[urls.kafka_protocol]: https://kafka.apache.org/protocol
