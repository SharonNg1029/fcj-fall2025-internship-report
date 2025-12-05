---
title: "Blog 3"
date: 2025-10-04
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Simplify AWS AppSync Events integration with Powertools for AWS Lambda

> by Ana Falcao and Leandro Cavalcante Damascena | on 14 MAY 2025 | [AWS AppSync](https://aws.amazon.com/blogs/mobile/category/mobile-services/aws-appsync/), [AWS Lambda](https://aws.amazon.com/blogs/mobile/category/compute/aws-lambda/), [Developer Tools](https://aws.amazon.com/blogs/mobile/category/developer-tools/), [Technical How-to](https://aws.amazon.com/blogs/mobile/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/mobile/simplify-aws-appsync-events-integration-with-powertools-for-aws-lambda/) | [Share](https://aws.amazon.com/vi/blogs/mobile/simplify-aws-appsync-events-integration-with-powertools-for-aws-lambda/?fbclid=IwY2xjawNNLSdleHRuA2FlbQIxMABicmlkETFaSWRiTGFjUFVlemt0TURTAR5NZ_uttOfpoh-gyfNYi3YAyK8gskVcGVgwO1ZoanVHCVuQNrI9rrAHDenHrw_aem_NIqTzK504NOf04BvE3OUvA)

Real-time capabilities have become essential in modern applications, where users expect immediate updates and interactive experiences. Whether you’re building chat applications, live dashboards, gaming leaderboards, or IoT systems, [AWS AppSync Events](https://docs.aws.amazon.com/appsync/latest/eventapi/event-api-welcome.html) enables these real-time features through WebSocket APIs, allowing you to build scalable and performant real-time applications, without worrying about scale or connection management.

[Powertools for AWS Lambda](https://powertools.aws.dev/) is a developer toolkit that includes observability, batch processing, [AWS Systems Manager Store](https://aws.amazon.com/systems-manager/) Parameter Store integration, idempotency, feature flags, [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) Metrics, structured logging, and more. Powertools for AWS now supports AppSync Events through the new `AppSyncEventsResolver`, available in Python, TypeScript, and .NET. This new feature enhances the development experience with capabilities designed to help you focus on your business logic. The `AppSyncEventsResolver` provides a simple and consistent interface for processing events, with built-in support for common patterns such as filtering, transforming, and routing events.

In this post, you will see examples in [TypeScript](https://docs.powertools.aws.dev/lambda/typescript/latest/), but you can use the same functionality in Python and .NET functions using [Powertools for AWS (Python)](https://docs.powertools.aws.dev/lambda/python/latest/) and [Powertools for AWS (.NET)](https://docs.powertools.aws.dev/lambda/dotnet/).

![Real-time event handling architecture using AWS AppSync, Lambda, and Powertools](/images/3-Translated-Blogs/Blog3_1.png)

<center> <i>Figure 1 Real-time event handling architecture using AWS AppSync, Lambda, and Powertools.</i></center>

In this post, you’ll learn how to:

- Set up event handlers using the `AppSyncEventsResolver`
- Implement different event processing patterns for optimal performance
- Use pattern-based routing to organize your event handlers
- Leverage built-in features for common use cases

## Getting Started

The `AppSyncEventsResolver` provides a simple, declarative way to handle AppSync Events in your [AWS Lambda](https://aws.amazon.com/lambda/) function. The event resolver allows you to listen for `PUBLISH` and `SUBSCRIBE` events. `PUBLISH` events occur when clients send messages to a channel, while `SUBSCRIBE` events happen when clients attempt to subscribe to a channel. You can register handlers for different namespaces and channels to manage your event-driven communications.

Let’s explore how to get started and the core features that will enhance your development experience. Here’s a basic example of how to set up the `AppSyncEventsResolver`:

```TypeScript

import {

AppSyncEventsResolver,

UnauthorizedException,

} from '@aws-lambda-powertools/event-handler/appsync-events';

// Types for our message handling

type ChatMessage = {

userId: string;

content: string;

}

// Simple authorization check

const isAuthorized = (path: string, userId?: string): boolean => {

// check against your authorization system

if (path.startsWith('/chat/private') && !userId) {

return false;

}

return true;

};

// Message processing logic

const processMessage = async (payload: ChatMessage) => {

// - Validate message content

// - Store in database

// - Enrich with additional data

return {

...payload,

timestamp: new Date().toISOString()

};

};

const app = new AppSyncEventsResolver();

// Handle publish events for a specific channel

app.onPublish('/chat/general', async (payload: ChatMessage) => {

// Process and return the message

return processMessage(payload);

});

// Handle subscription events for all channels

app.onSubscribe('/\*', async (info) => {

const {

channel: { path },

request,

} = info;

// Perform access control checks

if (!isAuthorized(path, userId)) {

throw new UnauthorizedException(`not allowed to subscribe to ${path}`);

}

return true;

});

export const handler = async (event, context) =>

app.resolve(event, context);
```

The `AppSyncEventsResolver` class takes care of parsing the incoming event data and invoking the appropriate handler method based on the event type. Let’s break down what’s happening:

### Pattern-based Routing

The `AppSyncEventsResolver` uses an intuitive pattern-based routing system that allows you to organize your event handlers based on channel paths. You can:

- Handle specific channels (/chat/general)
- Use wildcards for namespaces (/chat/\*)
- Create global catch-all handlers (/\*)

```TypeScript

import { AppSyncEventsResolver } from '@aws-lambda-powertools/event-handler/appsync-events';

const app = new AppSyncEventsResolver();

// Specific channel handler

app.onPublish('/notifications/alerts', async (payload) => {

// your logic here

});

// Handle all channels in the notifications namespace

app.onPublish('/notifications/\*', async (payload) => {

// your logic here

});

// Global catch-all for unhandled channels

app.onPublish('/\*', async (payload) => {

// your logic here

});

export const handler = async (event, context) =>

app.resolve(event, context);
```

The most general catch-all handler is `/*`, which will match any namespace and channel, while `/default/*` will match any channel in the `default` namespace. When multiple handlers match the same event, the library will call the most specific handler and ignore the less specific ones. For example, if a handler is registered for `/default/channel1` and another one for `/default/\*`, Powertools will call the first handler for events that match `/default/channel1` and ignore the second one. This provides you control over how events are handled and to avoid unnecessary processing. If an event that does not match any handler, by default Powertools will return the events as is and log a warning. This means that the events will be passed through without any modifications. This approach is helpful for gradually adopting the library, allowing you to handle specific events with custom logic while others are processed by the default behavior.

### Subscription Handling

Powertools also provides a simple way to handle subscription events. It will automatically parse the incoming event and call the appropriate handler based on the event type. By default, AppSync allows the subscription unless your Lambda handler either throws an error or explicitly rejects the request. When a subscription is rejected, AppSync will return a 4xx response to the client and prevent the subscription from being established.

```TypeScript

import { AppSyncEventsResolver } from '@aws-lambda-powertools/event-handler/appsync-events';

import { Metrics, MetricUnit } from '@aws-lambda-powertools/metrics';

import type { Context } from 'aws-lambda';

const metrics = new Metrics({

namespace: 'serverlessAirline',

serviceName: 'chat',

singleMetric: true,

});

const app = new AppSyncEventsResolver();

app.onSubscribe('/default/foo', (event) => {

metrics.addDimension('channel', event.info.channel.path);

metrics.addMetric('connections', MetricUnit.Count, 1);

});

export const handler = async (event: unknown, context: Context) =>

app.resolve(event, context);
```

The library calls the appropriate handler and passes the event object as the first argument when a subscription event arrives. You can take any necessary actions based on the subscription event, such as running access control checks:

```TypeScript

app.onSubscribe('/private/\*', async (info) => {

const userGroups =

info.identity?.groups && Array.isArray(info.identity?.groups)

? info.identity?.groups

: [];

const channelGroup = 'premium-users';

if (!userGroups.includes(channelGroup)) {

throw new UnauthorizedException(

`Subscription requires ${channelGroup} group membership`

);

}

})
```

Subscription events follow the same matching rules and provide the same access to the full event and context. You can register catch-all handlers for any namespace or channel by using the wildcard `*` character, and also access the full event and context objects in your handlers.

### Access full event and context

While the resolver simplifies event handling, you still have full access to the event and context objects when needed. This is helpful in scenarios where you need additional information, such as request headers or the remaining execution time from the Lambda context, to implement custom logic.

The resolver passes the full event and context to each handler as the second and third arguments. This lets you access all relevant information without changing your existing code

```TypeScript

import { AppSyncEventsResolver } from '@aws-lambda-powertools/event-handler/appsync-events';

import { Logger } from '@aws-lambda-powertools/logger';

const logger = new Logger({

logLeveL: 'INFO',

serviceName: 'serverlessAirline'

});

const app = new AppSyncEventsResolver({ logger});

app.onPublish('/orders/process', async (payload, event, context) => {

// Access request headers

const { headers } = event.request;

// Access Lambda context

const { getRemainingTimeInMillis } = context;

logger.info('Processing event details', {

headers,

remainingTime: getRemainingTimeInMillis()

});

return payload;

});

export const handler = async (event, context) =>

app.resolve(event, context);:
```

### Error handling

The `AppSyncEventsResolver` offers built-in error handling that prevents Lambda function failures while ensuring errors are properly communicated to AppSync, which then propagates them to the clients. When an error occurs in your handler, instead of failing the entire Lambda invocation, the resolver captures the error and includes it in the response payload for the specific event that failed.

This approach ensures the Lambda function continues execution while providing properly formatted error messages to AppSync. When processing multiple events, if one event fails, the others continue processing normally. This is particularly useful in parallel processing scenarios where you want to ensure that an error in one event doesn’t affect the processing of other events.

```TypeScript

import { AppSyncEventsResolver } from '@aws-lambda-powertools/event-handler/appsync-events';

const app = new AppSyncEventsResolver();

app.onPublish('/messages', async (payload) => {

// If message contains "error", throw an exception

if (payload.message === "error") {

throw new Error("Invalid message");

}

return payload;

});

export const handler = async (event, context) =>

app.resolve(event, context);

// When processing this event:

// {

// "id": "123",

// "payload": {

// "message": "error"

// }

// }

// The resolver will return:

// {

// "id": "123",

// "error": "Error - Invalid message"

// }
```

## Advanced Patterns and Best Practices

The `AppSyncEventsResolver` has additional advanced features that help you build robust and maintainable real-time applications. Let’s explore these capabilities and how to use them effectively.

### On publish processing

By default, we call your route handler once per message. This allows you to focus on your business logic and avoid writing boilerplate code while Powertools handles message iteration and converts the event and response format. You only need to return the value you want to use as payload, or throw an error for that message. The library will then correlate the payload with the correct event id.

```TypeScript

import { AppSyncEventsResolver } from '@aws-lambda-powertools/event-handler/appsync-events';

import { Metrics, MetricUnit } from '@aws-lambda-powertools/metrics';

type SensorReading = {

deviceId: string;

temperature: number;

humidity: number;

timestamp: string;

}

const app = new AppSyncEventsResolver();

const metrics = new Metrics({ namespace: 'SensorReadings' });

app.onPublish('/sensors/readings', async (payload: SensorReading) => {

// Process each sensor reading independently

if (payload.temperature > 100) {

metrics.addDimension('alertType', 'highTemperature');

metrics.addMetric('HighTemperature', MetricUnit.Count, 1);

throw new Error('Temperature reading too high');

}

// Enrich the payload with processing timestamp

return {

...payload,

processed: true,

processedAt: new Date().toISOString()

};

});

export const handler = async (event, context) =>

app.resolve(event, context);
```

This pattern simplifies development by letting you write only the logic for a single event, Powertools handles the rest automatically.

### Aggregate Processing

The aggregate mode lets you to process multiple events as a single batch, rather than handling them individually. This is particularly useful when you want to optimize resource usage, such as sending multiple queries to a database in a single operation, or to analyze multiple events together before processing them. While both modes give you full control over event processing, aggregate mode provides access to the entire list of events at once.

To achieve this, you can set the `aggregate` option to `true`. When using this mode, the resolver sends the entire list of events to your handler in a single call, letting you process them as a batch.

```TypeScript
import { AppSyncEventsResolver } from '@aws-lambda-powertools/event-handler/appsync-events';

const app = new AppSyncEventsResolver();

app.onPublish('/default/*', async (events) => {
  const results = [];
  for (const event of events) {
    try {
      results.push(await handleDefaultNamespaceCatchAll(event));
    } catch (error) {
      results.push({
        error: {
          errorType: 'Error',
          message: error.message,
        },
        id: event.id,
      });
    }
  }

  return results;
}, {
  aggregate: true,
});

export const handler = async (event, context) =>
  app.resolve(event, context);
```

Note that the `aggregate` option is only available for publish events, and that when using this option, you are responsible for handling the events and returning the appropriate response. Powertools will still take care of routing the events to the correct handler, but you have full control over how the events are processed.

### Event Filtering

To filter out an event, throw an error in the channel handler. If a handler throws an error for a specific event, the library catches it and adds an error object to the response list at the same index. This signals that the corresponding message should be dropped. This allows you to either silently filter events or provide meaningful error feedback to your subscribers.

```TypeScript

import { AppSyncEventsResolver } from '@aws-lambda-powertools/event-handler/appsync-events';

app.onPublish('/moderation/\*', async (payload) => {

// Filter out inappropriate content

if (await containsInappropriateContent(payload)) {

throw new CustomError('Content violates guidelines');

}

// Process valid content

return await processContent(payload);

});

export const handler = async (event, context) =>

app.resolve(event, context);
```

## Conclusion

The `AppSyncEventsResolver` in Powertools for AWS enhances your development experience with AppSync Events by providing a simple and consistent interface for processing real-time events. By reducing boilerplate code and offering built-in support for common patterns, you can focus on your business logic rather than infrastructure code.

To learn more:

- Explore the [Powertools for AWS documentation](https://docs.powertools.aws.dev/lambda/typescript/latest/) for detailed information.
- Check out the [AppSync Events documentation](https://docs.aws.amazon.com/appsync/latest/eventapi/event-api-welcome.html) to learn more about the service.
- Visit our [GitHub repository](https://github.com/aws-powertools) to contribute or report issues.

We’re excited to see what you build with these new capabilities. Share your feedback and let us know how you’re using `AppSyncEventsResolver` in your applications!

TAGS: [AppSync](https://aws.amazon.com/blogs/mobile/tag/appsync/), [AWS Lambda](https://aws.amazon.com/blogs/mobile/tag/aws-lambda/)
