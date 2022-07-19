---
title: Enable Live Debugging for Java
kind: Documentation
is_beta: true
private: true
code_lang: java
type: multi-code-lang
code_lang_weight: 10
further_reading:
    - link: 'agent'
      tag: 'Documentation'
      text: 'Getting Started with Datadog Agent'
---

The Live Debugger is shipped within Datadog tracing libraries. If you are
already using [APM to collect traces][1] for your application, you can skip
installing the library and go directly to enabling the debugger in step 3.

## Requirements

Datadog Agent has to be running in your infrastructure and must be at least
version [7.38.0][2]+ and must have Remote Configuration Management enabled.

The Datadog Live Debugging library is supported in OpenJDK 11+, Oracle JDK 11+,
[OpenJDK 8 (version 8u262+)][3] and Azul Zulu 8+ (version 8u212+).

All JVM-based languages, such as Java, Scala, Groovy, Kotlin, and Clojure are
partly supported.

## Installation

To begin debugging applications:

1. If you are already using Datadog, upgrade your Agent to version
   [7.38.0][2]+. If you don't have APM enabled to set up your application to
   send data to Datadog, in your Agent, set the `DD_APM_ENABLED` environment
   variable to `true` and listening to the port `8126/TCP`.

2. Download `dd-java-agent.jar`, which contains the Java Agent class files:

    ```shell
    wget -O dd-java-agent.jar 'https://dtdg.co/latest-java-tracer'
    ```

     **Note**: Live Debugging is available in the `dd-java-agent.jar` library
     in versions **!!!TODO!!!**.

3. Enable the debugger by setting `-Ddd.debugging.enabled` flag or
   `DD_DEBUGGER_ENABLED` environment variable to `true`. Specify `dd.service`,
   `dd.env`, and `dd.version` so you can filter and group your probes and
   target active clients across these dimensions:
   {{< tabs >}}
{{% tab "Command arguments" %}}

Invoke your service:
```shell
java \
    -javaagent:dd-java-agent.jar \
    -Ddd.service=<YOUR_SERVICE> \
    -Ddd.env=<YOUR_ENVIRONMENT> \
    -Ddd.version=<YOUR_VERSION> \
    -Ddd.debugging.enabled=true \
    -jar <YOUR_SERVICE>.jar <YOUR_SERVICE_FLAGS>
```
{{% /tab %}}
{{% tab "Environment variables" %}}

```shell
export DD_SERVICE=<YOUR_SERVICE>
export DD_ENV=<YOUR_ENV>
export DD_VERSION=<YOUR_VERSION>
export DD_DEBUGGING_ENABLED=true
java \
    -javaagent:dd-java-agent.jar \
    -jar <YOUR_SERVICE>.jar <YOUR_SERVICE_FLAGS>
```
{{% /tab %}}
{{< /tabs >}}

    **Note**: The `-javaagent` argument needs to be before `-jar`, adding it as
    a JVM option rather than an application argument. For more information, see
    the [Oracle documentation][4]:

    ```shell
    # Good:
    java -javaagent:dd-java-agent.jar ... -jar my-service.jar -more-flags
    # Bad:
    java -jar my-service.jar -javaagent:dd-java-agent.jar ...
    ```

4. After starting your service with Live Debugging enabled, you can start
   debugging on the [Datadog APM > Live Debugging pservice
   age](https://app.datadoghq.com/debugging).

## Configuration

You can configure the debugger using the following environment variables:

| Environment variable                             | Type          | Description                                                                                                               |
| ------------------------------------------------ | ------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `DD_DEBUGGING_ENABLED`                           | Boolean       | Alternate for `-Ddd.debugging.enabled` argument. Set to `true` to enable live debugging.                                  |
| `DD_SERVICE`                                     | String        | The [service][5] name, for example, `web-backend`.                                                                        |
| `DD_ENV`                                         | String        | The [environment][5] name, for example: `production`.                                                                     |
| `DD_VERSION`                                     | String        | The [version][5] of your service.                                                                                         |
| `DD_TAGS`                                        | String        | Tags to apply to produced data. Must be a list of `<key>:<value>` separated by commas such as: `layer:api, team:intake`.  |

## Not sure what to do next?

Return to the [overview page][6] to see what you can do with the Live Debugging
product.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /tracing/trace_collection/
[2]: https://app.datadoghq.com/account/settings#agent/overview
[3]: /tracing/profiler/profiler_troubleshooting/#java-8-support
[4]: https://docs.oracle.com/javase/7/docs/technotes/tools/solaris/java.html
[5]: /getting_started/tagging/unified_service_tagging
[6]: /tracing/live_debugging/
