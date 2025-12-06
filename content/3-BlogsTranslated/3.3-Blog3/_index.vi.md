---
title: "Blog 3"
date: 2025-10-04
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Đơn giản hóa việc tích hợp AWS AppSync Events với Powertools for AWS Lambda

> bởi Ana Falcao và Leandro Cavalcante Damascena | ngày 14 tháng 5 năm 2025 | trong mục [AWS AppSync](https://aws.amazon.com/blogs/mobile/category/mobile-services/aws-appsync/), [AWS Lambda](https://aws.amazon.com/blogs/mobile/category/compute/aws-lambda/), [Developer Tools](https://aws.amazon.com/blogs/mobile/category/developer-tools/), [Technical How-to](https://aws.amazon.com/blogs/mobile/category/post-types/technical-how-to/) | [Permalink](https://aws.amazon.com/blogs/mobile/simplify-aws-appsync-events-integration-with-powertools-for-aws-lambda/) | [Share](https://aws.amazon.com/vi/blogs/mobile/simplify-aws-appsync-events-integration-with-powertools-for-aws-lambda/?fbclid=IwY2xjawNNLSdleHRuA2FlbQIxMABicmlkETFaSWRiTGFjUFVlemt0TURTAR5NZ_uttOfpoh-gyfNYi3YAyK8gskVcGVgwO1ZoanVHCVuQNrI9rrAHDenHrw_aem_NIqTzK504NOf04BvE3OUvA)

Các ứng dụng real-time đã trở thành yếu tố thiết yếu trong các ứng dụng hiện đại, nơi người dùng mong đợi được cập nhật ngay lập tức và có trải nghiệm tương tác. Dù bạn đang xây dựng ứng dụng chat, bảng điều khiển trực tiếp, bảng xếp hạng trò chơi, hay hệ thống IoT, [AWS AppSync Events](https://docs.aws.amazon.com/appsync/latest/eventapi/event-api-welcome.html) cho phép triển khai các tính năng real-time này thông qua WebSocket APIs, giúp bạn xây dựng các ứng dụng real-time có khả năng mở rộng và hiệu năng cao mà không phải lo lắng về việc mở rộng hay quản lý kết nối.

[Powertools for AWS Lambda](https://powertools.aws.dev/) là một bộ công cụ dành cho developer, bao gồm các tính năng như quan sát xử lý batch, tích hợp với thước đo hệ thống [AWS Systems Manager Store](https://aws.amazon.com/systems-manager/) mang tính bất biến, gắn cờ tính năng, [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) hỗ trợ ghi nhật ký có cấu trúc. Hơn thế nữa giờ đây Powertools for AWS đã hỗ trợ AppSync Events thông qua tính năng mới `AppSyncEventsResolver`, khả dụng cho Python, TypeScript, và .NET. Tính năng mới này nâng cao trải nghiệm phát triển với những khả năng được thiết kế để giúp bạn tập trung vào business logic. `AppSyncEventsResolver` cung cấp một giao diện đơn giản và nhất quán để xử lý sự kiện, với hỗ trợ tích hợp sẵn cho các pattern phổ biến như lọc, chuyển đổi và định tuyến sự kiện.

Trong bài viết này, bạn sẽ thấy các ví dụ bằng [TypeScript](https://docs.powertools.aws.dev/lambda/typescript/latest/), nhưng bạn cũng có thể sử dụng cùng chức năng trong các hàm Python và .NET với [Powertools for AWS (Python)](https://docs.powertools.aws.dev/lambda/python/latest/) và [Powertools for AWS (.NET)](https://docs.powertools.aws.dev/lambda/dotnet/).

![Real-time event handling architecture using AWS AppSync, Lambda, and Powertools](/images/3-Translated-Blogs/Blog3_1.png)

<center> <i>Hình 1 – Kiến trúc xử lý sự kiện real-time sử dụng AWS AppSync, Lambda, và Powertools.</i></center>

Trong bài viết này, bạn sẽ học cách:

- Thiết lập event handlers bằng `AppSyncEventsResolver`
- Triển khai các mẫu xử lý sự kiện khác nhau để đạt hiệu năng tối ưu
- Sử dụng pattern-based routing để tổ chức các event handlers
- Tận dụng các tính năng tích hợp sẵn cho những use case phổ biến

## Bắt đầu

`AppSyncEventsResolver` cung cấp một cách tiếp cận đơn giản và khai báo (declarative) để xử lý AppSync Events trong các hàm [AWS Lambda](https://aws.amazon.com/lambda/). Event resolver ho phép bạn lắng nghe hai loại sự kiện `PUBLISH` và `SUBSCRIBE`. `PUBLISH` events xảy ra khi các client gửi tin nhắn đến một channel, trong khi `SUBSCRIBE` xảy ra khi các client cố gắng đăng ký vào một channel. Bạn có thể đăng ký các handler cho những namespace và channel khác nhau để quản lý luồng giao tiếp theo hướng sự kiện.

Hãy cùng tìm hiểu cách bắt đầu và các tính năng cốt lõi sẽ nâng cao trải nghiệm phát triển của bạn. Đây là một ví dụ cơ bản về cách thiết lập `AppSyncEventsResolver`:

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

Lớp `AppSyncEventsResolver` đảm nhận việc phân tích dữ liệu sự kiện nhận vào và gọi đến phương thức handler phù hợp dựa trên loại sự kiện. Hãy cùng phân tích chi tiết:

### Pattern-based Routing

`AppSyncEventsResolver` sử dụng một hệ thống định tuyến dựa trên mẫu (pattern-based routing) trực quan, cho phép bạn tổ chức các event handler dựa trên đường dẫn channel. Bạn có thể:

- Xử lý các channel cụ thể (/chat/general)
- Sử dụng ký tự đại diện (wildcards) cho namespaces (/chat/\*)
- Tạo các handler tổng quát bắt tất cả (catch-all) (/\*)

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

Handler tổng quát nhất (catch-all) là `/*`,nó sẽ khớp với mọi namespace và channel, trong khi `/default/*` sẽ khớp với bất kỳ channel nào trong namespace default. Khi có nhiều handler cùng khớp với một sự kiện, thư viện sẽ gọi handler cụ thể nhất và bỏ qua các handler ít cụ thể hơn. Ví dụ: nếu một handler được đăng ký cho `/default/channel1` và một handler khác cho `/default/\*`, thì Powertools sẽ gọi handler đầu tiên cho các sự kiện khớp với `/default/channel1` và bỏ qua handler thứ hai. Điều này giúp bạn kiểm soát cách các sự kiện được xử lý và tránh việc xử lý thừa. Nếu có sự kiện không khớp với bất kỳ handler nào, theo mặc định Powertools sẽ trả về sự kiện đó như nguyên bản và ghi log một cảnh báo. Điều này có nghĩa là sự kiện sẽ được truyền qua mà không có bất kỳ chỉnh sửa nào. Cách tiếp cận này hữu ích khi bạn muốn áp dụng thư viện một cách dần dần: xử lý những sự kiện cụ thể bằng logic tùy chỉnh, trong khi các sự kiện khác được xử lý bằng hành vi mặc định.

### Xử lý đăng ký

Powertools cũng cung cấp một cách đơn giản để xử lý các events đăng ký. Thư viện sẽ tự động phân tích sự kiện nhận vào và gọi đến handler phù hợp dựa trên loại sự kiện. Theo mặc định, AppSync sẽ cho phép đăng ký, trừ khi Lambda handler của bạn ném ra lỗi hoặc từ chối yêu cầu một cách rõ ràng. Khi một đăng ký bị từ chối, AppSync sẽ trả về phản hồi 4xx cho client và ngăn không cho đăng ký được thiết lập.

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

Thư viện sẽ gọi đến handler phù hợp và truyền đối tượng sự kiện làm đối số đầu tiên khi một subscription event đến. Bạn có thể thực hiện bất kỳ hành động cần thiết nào dựa trên subscription event, chẳng hạn như chạy các kiểm tra quyền truy cập:

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

Các sự kiện đăng ký có cùng một quy tắc phù hợp và cung cấp quyền truy cập tương tự vào toàn bộ sự kiện và bối cảnh. Bạn có thể đăng ký các trình xử lý bắt tất cả cho bất kỳ không gian tên hoặc kênh nào bằng cách sử dụng ký tự đại diện `*`, đồng thời vẫn có thể truy cập đầy đủ các sự kiện và bối cảnh đối tượng trong trình xử lý của mình.

### Truy cập toàn bộ event và context

Mặc dù resolver đơn giản hóa việc xử lý sự kiện, bạn vẫn có toàn quyền truy cập vào các đối tượng event và context khi cần. Điều này rất hữu ích trong các tình huống bạn cần thêm thông tin, chẳng hạn như request headers hoặc thời gian thực thi còn lại từ Lambda context, để triển khai logic tùy chỉnh.

Resolver truyền toàn bộ event và context đến mỗi handler dưới dạng đối số thứ hai và thứ ba. Điều này cho phép bạn truy cập tất cả thông tin liên quan mà không cần thay đổi code hiện có.

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

### Xử lý lỗi

`AppSyncEventsResolver` cung cấp cơ chế xử lý lỗi tích hợp sẵn, giúp ngăn chặn việc hàm Lambda bị thất bại toàn bộ đồng thời vẫn đảm bảo lỗi được truyền đạt đúng cách đến AppSync, sau đó được chuyển tiếp đến client. Khi xảy ra lỗi trong handler, thay vì khiến toàn bộ Lambda invocation thất bại, resolver sẽ bắt lỗi và đưa nó vào response payload của sự kiện cụ thể bị lỗi.

Cách tiếp cận này đảm bảo hàm Lambda vẫn tiếp tục thực thi, trong khi vẫn gửi các thông báo lỗi được định dạng chuẩn đến AppSync. Khi xử lý nhiều sự kiện cùng lúc, nếu một sự kiện thất bại thì các sự kiện khác vẫn tiếp tục được xử lý bình thường. Điều này đặc biệt hữu ích trong các kịch bản xử lý song song, nơi bạn muốn đảm bảo rằng lỗi ở một sự kiện sẽ không ảnh hưởng đến việc xử lý các sự kiện khác.

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

## Các mẫu nâng cao và những ví dụ thực tiễn tốt nhất

`AppSyncEventsResolver` có thêm các tính năng nâng cao giúp bạn xây dựng các ứng dụng real-time mạnh mẽ và dễ bảo trì. Hãy cùng tìm hiểu những khả năng này và cách sử dụng chúng một cách hiệu quả.

### Xử lý khi publish

Theo mặc định, route handler của bạn sẽ được gọi một lần cho mỗi message. Điều này cho phép bạn tập trung vào business logic và tránh phải viết nhiều đoạn code lặp lại, trong khi Powertools sẽ xử lý việc lặp qua các message và chuyển đổi định dạng event cũng như response. Bạn chỉ cần trả về giá trị mà bạn muốn sử dụng làm payload, hoặc ném ra lỗi cho message đó. Thư viện sau đó sẽ ánh xạ payload với đúng event id tương ứng.

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

Mẫu này giúp đơn giản hóa việc phát triển bằng cách cho phép bạn chỉ cần viết logic cho một sự kiện duy nhất, còn Powertools sẽ tự động xử lý mọi phần còn lại.

### Xử lý gộp

Chế độ aggregate cho phép bạn xử lý nhiều sự kiện như một batch duy nhất, thay vì xử lý chúng riêng lẻ. Điều này đặc biệt hữu ích khi bạn muốn tối ưu việc sử dụng tài nguyên, chẳng hạn như gửi nhiều truy vấn đến cơ sở dữ liệu trong một lần thao tác, hoặc phân tích nhiều sự kiện cùng nhau trước khi xử lý. Mặc dù cả hai chế độ đều cho bạn toàn quyền kiểm soát quá trình xử lý sự kiện, nhưng chế độ aggregate cung cấp khả năng truy cập toàn bộ danh sách sự kiện cùng một lúc.

Để thực hiện điều này, bạn có thể đặt tùy chọn `aggregate` đến `true`. Khi sử dụng chế độ này, resolver sẽ gửi toàn bộ danh sách sự kiện đến handler của bạn chỉ trong một lần gọi, cho phép bạn xử lý chúng như một batch.

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

Lưu ý rằng tùy chọn `aggregate` chỉ khả dụng cho publish events, và khi sử dụng tùy chọn này, bạn sẽ chịu trách nhiệm xử lý các sự kiện cũng như trả về phản hồi phù hợp. Powertools vẫn sẽ đảm nhận việc định tuyến các sự kiện đến đúng handler, nhưng bạn có toàn quyền kiểm soát cách các sự kiện được xử lý.

### Bộ lọc sự kiện

Để lọc bỏ một sự kiện, bạn có thể ném ra một lỗi trong channel handler. Nếu một handler ném lỗi cho một sự kiện cụ thể, thư viện sẽ bắt lỗi đó và thêm một đối tượng lỗi vào danh sách phản hồi tại cùng chỉ số. Điều này báo hiệu rằng tin nhắn tương ứng sẽ bị loại bỏ. Cách tiếp cận này cho phép bạn hoặc là lặng lẽ lọc sự kiện, hoặc cung cấp phản hồi lỗi có ý nghĩa cho các người đăng ký của bạn.

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

## Kết luận

`AppSyncEventsResolver` trong Powertools for AWS nâng cao trải nghiệm phát triển với AppSync Events bằng cách cung cấp một giao diện đơn giản và nhất quán để xử lý các sự kiện real-time. Bằng cách giảm thiểu bản soạn sẵn code và cung cấp sẵn hỗ trợ cho các mẫu phổ biến, bạn có thể tập trung vào business logic thay vì phải lo về code hạ tầng.

Đọc thêm:

- Khám phá [Powertools for AWS documentation](https://docs.powertools.aws.dev/lambda/typescript/latest/) để biết thêm thông tin.
- Ghé qua [AppSync Events documentation](https://docs.aws.amazon.com/appsync/latest/eventapi/event-api-welcome.html) để biết thêm về dịch vụ.
- Ghé thăm [GitHub repository](https://github.com/aws-powertools) để tiếp tục thảo luận vấn đề.

Chúng tôi rất mong chờ được thấy những gì bạn sẽ xây dựng với các khả năng mới này. Hãy chia sẻ phản hồi của bạn và cho chúng tôi biết cách bạn đang sử dụng `AppSyncEventsResolver` trong các ứng dụng của mình!

TAGS: [AppSync](https://aws.amazon.com/blogs/mobile/tag/appsync/), [AWS Lambda](https://aws.amazon.com/blogs/mobile/tag/aws-lambda/)
