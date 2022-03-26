# Ansible Role: couchbaselabs.couchbase_fluent_bit

## Description

Deploy [Fluent Bit](https://fluentbit.io) for log parsing and shipping.  After installing the `fluent-bit` service, the role will apply a config based on [couchbase-fluent-bit](https://github.com/couchbase/couchbase-fluent-bit) container.

## Requirements

-   Ansible >= 2.10 (It might work on previous versions, but we cannot guarantee it)
-   jmespath on deployer machine. If you are using Ansible from a Python virtualenv, install *jmespath* to the same virtualenv via pip.
-   gnu-tar on Mac deployer host (`brew install gnu-tar`)

## Installing

The role can be installed directly from the git repository

```bash
ansible-galaxy install git+https://github.com/couchbaselabs/ansible-couchbase-fluent-bit.git
```

It can also be added to a `requirements.yml` file

```yaml
- src: https://github.com/couchbaselabs/ansible-couchbase-fluent-bit.git
  name: couchbaselabs.couchbase_fluent_bit
```

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

### Fluent Bit Variables

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `fluent_bit_storage_buffer_path` | /var/lib/fluent-bit/buffer | The filesystem location for Fluent Bit to buffer to if necessary |
| `fluent_bit_http_listen` | 0.0.0.0 | The address to listen on for HTTP requests, this is primarily used for metrics |
| `fluent_bit_http_port` | 2020 | The port that Fluent Bit listens for requests on |
| `fluent_bit_conf_dir` | /etc/fluent-bit | The default configuration directory, as the Fluent Bit is installed via yum this is the default location |
| `fluent_bit_conf_file` | fluent-bit.conf | The name of the configuration file to use |
| `fluent_bit_env_file` | service.env | To avoid changing values in the configuration files directly, they are made to use environment variables, with a large number of values it is easier to manage through an environment file. |
| `fluent_bit_user` | fluent-bit | The name of the Fluent Bit user, this is the username that is configured when the agent is installed through `yum` |
| `fluent_bit_user_group` | fluent-bit | The name of the Fluent Bit group, this is the group that is configured when the agent is installed through `yum`  |
| `fluent_bit_log_level` | `warn` | The Fluent Bit agent log level.  Allowed values are: off, error, warn, info, and debug. Values are accumulative, e.g: if 'debug' is set, it will include error, warning, info and debug. |
| `fluent_bit_mbl_couchbase_audit` | false | Memory Buffer Limit for Audit log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_erlang` | false | Memory Buffer Limit for Erlang logs parsing, disabled by default.  |
| `fluent_bit_mbl_couchbase_eventing` | false | Memory Buffer Limit for Eventing log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_http` | false | Memory Buffer Limit for HTTP log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_index_projector` | false | Memory Buffer Limit for Index and Projector log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_java` | false | Memory Buffer Limit for Analytics Java log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_memcached` | false | Memory Buffer Limit for Memcached log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_prometheus` | false | Memory Buffer Limit for Prometheus log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_rebalance` | false | Memory Buffer Limit for Rebalance log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_xdcr` | false | Memory Buffer Limit for XDCR log parsing, disabled by default. |

### Couchbase Variables

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `couchbase_username` | `Administrator` | Couchbase cluster username. This is necessary to get the actual name of the cluster, as it can only be retrieved from ns_server. |
| `couchbase_password` | `password` | Couchbase cluster password. This is necessary to get the actual name of the cluster, as it can only be retrieved from ns_server. |
| `couchbase_install_dir` | `/opt/couchbase` | The Couchbase installation directory. |
| `couchbase_cluster_name` | `null` | The name of the Couchbase Cluster, if not specified it will be automatically determined. |
| `couchbase_server_version` | `null` | The version and build of Couchbase Server, if not set it will automatically be determined. |
| `couchbase_logs_dir` | /opt/couchbase/var/lib/couchbase/logs | The location of the Couchbase logs directory |
| `couchbase_logs_rebalance_tmp_dir` | /opt/couchbase/var/lib/couchbase/logs/rebalance | The location of the Couchbase rebalance direcotry |
| `couchbase_user_group` | couchbase | The user group that couchbase is running in to grant access to the Fluent Bit user to read the logs |
| `couchbase_logs` | <ul><li>audit.log</li><li>babysitter.log</li><li>couchdb.log</li><li>debug.log</li><li>json\_rpc.log</li><li>mapreduce\_errors.log</li><li>metakv.log</li><li>ns\_couchdb.log</li><li>reports.log</li><li>eventing.log</li><li>fts.log</li><li>http.log</li><li>indexer.log</li><li>projector.log</li><li>analytics.log</li><li>memcached.log</li><li>prometheus.log</li><li>rebalance-report</li><li>xdcr.log</li></ul> | List of log files to monitor and process, the default is all |

## Output Variables

#### Standard Out

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `fluent_bit_stdout_match` | `couchbase.log.*` | The tags to match for outputting to stdout |

#### Loki

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `fluent_bit_loki_enabled` | `false` | Whether or not to enable Loki Output |
| `fluent_bit_loki_match` | `couchbase.log.*` | The tags to match when sending to Loki |
| `fluent_bit_loki_host` | `localhost` | The Loki host address |
| `fluent_bit_loki_port` | `3100` | The Loki host port |
| `fluent_bit_loki_workers` | `1` | The number of worker threads to use for Loki output |
| `fluent_bit_loki_http_user` | `null` | Set HTTP basic authentication user name if authentication is enabled |
| `fluent_bit_loki_http_passwd` | `null` | Set HTTP basic authentication password if authentication is enabled |
| `fluent_bit_loki_tenant_id` | `null` | Tenant ID used by default to push logs to Loki. If omitted or empty it assumes Loki is running in single-tenant mode and no X-Scope-OrgID header is sent. |
| `fluent_bit_loki_tls` | `off` | Whether or not to use TLS |
| `fluent_bit_loki_tls_verify` | `off` | Whether not TLS verification should be skipped |

[Docs Reference](https://docs.fluentbit.io/manual/pipeline/outputs/loki)

#### Splunk

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `fluent_bit_splunk_enabled` | `false` | Whether or not to enable Splunk Output |
| `fluent_bit_splunk_match` | `couchbase.log.*` | The tags to match when sending to Splunk |
| `fluent_bit_splunk_host` | `localhost` | The Splunk host address |
| `fluent_bit_splunk_port` | `8088` | The Splunk host port |
| `fluent_bit_splunk_token` | `null` | The Splunk Token  |
| `fluent_bit_splunk_channel` | `null` | The Splunk Channel to use |
| `fluent_bit_splunk_workers` | `2` | The number of worker threads to use for Splunk output |
| `fluent_bit_splunk_http_user` | `null` | Set HTTP basic authentication user name if authentication is enabled |
| `fluent_bit_splunk_http_passwd` | `null` | Set HTTP basic authentication password if authentication is enabled |
| `fluent_bit_splunk_tls` | `off` | Whether or not to use TLS |
| `fluent_bit_splunk_tls_verify` | `off` | Whether not TLS verification should be skipped |
| `fluent_bit_splunk_compress` | `null` | Whether or not to perform compression, the only valid value is `"gzip"` |

[Docs Reference](https://docs.fluentbit.io/manual/pipeline/outputs/splunk#configuration-parameters)
