# Hadoop Setup Scripts

## Preparation

* Build the Hadoop source tree

      mvn install -Pdist -Dtar -DskipTests -DskipShade -Dmaven.javadoc.skip

* Add `hadoop-sanity-tests/bin` directory to your PATH
* Replace `jzhuge` with your username in config files
* Replace `GCE.CLOUDERA.COM` with your Kerberos realm

## Run Hadoop sanity tests

Inside a Hadoop source or binary directory, run:

    hadoop-sanity-tests

The command will only run sanity tests in `insecure` and `ssl` mode by default.
Set TEST_SECURE_MODE to `yes` to test secure mode as well,
or set TEST_MODES to select which modes to test.

The command run sanity tests in all supported Hadoop services by default.
Set TEST_SERVICES to select which services to test.

## Run Hadoop in pseudo-distributed mode

### Start Hadoop in insecure mode

    pseudo_dist start config/insecure

### Start Hadoop in SSL mode

    pseudo_dist start config/ssl

### Start Hadoop in secure (SSL and Kerberos) mode

    create_keytabs <kdc_host> <fq_hostname> <kerberos_realm>

    kdestroy ; kinit -t ~/.config/kerberos/hdfs.keytab hdfs/localhost@<kerberos_realm>
    ./pseudo_dist start config/secure
    kdestroy ; kinit -t ~/.config/kerberos/$USER.keytab

### Stop Hadoop

    pseudo_dist kill|stop

## Environment variables

Name             | Default      | Description
-----------------|--------------|-------------
HADOOP_HOME      | (auto)       | Hadoop home directory
HADOOP_CONF_DIR  | (auto)       | Hadoop config directory
TEST_MODES       | insecure ssl | Space delimited list of test modes: insecure, ssl, and secure
TEST_SECURE_MODE | (none)       | Set it to `yes` to test secure mode as well
TEST_SERVICES    | hdfs yarn balancer httpfs kms | Space delimited list of Hadoop services: hdfs, yarn, balancer, httpfs, and kms
