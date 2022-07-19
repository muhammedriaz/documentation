---
title: Live Debugging
kind: documentation
is_beta: true
private: true
further_reading:
- link: "/tracing/trace_collection/dd_libraries"
  tag: "Documentation"
  text: "Learn more about how to instrument your application"
- link: "/getting_started/tagging/unified_service_tagging/"
  tag: "Documentation"
  text: "Unified Service Tagging"
- link: "/tracing/services/services_list/"
  tag: "Documentation"
  text: "Discover the list of services reporting to Datadog"
- link: "/metrics"
  tag: "Documentation"
  text: "Learn more about Metrics"
---

{{< beta-callout url="http://d-sh.io/debugging" d-toggle="modal" d_target="#signupModal" custom_class="sign-up-trigger">}}
<!-- **!!!TODO: create beta signup form/link!!!** -->
  Live Debugging (LD) is in private beta. Let us know if you would like to
  access it.
{{< /beta-callout >}}

Live Debugging allows you to dynamically instrument your running application.
With Live Debugging you can set different kinds of probes, like snapshot or
metric probes, at specific code locations in your application. It relies on an
up-to-date APM tracing library, an up-to-date [Datadog Agent][1] and [Remote
Configuration Management](/remote_configuration) being enabled.

Create a probe:

{{< img src="tracing/live_debugging/snapshot_probe_creation.mp4" alt="Video demonstrating Live Debugging. A probe is configured on a service." video="true" >}}

Observe live data:

{{< img src="tracing/live_debugging/data_snapshot.mp4" alt="Video demonstrating Live Debugging. Data generated from a probe is explored." video="true" >}}

### Supported versions and compatibility

Required Agent version
: Live Debugging requires that the Datadog Agent installed alongside your
service be at least version 7.38.0.

Required Tracer version
: LD is currently supported for Java, .NET and Python. For details about the
required version refer to the respective [Getting Started Guide][2].

<div class="alert alert-info">
If you have feedback about what languages or runtimes you would like to see
supported, <a href="/help/">contact Support</a>.
</div>

## Getting Started

### Prerequisites

- Datadog Agent 7.38.0 or higher is installed alongside your service.
- Recommended: [Unified Service Tagging][3] tags `service`, `env` and `version`
  are applied to your deployment.
- Recommended: [Source Code Integration][4] has been set-up for your service.

### Configure Datadog Agent

**!!!TODO: create minimal Remote Configuration on boarding docs!!!**

### Create a Logs index

Live Debugging snapshots are ingested into the Datadog Logs product. They will
appear in context with your application logs. To avoid snapshots being sampled
out, it is required you create a Logs index with the name
`live-debugging-snapshots`.

[Configure the index][5] to the desired retention and make sure that no
sampling is configured. Use the filter `dd_source:debugger`. You should also
make sure that the new index is at a position in the list that is not
[superceeded][6] by any other filters. Logs enter the first index whose filter
they match on.

The amount of snapshots produced by a snapshot probe is limited by a sampling
mechanism inside the debugger library, so you don't have to worry about
excessive amounts of snapshots being produced.

### Enable Live Debugging

Adding the Live Debugger to your service is quite simple. Choose the language
of the service that you would like to debug below to learn how to enable it.

{{< partial name="live_debugging/livedebugging-languages.html" >}}

## Explore Live Debugging

The Live Debugging product can help you understand what your application is
doing at runtime.

### Create a snapshot probe

Probes can be thought of as non-breaking break point. Snapshot probes capture
the context at the code location. This includes members, arguments and locals.

Create the snapshot probe:

{{< img src="tracing/live_debugging/snapshot_probe_creation.mp4" alt="Video demonstrating Live Debugging. A snapshot probe is configured on a service." video="true" >}}

Observe the produced data:

{{< img src="tracing/live_debugging/data_snapshot.mp4" alt="Video demonstrating Live Debugging. Data generated from a snapshot probe is explored." video="true" >}}

### Create a metric probe

Metric probes are used to emit metrics at a code location. The Live Debugging
expression language can be used to extract values from the context, like from
a local variable.

Create the metric probe:

{{< img src="tracing/live_debugging/metric_probe_creation.mp4" alt="Video demonstrating Live Debugging. A metric probe is configured on a service." video="true" >}}

Observe the metric:

{{< img src="tracing/live_debugging/data_metric.mp4" alt="Video demonstrating Live Debugging. Data generated from a metric probe is explored." video="true" >}}

### Add a probe condition

Because we are sampling inside the debugger library it is sometimes hard to see
snapshots for edge cases which are most interesting most of the time. To be
able to see these edge cases you can configure a probe condition that will
govern when the debugger library captures data.

For example to capture snapshots only when the execution duration is more than
500ms, you can use the following conditional expression:

```
@duration > '500ms'
```

To only capture snapshots when a given argument `urlPath` matches a certain
string, use this expression:

```
^urlPath == "/foo/bar/baz"
```

To learn more about the Live Debugging expression language, refer to the
language reference.
**!!!TODO: Add expression language reference in different file and link here!!!**

### Select instrumented instances

The Live Debugging product allows you to specify which instances of your
service should be instrumented. Since the instrumentation applied by Live
Debugging is not without over-head we are making sure that the instrumentation
is not applied to your whole fleet.

{{< img src="tracing/live_debugging/client_selection.mp4" alt="Video demonstrating Live Debugging. Active clients are selected for a service." video="true" >}}

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /agent
[2]: /tracing/live_debugging/enabling
[3]: /getting_started/tagging/unified_service_tagging
[4]: https://docs.datadog.com/integrations/guide/source-code-integration
[5]: /logs/log_configuration/indexes/#add-indexes
[6]: /logs/log_configuration/indexes/#indexes-filters
