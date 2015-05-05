---
layout: post101
title:  InfluxDB Reporter
categories: XAP101ADM
parent: metrics-overview.html
weight: 200
---

GigaSpaces XAP is shipped with built-in support for [InfluxDB](http://influxdb.com/). This page explains how to configure XAP to report metrics to InfluxDB. 

Configuration takes place in `metrics.xml`, which resides in `XAP_HOME/config/metrics`. For more information see [Configuration](./metrics-configuration.html).

{%info title=InfluxDB Status%}The GigaSpaces InfluxDB reporter implementation is based on [InfluxDB v0.8.x](http://influxdb.com/docs/v0.8/). InfluxDB v0.9 (which is expected to be production ready in April 2015) includes some powerful enhancements (e.g. tags support), which the XAP metric framework can leverage as well. We're already experimenting with InfluxDB 0.9 and plan to release a compatible reporter soon after InfluxDB 0.9 is GA.{%endinfo%}

# HTTP Reporter

InfluxDB provides an [HTTP API](http://influxdb.com/docs/v0.8/api/reading_and_writing_data.html#writing-data-through-http) for writing data, which is implemented by XAP. For example, if InfluxDB is installed on `myhost` and you want to report metrics to the `mydb` database, use the following configuration:

{% highlight xml %}
<metrics-configuration>
    <reporters>
        <reporter name="influxdb-http">
            <property name="host" value="http://myhost:8086"/>
            <property name="database" value="mydb"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
        </reporter>
    </reporters>
</metrics-configuration>
{% endhighlight %}

### Security

InfluxDB security is enabled by default, and requires a username and password to access the database. The default username and password are `root`, as in the example above. If your database has different username/password, change the configuration accordingly.

### Port

The InfluxDB HTTP API is bounded to port `8086` by default. If you've configured your InfluxDB instance to use a different port you should modify the reporter configuration accordingly and add a `<property name="port" value="nnnn"/>` with the new port.

{% comment %}

Retention policy is 0.9 specific.

### Retention Policy

An InfluxDB database has one or more [Retention Policies](http://influxdb.com/docs/v0.9/query_language/database_administration.html#retention-policy-management), which determine determine the *retention period* (the time after which data is automatically deleted from the InfluxDB system). By default, metrics are reported to the default retention policy of the database. To report to a different retention policy add `<property name="retention-policy" value="mypolicy"/>` to the configuration.

{% endcomment %}

### Report Length

The InfluxDB reporter batches multiple metrics within each report to maximize performance, up to a maximum value determined by the `max-report-length` property. The default value is `65507` (based on [UDP max length](http://en.wikipedia.org/wiki/User_Datagram_Protocol)), and usually should not be changed. 

# UDP Reporter

InfluxDB allows you to [write data through UDP](http://influxdb.com/docs/v0.8/api/reading_and_writing_data.html#writing-data-through-json-+-udp), assuming you will be writing data to a single database which is configured in InfluxDB's configuration file. The following snippet shows how to configure xap to use it:

{% highlight xml %}
<metrics-configuration>
    <reporters>
        <reporter name="influxdb-udp">
            <property name="host" value="myhost"/>
            <property name="port" value="4444"/>
        </reporter>
    </reporters>
</metrics-configuration>
{% endhighlight %}

Note we're setting the `host` to the location of the InfluxDB installation, and the database is not configured here at all. The default port is `4444`.

{% comment %}

Requires some cleanup
# Overriding metrics names

A user may wish to use a different naming scheme for the metrics. This can be accomplished by extending the relevant InfluxDB reporter and overriding the `getMetricNameForReport(String metricName)` method. 

For example, Processing unit metrics are prefixed by `pu_`. The following implementation replaces that prefix with `foo_`:

{% highlight java %}
public class MyInfluxDBHttpReporter extends InfluxDBHttpReporter {
    public MyInfluxDBHttpReporter(InfluxDBHttpReporterFactory factory) {
        super(factory);
    }

    @Override
    protected String getMetricNameForReport(String metricName) {
        final String prefix = "pu_";
        return metricName.startsWith(prefix) ? "foo_" + metricName.substring(prefix.length()) : metricName;
    }
}
{% endhighlight %}

To learn how to configure and install this custom reporter, refer to [Custom Reporter](./metrics-custom-reporter.html).
{% endcomment %}
