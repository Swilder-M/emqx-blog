## Background

As a MQTT broker admin, you may often want to know which clients are the slowest. We have a nice ‘top’ view for you. One option is perhaps to instrument the subscriber code with latency measurements. However given in MQTT world, the subscribers might be devices out in the wild which are hard to upgrade or collect metrics from.

EMQX v4.4 has a nice feature to overcome this challenge.

## Quick view

The per clientid-topic latency measurements are ranked in the table view as below.

![EMQX Slow subscribers Top-View](https://assets.emqx.com/images/5539aecaedd33fcfd342f12e4e81cc9f.png)

<center>Slow subscribers Top-View</center>

A new module named `emqx_mod_slow_subs`  was added in this release. With this module enabled, EMQX will start measuring message transmission latency. The measurement always starts at when a message is received by EMQX, the end of the measurement is configurable. Find more details in EMQX doc [Slow subscribers statistics](https://docs.emqx.com/en/emqx/v4.4/modules/slow_subscribers_statistics.html) 

# FAQ

### How to enable it

It can be enabled from the dashboard UI, in the “modules” tab.

### How to configure it

Find the configs in `emqx.conf` config namespace is `module.slow_subs.*`. 

For EMQX Enterprise edition, it can be dynamically configured from the dashboard UI.

![EMQX Enterprise dashboard](https://assets.emqx.com/images/c9d31f7050be11cb42832c336f0de144.png)

<center>EMQX Enterprise dashboard</center>

### Does it affect system performance

Very little with the default configuration.

- First of all, the module can be switched off, when switched off, it costs nothing.
- EMQX only collects latency for statistics when over certain limit (`500ms` by default). 
- EMQX does not rank ALL subscribers, but the slowest top-K during the latest sliding time window. 
  The `module.slow_subs.top_k_num` config value (default to `10`), as long as it is not tens of thousands, the resources spent on continuous ranking adjustment is insignificant.   



<section class="promotion">
    <div>
        Try EMQX Enterprise for Free
    </div>
    <a href="https://www.emqx.com/en/try?product=enterprise" class="button is-gradient px-5">Get Started →</a >
</section>
