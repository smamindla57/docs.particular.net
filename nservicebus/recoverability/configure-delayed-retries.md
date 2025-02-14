---
title: Configure delayed retries
summary: How to configure delayed retries as a second stage of recoverability.
component: Core
reviewed: 2020-04-26
redirects:
 - nservicebus/second-level-retries
related:
 - samples/faulttolerance
---

NOTE: Starting from NServiceBus version 6, the delayed retries policy (formerly known as second level retries policy) has been deprecated in favor of the new custom recoverability policy which allows more control over the recoverability behavior. This documentation shows how the previous delayed retries policies can be implemented with the new [recoverability policy](/nservicebus/recoverability/custom-recoverability-policy.md).

WARNING: Delayed retries cannot be used when transport transactions are disabled or delayed delivery is not available. For more information about transport transactions, refer to [transport transactions](/transports/transactions.md). For more details on the caveats, see the [delayed delivery](/nservicebus/messaging/delayed-delivery.md#caveats) article.

## Configuring delayed retries

 * `TimeIncrease`: Specified as a TimeSpan, defaults to 10 seconds. Specifies the delay interval for each retry attempt. This delay increases by the same timespan with each delayed delivery. For example, if the specified value is the default 10 seconds, i.e. 00:00:10, then the first delayed retry will be at ten seconds, the subsequent delayed retry will be 20 seconds, and so on.
 * `NumberOfRetries`: Number of times delayed retries are performed. Default is 3.

snippet: DelayedRetriesConfiguration


## Disabling through code

snippet: DisableDelayedRetries


## Custom retry policy

Custom retry logic can be configured via code.

snippet: DelayedRetriesCustomPolicy


### Simple policy

The following retry policy overrides the default ten second delay interval and sets it to five seconds.

snippet: DelayedRetriesCustomPolicyHandler


### Exception-based policy

Sometimes the number of retries or the delay interval might depend on the error exception thrown. The following retry policy extends the previous one by skipping delayed retries whenever `MyBusinessException` has been thrown during incoming message processing.

snippet: DelayedRetriesCustomExceptionPolicyHandler


partial: legacy
