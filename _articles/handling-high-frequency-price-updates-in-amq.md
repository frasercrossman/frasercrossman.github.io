---
layout: article
title: "Handling High Frequency Price Updates in ActiveMQ"
description: "How to use UniquePropertyMessageEvictionStrategy to provide price updates to ActiveMQ"
date: "06-12-2020"
custom_css:
    - solarized-light
---

I was recently tasked with investigating how we can make better use of our messaging broker for sending high frequency
price updates to multiple Java and C# clients. In an ideal world we would like to consume all price updates that get
published to the topic but interrupted processes, network latency, and slow message deserialisation can cause consumers
to be slow.

[Slow consumers](https://activemq.apache.org/slow-consumers) are especially problematic when the topic they are
consuming from is non-durable as the amount of memory for buffered messages is typically very limited. ActiveMQ can be
configurated to cope with slow consumers in the following ways:
1. Halt the producer when any consumer message buffer is full
2. Drop slow consumers using one of two strategies[^1]
3. Spool messages to disk using temporary storage
4. Evict messages from the buffer

Halting the producer will ensure that all messages are consumed by all consumers but will cause price updates to be
lost while slow consumers catch up. Dropping a slow consumer is unacceptable as all clients may at some point become
slow but must continue to receive messages. Spooling messages to disk will allow consumers more time to catch up but
due to the frequency of price updates it is likely to simply delay halting the producer. Evicting messages will ensure
that the producer never halts and fast consumers are unaffected by slow consumers but it will mean that some messages
are missed.

In the context of price updates we can’t afford to miss messages that cause a given financial instrument to have no
known price if a price has been sent. We can however safely discard old price updates that we know have already become
obsolete by a more recent update. The unique property message eviction strategy provides exactly this functionality!

When sending a message set a named property in the message header:
```java
message.setStringProperty("TICKER", "AAPL");
```

To configure the ActiveMQ broker to use the eviction strategy add a destination policy:
```xml
<destinationPolicy>
  <policyMap>
    <policyEntries>
      <policyEntry topic="PRD.STOCK.PRICES">

        <pendingMessageLimitStrategy> 
          <constantPendingMessageLimitStrategy limit="5"/> 
        </pendingMessageLimitStrategy>

        <slowConsumerStrategy>
          <uniquePropertyMessageEvictionStrategy propertyName="TICKER"/>
        </slowConsumerStrategy>

      </policyEntry>
    </policyEntries>
  </policyMap>
</destinationPolicy>
```
This configuration will limit the number of pending messages to 5 and if exceeded will remove all except the most
recently published message for a given ticker. An example is shown below:

![](/images/articles/handling-high-frequency-price-updates-in-amq/unq-prop-eviction.svg){:width="100%"}

[^1]: You can either use `AbortSlowConsumerStrategy` or `AbortSlowAckConsumerStrategy`. The first periodically checks to see if the prefetch buffer is full and if so terminates the connection to a consumer. The second monitors the amount of time since the last acknowledged message. It was added to address the case where a low prefetch buffer limit is set and is therefore often full even when the consumer is fast.