---
layout: default
title: Timeout
---

# Timeout
{: .no_toc }

1. TOC
{:toc}

[Timeouts][Timeout] allow you to fail an execution with `TimeoutExceededException` if it takes too long to complete:

```java
Timeout<Object> timeout = Timeout.of(Duration.ofSeconds(10));
```

You can also [interrupt] an execution if it times out:

```java
timeout.withInterrupt(true);
```

If a cancellation is triggered by a `Timeout`, the execution is completed with `TimeoutExceededException`. See the [execution cancellation][execution-cancellation] section for more on cancellation.

## Timeouts with Retries

When a `Timeout` is [composed][policy-composition] _outside_ a `RetryPolicy`, a timeout occurrence will cancel any _inner_ retries:

```java
Failsafe.with(timeout, retryPolicy).run(this::connect);
```

When a `Timeout` is [composed][policy-composition] _inside_ a `RetryPolicy`, a timeout occurrence will not automically cancel any _outer_ retries:

```java
Failsafe.with(retryPolicy, timeout).run(this::connect);
```

## Event Listeners

[Timeouts] support the standard [policy listeners][PolicyListeners] which can notify you when a timeout is exceeded:

```java
timeout.onFailure(e -> log.error("Connection attempt timed out", e.getFailure()));
```

Or when an execution completes and the timeout is not exceeded:

```java
timeout.onSuccess(e -> log.info("Execution completed on time"));
```

{% include common-links.html %}