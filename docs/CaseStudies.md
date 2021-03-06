## Case studies

Below are approved public case studies from VictoriaMetrics users. Join our [community Slack channel](http://slack.victoriametrics.com/)
and feel free asking for references, reviews and additional case studies from real VictoriaMetrics users there.


### Wix.com

[Wix.com](https://en.wikipedia.org/wiki/Wix.com) is the leading web development platform.

> We needed to redesign metric infrastructure from the ground up after the move to Kubernethes. A few approaches/designs have been tried before the one that works great has been chosen: Prometheus instance in every datacenter with 2 hours retention for local storage and remote write into [HA pair of single-node VictoriaMetrics instances](https://github.com/VictoriaMetrics/VictoriaMetrics/blob/master/README.md#high-availability).

Numbers:

* The number of active time series per VictoriaMetrics instance is 20M.
* The total number of time series per VictoriaMetrics instance is 400M+.
* Ingestion rate per VictoriaMetrics instance is 800K data points per second.
* The average time series churn rate is ~3M per day.
* The average query rate is ~1K per minute (mostly alert queries).
* Query duration: median is ~70ms, 99th percentile is ~2sec.
* Retention: 6 months.

> Alternatives that we’ve played with before choosing VictoriaMetrics are: federated Prometheus, Cortex, IronDB and Thanos.
> Points that were critical to us when we were choosing a central tsdb, in order of importance:

* At least 3 month worth of history.
* Raw data, no aggregation, no sampling.
* High query speed.
* Clean fail state for HA (multi-node clusters may return partial data resulting in false alerts).
* Enough head room/scaling capacity for future growth, up to 100M active time series.
* Ability to split DB replicas per workload. Alert queries go to one replica, user queries go to another (speed for users, effective cache).

> Optimizing for those points and our specific workload VictoriaMetrics proved to be the best option. As an icing on a cake we’ve got [PromQL extensions](https://github.com/VictoriaMetrics/VictoriaMetrics/wiki/ExtendedPromQL) - `default 0` and `histogram` are my favorite ones, for example. What we specially like is having a lot of tsdb params easily available via config options, that makes tsdb easy to tune for specific use case. Also worth noting is a great community in [Slack channel](http://slack.victoriametrics.com/) and of course maintainer support.

Alex Ulstein, Head of Monitoring, Wix.com


### Wedos.com

> [Wedos](https://www.wedos.com/) is the Biggest Czech Hosting. We have our own private data center, that holds only our servers and technologies. The second data center, where the servers will be cooled in an oil bath, is being built. We started using [cluster VictoriaMetrics](https://github.com/VictoriaMetrics/VictoriaMetrics/blob/cluster/README.md) to store Prometheus metrics from all our infrastructure after receiving positive references from our friends who successfully use VictoriaMetrics.

Numbers:

* The number of acitve time series: 5M.
* Ingestion rate: 170K data points per second.
* Query duration: median is ~2ms, 99th percentile is ~50ms.

> We like configuration simplicity and zero maintenance for VictoriaMetrics - once installed and forgot about it. It works out of the box without any issues.


### Dreamteam

[Dreamteam](https://dreamteam.gg/) successfully uses single-node VictoriaMetrics in multiple environments.

Numbers:

* Active time series: from 350K to 725K.
* Total number of time series: from 100M to 320M.
* Total number of datapoints: from 120 billions to 155 billions.
* Retention: 3 months.

VictoriaMetrics in production environment runs on 2 M5 EC2 instances in "HA" mode, managed by Terraform and Ansible TF module.
2 Prometheus instances are writing to both VMs, with 2 [Promxy](https://github.com/jacksontj/promxy) replicas
as load balancer for reads.
