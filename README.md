# Amazon Web Services (AWS) Advanced Python Driver

[![Build Status](https://github.com/awslabs/aws-advanced-python-wrapper/actions/workflows/main.yml/badge.svg?branch=main)](https://github.com/awslabs/aws-advanced-python-wrapper/actions/workflows/main.yml)
[![Integration Tests](https://github.com/awslabs/aws-advanced-python-wrapper/actions/workflows/integration_tests.yml/badge.svg?branch=main)](https://github.com/awslabs/aws-advanced-python-wrapper/actions/workflows/integration_tests.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

## About the Driver

Hosting a database cluster in the cloud via Aurora is able to provide users with sets of features and configurations to obtain maximum performance and availability, such as database failover. However, at the moment, most existing drivers do not currently support those functionalities or are not able to entirely take advantage of it.

The main idea behind the AWS Advanced Python Driver is to add a software layer on top of an existing Python driver that would enable all the enhancements brought by Aurora, without requiring users to change their workflow with their databases and existing Python drivers.

### What is Failover?

In an Amazon Aurora database cluster, **failover** is a mechanism by which Aurora automatically repairs the cluster status when a primary DB instance becomes unavailable. It achieves this goal by electing an Aurora Replica to become the new primary DB instance, so that the DB cluster can provide maximum availability to a primary read-write DB instance. The AWS Advanced Python Driver is designed to understand the situation and coordinate with the cluster in order to provide minimal downtime and allow connections to be very quickly restored in the event of a DB instance failure.

### Benefits of the AWS Advanced Python Driver

Although Aurora is able to provide maximum availability through the use of failover, existing client drivers do not currently support this functionality. This is partially due to the time required for the DNS of the new primary DB instance to be fully resolved in order to properly direct the connection. The AWS Advanced Python Driver allows customers to continue using their existing community drivers in addition to having the AWS Advanced Python Driver fully exploit failover behavior by maintaining a cache of the Aurora cluster topology and each DB instance's role (Aurora Replica or primary DB instance). This topology is provided via a direct query to the Aurora DB, essentially providing a shortcut to bypass the delays caused by DNS resolution. With this knowledge, the AWS Advanced Python Driver can more closely monitor the Aurora DB cluster status so that a connection to the new primary DB instance can be established as fast as possible.

### Enhanced Failure Monitoring

Since a database failover is usually identified by reaching a network or a connection timeout, the AWS Advanced Python Driver introduces an enhanced and customizable manner to faster identify a database outage.

Enhanced Failure Monitoring (EFM) is a feature available from the [Host Monitoring Connection Plugin](./docs/using-the-python-driver/using-plugins/UsingTheHostMonitoringPlugin.md#enhanced-failure-monitoring) that periodically checks the connected database host's health and availability. If a database host is determined to be unhealthy, the connection is aborted (and potentially routed to another healthy host in the cluster).

### Using the AWS Advanced Python Driver with plain RDS databases

The AWS Advanced Python Driver also works with RDS provided databases that are not Aurora.

Please visit [this page](./docs/using-the-python-driver/UsingThePythonDriver.md#using-the-aws-python-driver-with-plain-rds-databases) for more information.

## Getting Started

To start using the driver with Psycopg, you need to pass Psycopg's connect function to the `AwsWrapperConnection#connect` method as shown in the following example:

```python
from aws_advanced_python_wrapper import AwsWrapperConnection
from psycopg import Connection

with AwsWrapperConnection.connect(
        Connection.connect,
        "host=database.cluster-xyz.us-east-1.rds.amazonaws.com dbname=db user=john password=pwd",
        plugins="failover",
        wrapper_dialect="aurora-pg",
        autocommit=True
) as awsconn:
    awscursor = awsconn.cursor()
    awscursor.execute("SELECT aurora_db_instance_identifier()")
    awscursor.fetchone()
    for record in awscursor:
        print(record)
```
The `AwsWrapperConnection#connect` method accepts the connection configuration through both the connection string and the keyword arguments.

You can either pass the connection configuration entirely through the connection string, entirely though the keyword arguments, or through a mixture of both.

To use the driver with MySQL Connection/Python, see the following example:

```python
from aws_advanced_python_wrapper import AwsWrapperConnection
from mysql.connector import Connect

with AwsWrapperConnection.connect(
        Connect,
        "host=database.cluster-xyz.us-east-1.rds.amazonaws.com database=db user=john password=pwd",
        plugins="failover",
        wrapper_dialect="aurora-mysql",
        autocommit=True
) as awsconn:
    awscursor = awsconn.cursor()
    awscursor.execute("SELECT @@aurora_server_id")
    awscursor.fetchone()
    for record in awscursor:
        print(record)
```

For more details on how to download the AWS Advanced Python Driver, minimum requirements to use it, 
and how to integrate it within your project and with your Python driver of choice, please visit the 
[Getting Started page](./docs/GettingStarted.md).

### Connection Properties
The following table lists the common connection properties used with the AWS Advanced Python Wrapper.

| Parameter                                    |                                                   Documentation Link                                                    |
|----------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------:|
| `wrapper_dialect`                            |            [Dialects](./docs/using-the-python-driver/DatabaseDialects.md), and whether you should include it.            |
| `plugins`                                    | [Connection Plugin Manager](./docs/using-the-python-driver/UsingThePythonDriver.md#connection-plugin-manager-parameters) |
| `secrets_manager_secret_id`                  |         [SecretsManagerPlugin](./docs/using-the-python-driver/using-plugins/UsingTheAwsSecretsManagerPlugin.md)         |
| `secrets_manager_region`                     |         [SecretsManagerPlugin](./docs/using-the-python-driver/using-plugins/UsingTheAwsSecretsManagerPlugin.md)         |
| `enable_failover`                            |                [FailoverPlugin](./docs/using-the-python-driver/using-plugins/UsingTheFailoverPlugin.md)                 |
| `failover_mode`                              |                [FailoverPlugin](./docs/using-the-python-driver/using-plugins/UsingTheFailoverPlugin.md)                 |
| `cluster_instance_host_pattern`              |                [FailoverPlugin](./docs/using-the-python-driver/using-plugins/UsingTheFailoverPlugin.md)                 |
| `failover_cluster_topology_refresh_rate_sec` |                [FailoverPlugin](./docs/using-the-python-driver/using-plugins/UsingTheFailoverPlugin.md)                 |
| `failover_reader_connect_timeout_sec`        |                [FailoverPlugin](./docs/using-the-python-driver/using-plugins/UsingTheFailoverPlugin.md)                 |
| `failover_timeout_sec`                       |                [FailoverPlugin](./docs/using-the-python-driver/using-plugins/UsingTheFailoverPlugin.md)                 |
| `failover_writer_reconnect_interval_sec`     |                [FailoverPlugin](./docs/using-the-python-driver/using-plugins/UsingTheFailoverPlugin.md)                 |
| `failure_detection_count`                    |          [HostMonitoringPlugin](./docs/using-the-python-driver/using-plugins/UsingTheHostMonitoringPlugin.md)           |
| `failure_detection_enabled`                  |          [HostMonitoringPlugin](./docs/using-the-python-driver/using-plugins/UsingTheHostMonitoringPlugin.md)           |
| `failure_detection_interval_ms`              |          [HostMonitoringPlugin](./docs/using-the-python-driver/using-plugins/UsingTheHostMonitoringPlugin.md)           |
| `failure_detection_time_ms`                  |          [HostMonitoringPlugin](./docs/using-the-python-driver/using-plugins/UsingTheHostMonitoringPlugin.md)           |
| `monitor_disposal_time_ms`                   |          [HostMonitoringPlugin](./docs/using-the-python-driver/using-plugins/UsingTheHostMonitoringPlugin.md)           |
| `iam_default_port`                           |       [IamAuthenticationPlugin](./docs/using-the-python-driver/using-plugins/UsingTheIamAuthenticationPlugin.md)        |
| `iam_host`                                   |       [IamAuthenticationPlugin](./docs/using-the-python-driver/using-plugins/UsingTheIamAuthenticationPlugin.md)        |
| `iam_region`                                 |       [IamAuthenticationPlugin](./docs/using-the-python-driver/using-plugins/UsingTheIamAuthenticationPlugin.md)        |
| `iam_expiration`                             |       [IamAuthenticationPlugin](./docs/using-the-python-driver/using-plugins/UsingTheIamAuthenticationPlugin.md)        |

### Using the AWS Advanced Python Driver

Technical documentation regarding the functionality of the AWS Advanced Python Driver will be maintained in this GitHub repository. Since the AWS Advanced Python Driver requires an underlying Python driver, please refer to the individual driver's documentation for driver-specific information.
To find all the documentation and concrete examples on how to use the AWS Advanced Python Driver, please refer to the [AWS Advanced Python Driver Documentation](./docs/Documentation.md) page.

### Known Limitations

#### Amazon RDS Blue/Green Deployments

This driver currently does not support switchover in Amazon RDS Blue/Green Deployments. In order to execute a Blue/Green deployment with the driver,
please ensure your application is coded to retry the database connection. Retry will allow the driver to re-establish a connection to an available
database instance. Without a retry, the driver will not be able to identify an available database instance after  blue/green switchover has occurred.

#### MySQL Connector/Python C Extension

When connecting to Aurora MySQL clusters, it is recommended to use the Python implementation of the MySQL Connector/Python driver by setting the `use_pure` connection argument to `True`.
The AWS Advanced Python Driver internally calls the MySQL Connector/Python's `is_connected` method to verify the connection. The [MySQL Connector/Python's C extension](https://dev.mysql.com/doc/connector-python/en/connector-python-cext.html) uses a network blocking implementation of the `is_connected` method.
In the event of a network failure where the host can no longer be reached, the `is_connected` call may hang indefinitely and will require users to forcibly interrupt the application.

#### MySQL Support for the IAM Authentication Plugin

The official MySQL Connector/Python [offers a Python implementation and a C implementation](https://dev.mysql.com/doc/connector-python/en/connector-python-example-connecting.html#:~:text=Using%20the%20Connector/Python%20Python%20or%20C%20Extension) of the driver that can be toggled using the `use_pure` connection argument.
The [IAM Authentication Plugin](./docs/using-the-python-driver/using-plugins/UsingTheIamAuthenticationPlugin.md) is incompatible with the Python implementation of the driver due to its 255-character password limit.
The IAM Authentication Plugin generates a temporary AWS IAM token to authenticate users. Passing this token to the Python implementation of the driver will result in the following error:
`struct.error: ubyte format requires 0 <= number <= 255`. To avoid this error, we recommend you set `use_pure` to `False` when using the IAM Authentication Plugin.
However, as noted in the [MySQL Connector/Python C Extension](#mysql-connectorpython-c-extension) section, doing so may cause the application to indefinitely hang if there is a network failure.
Unfortunately, due to conflicting limitations, you will need to decide if using the IAM plugin is worth this risk for your application.

## Getting Help and Opening Issues

If you encounter a bug with the AWS Advanced Python Driver, we would like to hear about it.
Please search the [existing issues](https://github.com/awslabs/aws-advanced-python-wrapper/issues) to see if others are also experiencing the issue before reporting the problem in a new issue. GitHub issues are intended for bug reports and feature requests. 

When opening a new issue, please fill in all required fields in the issue template to help expedite the investigation process.

For all other questions, please use [GitHub discussions](https://github.com/awslabs/aws-advanced-python-wrapper/discussions).

## How to Contribute

1. Set up your environment by following the directions in the [Development Guide](./docs/development-guide/DevelopmentGuide.md).
2. To contribute, first make a fork of this project. 
3. Make any changes on your fork. Make sure you are aware of the requirements for the project (e.g. do not require Python 3.7 if we are supporting Python 3.8 and higher).
4. Create a pull request from your fork. 
5. Pull requests need to be approved and merged by maintainers into the main branch. <br />
> [!NOTE]\
> Before making a pull request, [run all tests](./docs/development-guide/DevelopmentGuide.md#running-the-tests) and verify everything is passing.

### Code Style

The project source code is written using the [PEP 8 Style Guide](https://peps.python.org/pep-0008/), and the style is strictly enforced in our automation pipelines. Any contribution that does not respect/satisfy the style will automatically fail at build time.

## Releases

The `aws-advanced-python-wrapper` has a regular monthly release cadence. A new release will occur during the last week of each month. However, if there are no changes since the latest release, then a release will not occur.

## Aurora Engine Version Testing

This `aws-advanced-python-wrapper` is being tested against the following Community and Aurora database versions in our test suite:

| Database          | Versions                                                                                                                                                                                                  |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| MySQL             | 8.0.32                                                                                                                                                                                                    |
| PostgreSQL        | 15.2                                                                                                                                                                                                      |
| Aurora MySQL      | MySQL	8.0.mysql_aurora.3.02.2 (Wire-compatible with MySQL 8.0.23 onward. For more details see [here](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraMySQLReleaseNotes/AuroraMySQL.Updates.3022.html)) |
| Aurora PostgreSQL | 14.7 and 15.2 (Compatible with PostgreSQL 14.7 and 15.2, see release notes [here](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraPostgreSQLReleaseNotes/AuroraPostgreSQL.Updates.html))               |

The `aws-advanced-python-wrapper` is compatible with MySQL 5.7 and MySQL 8.0 as per MySQL Connector/Python.

## License

This software is released under the Apache 2.0 license.
