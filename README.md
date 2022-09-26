# Couchbase Fluent Bit Ansible Role


<p align="center">

  <a href="https://github.com/couchbaselabs/ansible-couchbase-fluent-bit/graphs/contributors" alt="Contributors">
    <img src="https://img.shields.io/github/license/couchbaselabs/ansible-couchbase-fluent-bit" />
  </a>

  <!--
  <a href="https://galaxy.ansible.com/couchbaselabs/exporter" alt="Ansible Role">
    <img src="https://img.shields.io/ansible/role/{roleId}" />
  </a>

  <a href="https://galaxy.ansible.com/couchbaselabs/exporter" alt="Ansible Quality Score">
    <img src="https://img.shields.io/ansible/quality/{roleId}" />
  </a>

  <a href="https://galaxy.ansible.com/couchbaselabs/exporter" alt="Downloads">
    <img src="https://img.shields.io/ansible/role/d/{roleId}" />
  </a>
  -->

  <a href="https://github.com/couchbaselabs/ansible-couchbase-fluent-bit/releases" alt="GitHub tag (latest by date)">
    <img src="https://img.shields.io/github/v/tag/couchbaselabs/ansible-couchbase-fluent-bit" />
  </a>

  <a href="https://github.com/couchbaselabs/ansible-couchbase-fluent-bit/actions" alt="GitHub Workflow Status">
    <img src="https://img.shields.io/github/workflow/status/couchbaselabs/ansible-couchbase-fluent-bit/Lint" />
  </a>

  <a href="https://github.com/couchbaselabs/ansible-couchbase-fluent-bit/commits/main" alt="GitHub last commit">
    <img src="https://img.shields.io/github/last-commit/couchbaselabs/ansible-couchbase-fluent-bit" />
  </a>

  <a href="https://github.com/couchbaselabs/ansible-couchbase-fluent-bit/graphs/contributors" alt="GitHub last commit">
    <img src="https://img.shields.io/github/contributors/couchbaselabs/ansible-couchbase-fluent-bit" />
  </a>

</p>

## Description

Deploy [Fluent Bit](https://fluentbit.io) for log parsing and shipping.  After installing the `fluent-bit` service, the role will apply a config based on [couchbase-fluent-bit](https://github.com/couchbase/couchbase-fluent-bit) container.

## Disclaimer

This ansible role and the sub-components configured are not officially supported under Couchbase Enterprise Subscriptions. Please contact Couchbase on any details for your particular environment. Contents here and sub-components are provided as is, it is maintained through community contributions.  Any issues should be reported as an [Issue](https://github.com/couchbaselabs/ansible-couchbase-fluent-bit/issues) on Github.  Pull Requests are welcomed!

## Requirements

-   Ansible >= 2.10 (It might work on previous versions, but we cannot guarantee it)
-   jmespath on deployer machine. If you are using Ansible from a Python virtualenv, install *jmespath* to the same virtualenv via pip.
-   gnu-tar on Mac deployer host (`brew install gnu-tar`)

## Installing

The role can be installed directly from the git repository

```bash
ansible-galaxy role install \
  git+https://github.com/couchbaselabs/ansible-couchbase-fluent-bit.git,,couchbaselabs.couchbase_fluent_bit
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
| `fluent_bit_storage_buffer_path` | `/var/lib/fluent-bit/buffer` | The filesystem location for Fluent Bit to buffer to if necessary |
| `fluent_bit_http_listen` | `0.0.0.0` | The address to listen on for HTTP requests, this is primarily used for metrics |
| `fluent_bit_http_port` | `2020` | The port that Fluent Bit listens for requests on |
| `fluent_bit_conf_dir` | `/etc/fluent-bit` | The default configuration directory, as the Fluent Bit is installed via yum this is the default location |
| `fluent_bit_conf_file` | `fluent-bit.conf` | The name of the configuration file to use |
| `fluent_bit_env_file` | `service.env` | To avoid changing values in the configuration files directly, they are made to use environment variables, with a large number of values it is easier to manage through an environment file. |
| `fluent_bit_user` | `fluent-bit` | The name of the Fluent Bit user, this is the username that is configured when the agent is installed through `yum` |
| `fluent_bit_user_group` | `fluent-bit` | The name of the Fluent Bit group, this is the group that is configured when the agent is installed through `yum`  |
| `fluent_bit_user_shell` | `/usr/sbin/nologin` | By default `/usr/sbin/nologin` is used to prevent the user from logging in, if you're using an existing user account this should be `/bin/bash`   |
| `fluent_bit_user_createhome` | `false` | Whether or not to create the home directory for the user  |
| `fluent_bit_log_level` | `info` | The Fluent Bit agent log level.  Allowed values are: off, error, warn, info, and debug. Values are accumulative, e.g: if 'debug' is set, it will include error, warning, info and debug. |
| `fluent_bit_local_binary_path` | ` ` | Full path to the local binary if already downloaded or built on the controller this should only be set, if ansible is not downloading the binary and you have manually downloaded the binary |
| `fluent_bit_mbl_couchbase_audit` | false | Memory Buffer Limit for Audit log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_erlang` | false | Memory Buffer Limit for Erlang logs parsing, disabled by default.  |
| `fluent_bit_mbl_couchbase_eventing` | false | Memory Buffer Limit for Eventing log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_fts` | false | Memory Buffer Limit for FTS log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_http` | false | Memory Buffer Limit for HTTP log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_index` | false | Memory Buffer Limit for Index log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_projector` | false | Memory Buffer Limit for Projector log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_java` | false | Memory Buffer Limit for Analytics Java log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_memcached` | false | Memory Buffer Limit for Memcached log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_prometheus` | false | Memory Buffer Limit for Prometheus log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_rebalance` | false | Memory Buffer Limit for Rebalance log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_xdcr` | false | Memory Buffer Limit for XDCR log parsing, disabled by default. |
| `fluent_bit_mbl_couchbase_query` | false | Memory Buffer Limit for Query log parsing, disabled by default. |

### Couchbase Variables

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `couchbase_username` | `Administrator` | Couchbase cluster username. This is necessary to get the actual name of the cluster, as it can only be retrieved from ns_server. This is also used if `couchbase_slow_query_logging` is enabled |
| `couchbase_password` | `password` | Couchbase cluster password. This is necessary to get the actual name of the cluster, as it can only be retrieved from ns_server.  This is also used if `couchbase_slow_query_logging` is enabled  |
| `couchbase_cluster_name` | `null` | The name of the Couchbase Cluster, if not specified it will be automatically determined. |
| `couchbase_server_version` | `null` | The version and build of Couchbase Server, if not set it will automatically be determined. |
| `couchbase_logs_dir` | `/opt/couchbase/var/lib/couchbase/logs` | The location of the Couchbase logs directory |
| `couchbase_audit_logs_dir` | `/opt/couchbase/var/lib/couchbase/logs` | The location of the Couchbase Audit logs directory |
| `couchbase_logs_rebalance_tmp_dir` | `/opt/couchbase/var/lib/couchbase/logs/rebalance` | The location of the Couchbase rebalance directory |
| `fluent_bit_couchbase_user_group` | `couchbase` | The user group that couchbase is running in to grant access to the Fluent Bit user to read the logs |
| `couchbase_logs` | <ul><li>analytics.log</li><li>audit.log</li><li>babysitter.log</li><li>eventing.log</li><li>fts.log</li><li>http.log</li><li>indexer.log</li><li>memcached.log</li><li>projector.log</li><li>query.log</li><li>xdcr.log</li></ul> | List of log files to monitor and process |

The following log files are available:

-   `analytics.log`
-   `analytics_access.log`
-   `analytics_dcpdebug.log`
-   `analytics_dcp_failed_ingestion.log`
-   `audit.log`
-   `babysitter.log`
-   `couchdb.log`
-   `debug.log`
-   `eventing.log`
-   `fts.log`
-   `http.log`
-   `indexer.log`
-   `json_rpc.log`
-   `mapreduce_errors.log`
-   `memcached.log`
-   `metakv.log`
-   `ns_couchdb.log`
-   `projector.log`
-   `prometheus.log`
-   `query.log`
-   `rebalance-report`
-   `reports.log`
-   `xdcr.log`


### Couchbase Slow Query Logging Variables

Fluent Bit 1.9.3+ must be used when slow query logging is enabled.  The [Fluent Bit Exec Plugin](https://docs.fluentbit.io/manual/pipeline/inputs/exec) is used to execute the `query-logger` script at a 1min interval, which will retrieve any new slow queries from `system:completed_requests` on the local node.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `couchbase_slow_query_logging_enabled` | `false` | Whether or not to enable slow query logging |
| `couchbase_slow_query_exec_args` | ` ` | Any additional arguments to pass to the `query-logger`, run `/etc/fluent-bit/couchbase/scripts/query-logger --help` for all available options|
| `couchbase_slow_query_mode` | `exec` | Whether to use Fluent Bit Exec input or Tail a Log File.  Values are `"exec"` or `"tail"` |
| `couchbase_slow_query_log_path` | `{{ couchbase_logs_dir }}` | The path where to store the slow query logs |
| `couchbase_slow_query_cron_user` | `root` | If the slow query mode is "tail" the user to schedule the crontab to run as. |
| `couchbase_slow_query_logrotate_path` | `/etc/logrotate.d` | The path where to create the logrotate entry.  If this is not `/etc/logrotate.d` then it is recommended that you create your own `crontab` entry to call log rotate with the path to your config. |
| `couchbase_slow_query_logrotate_config` | [see `./defaults/main.yml`](./defaults/main.yml) | The contents of the `logrotate` file |

## Output Variables

The following output tags/patterns are available for matching:

-   `couchbase.log.<log-name>`
-   `couchbase.log-level.<log-level>.<log-name>`
-   `couchbase.audit.messages`
-   `couchbase.audit.bucket.<name-of-bucket>`
-   `couchbase.problems`

The following log names are available:

-   analytics.log
-   audit.log
-   babysitter.log
-   couchdb.log
-   debug.log
-   eventing.log
-   fts.log
-   http.log
-   indexer.log
-   json_rpc.log
-   mapreduce_errors.log
-   memcached.log
-   metakv.log
-   ns_couchdb.log
-   projector.log
-   prometheus.log
-   query.log
-   rebalance-report
-   reports.log
-   slow_query.log
-   xdcr.log


The following log levels  are available:

-   `DEBUG`
-   `INFO`
-   `WARN`
-   `ERROR`
-   `UNKNOWN`

The following is example tags:

Output All Logs

```bash
couchbase.log.*
```

Output only slow_query and audit logs

```bash
couchbase.log.slow_query,couchbase.log.audit
```

Output only Errors and Warnings

```bash
couchbase.log-levels.WARN.*,couchbase.log-levels.ERROR.*
```

Output Errors and Warnings, as well as slow_query and audit

```bash
couchbase.log-levels.WARN.*,couchbase.log-levels.ERROR.*,couchbase.log.slow_query,couchbase.log.audit
```

#### Standard Out

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `fluent_bit_stdout_enabled` | `false` | Whether or not to enable output to stdout |
| `fluent_bit_stdout_match` | `none.*` | The tags to match for outputting to stdout.  This is useful for validating log messages, but it is advised to not output everything as this can fill up the system logs. |

#### cbhealthagent output

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `fluent_bit_cbhealthagent_enabled` | `false` | Whether or not to output log messages to `cbhealthagent` running on the local node |
| `fluent_bit_cbhealthagent_match` | `couchbase.problems` | The tags to match for outputting to `cbhealthagent` |
| `fluent_bit_cbhealthagent_log_port` | `9084` | The TCP port that `cbhealthagent` is listening on |

#### Loki

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `fluent_bit_loki_enabled` | `false` | Whether or not to enable Loki Output |
| `fluent_bit_loki_match` | `couchbase.log.*` | The tags to match when sending to Loki.  |
| `fluent_bit_loki_host` | `localhost` | The Loki host address |
| `fluent_bit_loki_port` | `3100` | The Loki host port |
| `fluent_bit_loki_workers` | `1` | The number of worker threads to use for Loki output |
| `fluent_bit_loki_http_user` | `null` | Set HTTP basic authentication user name if authentication is enabled |
| `fluent_bit_loki_http_passwd` | `null` | Set HTTP basic authentication password if authentication is enabled |
| `fluent_bit_loki_tenant_id` | `null` | Tenant ID used by default to push logs to Loki. If omitted or empty it assumes Loki is running in single-tenant mode and no X-Scope-OrgID header is sent. |
| `fluent_bit_loki_tls` | `"off"` | Whether or not to use TLS |
| `fluent_bit_loki_tls_verify` | `"off"` | Whether not TLS verification should be skipped |

[Docs Reference](https://docs.fluentbit.io/manual/pipeline/outputs/loki)

Consider the following match to ship all `WARN` and `ERROR` messages, along with all memcached.log, audit.log and slow_query.log messages.

```
couchbase.log-level.WARN.*,couchbase.log-level.ERROR.*,couchbase.log.slow_query,couchbase.audit.log
```


#### Splunk

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `fluent_bit_splunk_enabled` | `false` | Whether or not to enable Splunk Output |
| `fluent_bit_splunk_match` | `couchbase.log.*` | The tags to match when sending to Splunk |
| `fluent_bit_splunk_match_regex` | ` ` | The tags to match when sending to Splunk |
| `fluent_bit_splunk_host` | `localhost` | The Splunk host address |
| `fluent_bit_splunk_port` | `8088` | The Splunk host port |
| `fluent_bit_splunk_token` | `null` | The Splunk Token  |
| `fluent_bit_splunk_channel` | `null` | The Splunk Channel to use |
| `fluent_bit_splunk_workers` | `2` | The number of worker threads to use for Splunk output |
| `fluent_bit_splunk_http_user` | `null` | Set HTTP basic authentication user name if authentication is enabled |
| `fluent_bit_splunk_http_passwd` | `null` | Set HTTP basic authentication password if authentication is enabled |
| `fluent_bit_splunk_tls` | `"off"` | Whether or not to use TLS |
| `fluent_bit_splunk_tls_verify` | `"off"` | Whether not TLS verification should be skipped |
| `fluent_bit_splunk_compress` | `null` | Whether or not to perform compression, the only valid value is `"gzip"` |

[Docs Reference](https://docs.fluentbit.io/manual/pipeline/outputs/splunk#configuration-parameters)
