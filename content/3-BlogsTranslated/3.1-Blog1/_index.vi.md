---
title: "Blog 1"
date: 2025-10-04
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Giới thiệu Strands Agents, mã nguồn mở của AI Agents SDK

> bởi Clare Liguori | ngày 16 tháng 5 năm 2025 | trong mục [Announcements](https://aws.amazon.com/blogs/opensource/category/post-types/announcements/), [Artificial Intelligence](https://aws.amazon.com/blogs/opensource/category/artificial-intelligence/), [Geneative AI](https://aws.amazon.com/blogs/opensource/category/artificial-intelligence/generative-ai/), [Open Source](https://aws.amazon.com/blogs/opensource/category/open-source/) | [Permalink](https://aws.amazon.com/blogs/opensource/introducing-strands-agents-an-open-source-ai-agents-sdk/) | [Comments](https://aws.amazon.com/blogs/opensource/introducing-strands-agents-an-open-source-ai-agents-sdk/#Comments) | [Shared](https://aws.amazon.com/vi/blogs/opensource/introducing-strands-agents-an-open-source-ai-agents-sdk/)

Hôm nay, tôi rất vui mừng thông báo rằng chúng tôi cho ra mắt [Strands Agents](https://strandsagents.com/). Strands Agents là một mã nguồn mở SDK áp dụng cách tiếp cận model-driven để xây dựng và vận hành AI agents chỉ với vài dòng code. Strands có khả năng mở rộng từ những use case đơn giản đến phức tạp, từ phát triển cục bộ đến triển khai trong môi trường sản phẩm. Nhiều team tại AWS đã sử dụng Strands cho AI agents trong sản phẩm của họ, bao gồm Amazon Q Developer, AWS Glue, và VPC Reachability Analyzer. Giờ đây, tôi cực kì hồi hộp khi chia sẻ Strands với bạn để tạo nên AI agents của riêng mình.

So với các frameworks yêu cầu lập trình viên phải định nghĩa lại quy trình làm việc phức tạp cho AI của họ, Strands đơn giản hóa việc phát triển AI bằng cách tận dụng khả năng của các mô hình hiện đại để lên kế hoạch, định hình suy nghĩ, sử dụng công cụ và lời phản ánh. Với Strands, lập trình viên chỉ cần định nghĩa một câu lệnh và một danh sách công cụ trong code để xây dựng agent, sau đó có thể test cục bộ và triển khai lên cloud. Giống như hai sợi DNA, Strands kết nối hai thành phần cốt lõi của AIt: mô hình và công cụ. Strands lên kế hoạch cho các bước tiếp theo của AI agents và thực thi chức năng công cụ nhờ vào khả năng lý luận nâng cao của mô hình ngôn ngữ. Đối với các trường hợp AI agent phức tạp hơn, lập trình viên có thể tùy chỉnh hành vi của AI agent trong Strands. Ví dụ, bạn có thể chỉ định cách công cụ được chọn, tùy chỉnh cách quản lý bối cảnh, chọn nơi lưu trữ trạng thái và bộ nhớ, cũng như xây dựng các ứng dụng đa tác vụ. Strands có thể chạy ở bất kỳ đâu và hỗ trợ mọi mô hình có khả năng lý luận và sử dụng công cụ, bao gồm các mô hình trong Amazon Bedrock, Anthropic, Ollama, Meta, và các nhà cung cấp khác thông qua LiteLLM.

Strands Agents là một cộng đồng mở, và chúng tôi rất háo hức khi có nhiều công ty tham gia cùng với sự hỗ trợ và đóng góp, bao gồm Accenture, Anthropic, Langfuse, mem0.ai, Meta, PwC, Ragas.io và Tavily. Ví dụ, Anthropic đã đóng góp vào Strands bằng cách bổ sung hỗ trợ sử dụng models thông qua Anthropic API, và Meta đã đóng góp hỗ trợ cho các Llama models thông qua Llama API. Hãy tham gia cùng chúng tôi trên [GitHub](https://github.com/strands-agents) để bắt đầu với Strands Agents!

## Hành trình của chúng tôi trong việc xây dựng AI agents

Tôi chủ yếu làm việc với [Amazon Q Developer](https://aws.amazon.com/q/developer/), một trợ lý được hỗ trợ bởi generative AI dành cho phát triển phần mềm. Nhóm của tôi và tôi bắt đầu xây dựng AI agents vào đầu năm 2023, khoảng thời gian khi bài báo khoa học gốc về [ReAct (Reasoning and Acting)](https://arxiv.org/pdf/2210.03629) được công bố. Bài báo này cho thấy rằng các large language models có thể suy luận, lập kế hoạch và thực hiện hành động trong môi trường của chúng. Ví dụ, các LLMs có thể suy luận rằng chúng cần thực hiện một API call để hoàn thành một nhiệm vụ, sau đó tạo ra các đầu vào cần thiết cho API call đó. Khi đó, chúng tôi nhận ra rằng các large language models có thể được sử dụng như các agents để hoàn thành nhiều loại nhiệm vụ, bao gồm cả phát triển phần mềm phức tạp và xử lý sự cố vận hành.

Vào thời điểm đó, các LLMs thường chưa được huấn luyện để hoạt động như các agents. Chúng thường được huấn luyện chủ yếu cho hội thoại ngôn ngữ tự nhiên. Việc sử dụng thành công một LLM để suy luận và hành động đòi hỏi phải có những prompt phức tạp hướng dẫn cách dùng tools, các bộ parser cho phản hồi của model, và logic điều phối (orchestration logic). Chỉ riêng việc khiến LLMs tạo ra JSON đúng cú pháp một cách ổn định đã là một thách thức lúc bấy giờ! Để tạo prototype và triển khai agents, nhóm của tôi phải dựa vào nhiều thư viện framework agent phức tạp, những thứ xử lý phần khung (scaffolding) và logic điều phối cần thiết để các agents có thể hoàn thành nhiệm vụ một cách ổn định với các model thế hệ trước. Ngay cả khi đã có các framework đó, chúng tôi vẫn phải mất nhiều tháng tinh chỉnh để đưa một agent vào production.

Kể từ đó, chúng tôi đã chứng kiến sự cải thiện vượt bậc trong khả năng suy luận và sử dụng tools để hoàn thành nhiệm vụ của các large language models. Chúng tôi nhận ra rằng mình không còn cần đến những orchestration phức tạp như trước để xây dựng agents, bởi vì các mô hình nay đã có khả năng lý luận và sử dụng công cụ một cách tự nhiên. Thực tế, một số thư viện framework agent mà chúng tôi từng dùng để xây dựng agents lại bắt đầu trở thành rào cản, khiến chúng tôi không thể tận dụng hết khả năng của các LLMs thế hệ mới. Mặc dù các LLMs đã tiến bộ rõ rệt, điều đó không đồng nghĩa rằng chúng tôi có thể xây dựng và lặp nhanh hơn với các frameworks cũ – việc đưa một agent sẵn sàng cho sản phẩm vẫn phải mất hàng tháng.

Chúng tôi đã bắt đầu xây dựng Strands Agents để loại bỏ sự phức tạp này cho các nhóm trong Q Developer. Chúng tôi nhận thấy rằng việc dựa vào khả năng mới nhất của các models để điều khiển agents giúp giảm đáng kể thời gian đưa sản phẩm ra thị trường và cải thiện trải nghiệm người dùng cuối, so với cách xây dựng agents bằng logic điều phối phức tạp. Nếu trước đây nhóm Q Developer phải mất hàng tháng để đưa một agent từ quy trình sang sản phẩm, thì nay chúng tôi có thể phát hành các agents mới chỉ trong vài ngày hoặc vài tuần với Strands.

## Các khái niệm cốt lõi của Strands Agents

Định nghĩa đơn giản nhất về một agent là sự kết hợp của ba yếu tố: 1) mô hình, 2) các công cụ, và 3) câu lệnh. Agent sử dụng ba thành phần này để hoàn thành một nhiệm vụ, thường theo cách tự động. Nhiệm vụ của agent có thể là trả lời một câu hỏi, sinh ra code, lên kế hoạch cho một chuyến du lịch, hoặc tối ưu danh mục tài chính của bạn. Trong cách tiếp cận model-driven, agent sử dụng mô hình để tự động định hướng các bước của mình và sử dụng công cụ nhằm hoàn thành nhiệm vụ đã được chỉ định.

![prompt diagram](/images/3-Translated-Blogs/Blog1_1.png)

Để định nghĩa một agent với Strands Agents SDK, bạn cần khai báo ba thành phần này trong code:

1. **Mô hình**: Strands cung cấp khả năng hỗ trợ mô hình linh hoạt. Bạn có thể sử dụng bất kỳ mô hình nào trong [Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/conversation-inference-supported-models-features.html) có hỗ trợ sử dụng công cụ (tool use) và truyền tải (streaming), một mô hình từ họ Claude của Anthropic thông qua [Anthropic API](https://www.anthropic.com/api), một mô hình từ họ Llama thông qua Llama API, [Ollama](https://ollama.com/) cho phát triển cục bộ, và nhiều nhà cung cấp mô hình khác như OpenAI thông qua [LiteLLM](https://docs.litellm.ai/docs/). Ngoài ra, bạn cũng có thể định nghĩa nhà cung cấp mô hình tùy chỉnh của riêng mình với Strands.
2. **Công cụ**: Bạn có thể chọn từ hàng ngàn máy chủ [Model Context Protocol (MCP)](https://modelcontextprotocol.io/examples) đã được công bố để sử dụng làm công cụ cho agent của bạn. Strands cũng cung cấp hơn [20 công cụ ví dụ dựng sẵn](https://strandsagents.com/0.1.x/user-guide/concepts/tools/tools_overview/#3-experimental-tools-package), bao gồm các công cụ để thao tác với tệp tin, thực hiện các yêu cầu API, và tương tác với AWS APIs. Bạn có thể dễ dàng sử dụng bất kỳ hàm Python nào như một công cụ, chỉ bằng cách dùng Strands `@tool` decorator.
3. **Câu lệnh**: Bạn cung cấp một câu lệnh bằng ngôn ngữ tự nhiên để định nghĩa nhiệm vụ cho agent, ví dụ như trả lời một câu hỏi từ người dùng cuối. Bạn cũng có thể cung cấp một câu lệnh hệ thống để đưa ra các hướng dẫn chung và hành vi mong muốn cho agent.

Một agent sẽ tương tác với mô hình và các công cụ của nó trong một vòng lặp cho đến khi hoàn thành nhiệm vụ được xác định bởi câu lệnh. Vòng lặp này là cốt lõi trong các khả năng của Strands. Vòng lặp của Strands tận dụng tối đa sức mạnh mà các LLMs đã đạt được và khả năng suy luận, lập kế hoạch, cũng như chọn công cụ một cách tự nhiên. Trong mỗi vòng lặp, Strands sẽ gọi LLM với câu lệnh và ngữ cảnh của agent, cùng với mô tả về các công cụ của agent. LLM có thể lựa chọn phản hồi bằng ngôn ngữ tự nhiên cho người dùng cuối của agent, lên kế hoạch cho một loạt bước, phản ánh lại các bước trước đó của agent, và/hoặc chọn một hoặc nhiều công cụ để sử dụng. Khi LLM chọn một công cụ, Strands sẽ đảm nhận việc thực thi công cụ đó và gửi kết quả trở lại cho LLM. Khi LLM hoàn thành nhiệm vụ, Strands sẽ trả về kết quả cuối cùng của agent.![agentic-loop](/images/3-Translated-Blogs/Blog1_2.png)

Trong cách tiếp cận định hướng theo mô hình của Strands, các công cụ (tools) là yếu tố then chốt để bạn tùy chỉnh hành vi của agents. Ví dụ, công cụ có thể truy xuất tài liệu liên quan từ một knowledge base, gọi APIs, chạy logic Python, hoặc đơn giản chỉ trả về một chuỗi tĩnh chứa các hướng dẫn bổ sung cho model. Công cụ cũng giúp bạn thực hiện các use case phức tạp trong cách tiếp cận định hướng theo mô hình, chẳng hạn như với các công cụ dựng sẵn trong Strands Agents:

- **Retrieve tool:** Công cụ này triển khai semantic search bằng [Amazon Bedrock Knowledge Bases](https://aws.amazon.com/bedrock/knowledge-bases/). Ngoài việc truy xuất tài liệu, retrieve tool còn có thể hỗ trợ model lập kế hoạch và suy luận bằng cách truy xuất các công cụ khác thông qua tìm kiếm ngữ nghĩa. Ví dụ, một agent nội bộ tại AWS có hơn 6.000 công cụ để lựa chọn! Các models hiện nay chưa có khả năng chọn chính xác trong số lượng công cụ lớn đến vậy. Thay vì mô tả cả 6.000 công cụ cho model, agent sử dụng tìm kiếm ngữ nghĩa để tìm ra những công cụ phù hợp nhất cho nhiệm vụ hiện tại và chỉ mô tả những công cụ đó cho model. Bạn có thể áp dụng cách này bằng cách lưu trữ nhiều mô tả công cụ trong một cơ sở tri thức và để model sử dụng retrieve tool để lấy ra tập hợp công cụ liên quan cho nhiệm vụ hiện tại.
- **Thinking tool:** Công cụ này nhắc model thực hiện phân tích chuyên sâu qua nhiều vòng lặp, cho phép xử lý tư duy phức tạp và tự phản chiếu như một phần của agent. Trong cách tiếp cận định hướng theo mô hình, việc mô hình hóa tư duy như một công cụ cho phép model suy luận xem một nhiệm vụ có cần đến phân tích sâu hay không, và khi nào cần.
- **Multi-agent tools như** workflow, graph và swarm tools: Với các nhiệm vụ phức tạp, Strands có thể điều phối nhiều agents theo nhiều mô hình cộng tác đa-agent khác nhau. Bằng cách mô hình hóa các sub-agents và sự cộng tác đa-agent như những công cụ, cách tiếp cận model-driven cho phép model suy luận xem một nhiệm vụ có yêu cầu một workflow, một graph, hay một swarm các sub-agents hay không. Strands sẽ sớm hỗ trợ Agent2Agent (A2A) protocol cho các ứng dụng multi-agent.

## Bắt đầu với Strands Agents

Hãy cùng đi qua một ví dụ về cách xây dựng một agent với Strands Agents SDK. [Như người ta thường nói](https://martinfowler.com/bliki/TwoHardThings.html), việc đặt tên là một trong những vấn đề khó nhất trong khoa học máy tính. Đặt tên cho một dự án mã nguồn mở cũng không ngoại lệ! Để giúp chúng tôi động não các tên tiềm năng cho dự án Strands Agents, tôi đã xây dựng một trợ lý AI đặt tên bằng Strands. Trong ví dụ này, bạn sẽ sử dụng Strands để xây dựng một naming agent với một mô hình mặc định trong Amazon Bedrock, một MCP server, và một công cụ dựng sẵn (pre-built tool) của Strands.

Tạo một file có tên `agent.py` với đoạn code sau:

```python
from strands import Agent
from strands.tools.mcp import MCPClient
from strands_tools import http_request
from mcp import stdio_client, StdioServerParameters

# Define a naming-focused system prompt
NAMING_SYSTEM_PROMPT = """
You are an assistant that helps to name open source projects.

When providing open source project name suggestions, always provide
one or more available domain names and one or more available GitHub
organization names that could be used for the project.

Before providing your suggestions, use your tools to validate
that the domain names are not already registered and that the GitHub
organization names are not already used.
"""

# Load an MCP server that can determine if a domain name is available
domain_name_tools = MCPClient(lambda: stdio_client(
    StdioServerParameters(command="uvx", args=["fastdomaincheck-mcp-server"])
))

# Use a pre-built Strands Agents tool that can make requests to GitHub
# to determine if a GitHub organization name is available
github_tools = [http_request]

with domain_name_tools:
    # Define the naming agent with tools and a system prompt
    tools = domain_name_tools.list_tools_sync() + github_tools
    naming_agent = Agent(
        system_prompt=NAMING_SYSTEM_PROMPT,
        tools=tools
    )

    # Run the naming agent with the end user's prompt
    naming_agent("I need to name an open source project for building AI agents.")
```

Bạn sẽ cần một [Khóa truy cập cá nhân của GITHUB](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) để chạy agent. Hãy thiết lập biến môi trường `GITHUB\_TOKEN` với giá trị là GitHub token của bạn. Bạn cũng sẽ cần quyền truy cập [Bedrock model](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access-modify.html) cho Anthropic Claude 3.7 Sonnet tại khu vực us-west-2, và AWS credentials đã được cấu hình cục bộ.

Bây giờ hãy chạy agent của bạn:

```python
pip install strands-agents strands-agents-tools

python -u agent.py
```

You should see output from the agent similar to this snippet:

```
Based on my checks, here are some name suggestions for your

open source AI agent building project:

## Project Name Suggestions:

1. **Strands Agents**

- Available domain: strandsagents.com

- Available GitHub organization: strands-agents
```

Bạn có thể dễ dàng bắt đầu xây dựng các agent mới ngay hôm nay với Strands Agents SDK trong công cụ phát triển được hỗ trợ AI mà bạn yêu thích. Để giúp bạn khởi đầu nhanh chóng, chúng tôi đã phát hành một Strands MCP server để sử dụng với bất kỳ công cụ phát triển nào có hỗ trợ MCP, chẳng hạn như [Q Developer CLI](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-installing.html) hoặc Cline. Đối với Q Developer CLI, hãy dùng ví dụ sau để thêm Strands MCP server vào [cấu hình MCP](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-mcp-configuration.html) của CLI. Bạn có thể xem thêm các ví dụ cấu hình khác trên [GitHub.](https://github.com/strands-agents/mcp-server/)

```json
{
  "mcpServers": {
    "strands": {
      "command": "uvx",

      "args": ["strands-agents-mcp-server"]
    }
  }
}
```

## Triển khai sản phẩm Strands Agents

Việc triển khai sản phẩm agents là một nguyên tắc quan trọng trong thiết kế của Strands. Dự án Strands Agents bao gồm một bộ công cụ triển khai [(deployment toolkit)](https://strandsagents.com/0.1.x/user-guide/deploy/operating-agents-in-production/) với tập hợp nhiều nguồn tham khảo để giúp bạn đưa ra sản phẩm agents. Strands đủ linh hoạt để hỗ trợ nhiều kiến trúc khác nhau trong sản phẩm. Bạn có thể dùng Strands để xây dựng các agents trò chyện cũng như các agents được kích hoạt bởi sự kiện, chạy theo lịch trình, hoặc chạy liên tục. Bạn có thể triển khai một agent được xây dựng với Strands Agents SDK dưới dạng monolith, nơi cả vòng lặp và việc thực thi tool cùng chạy trong một môi trường, hoặc dưới dạng một tập hợp các dịch vụ mini. Tôi sẽ mô tả bốn kiến trúc agent mà chúng tôi sử dụng nội bộ tại AWS với Strands Agents.

Sơ đồ dưới đây minh họa một kiến trúc agent với Strands chạy hoàn toàn cục bộ trong môi trường của người dùng thông qua một client application. [Ví dụ về công cụ dòng lệnh trên](https://github.com/strands-agents/agent-builder) GitHub áp dụng kiến trúc này cho một AI assistant dựa trên CLI để xây dựng agents.

![Run agent entirely locally](/images/3-Translated-Blogs/Blog1_3.png)

Sơ đồ tiếp theo minh họa một kiến trúc trong đó agent và các công cụ của nó được triển khai phía sau một API trong môi trường sản phẩm. Chúng tôi đã cung cấp các tài liệu tham khảo trên GitHub về cách triển khai các agents được xây dựng với Strands Agents SDK phía sau một API trên AWS, sử dụng <u>[AWS Lambda](https://strandsagents.com/0.1.x/user-guide/deploy/deploy_to_aws_lambda/), [AWS Fargate](https://strandsagents.com/0.1.x/user-guide/deploy/deploy_to_aws_fargate/), hoặc [Amazon Elastic Compute Cloud (Amazon EC2)](https://strandsagents.com/0.1.x/user-guide/deploy/deploy_to_amazon_ec2/)</u>.

![Run agent behind an AI](/images/3-Translated-Blogs/Blog1_4.png)

Bạn có thể tách biệt trách nhiệm giữa vòng lặp Strands và việc thực thi công cụ bằng cách chạy chúng trong các môi trường riêng biệt. Sơ đồ dưới đây minh họa một kiến trúc agent với Strands, trong đó agent gọi các công cụ của nó thông qua API, và các công cụ chạy trong một môi trường backend độc lập, tách rời với môi trường của agent. Ví dụ, bạn có thể chạy các công cụ của agent trong Lambda functions, trong khi chạy bản thân agent trong một khối Fargate.

![Run tools in an isolated backend environment](/images/3-Translated-Blogs/Blog1_5.png)

Bạn cũng có thể triển khai mô hình return-of-control với Strands, trong đó client chịu trách nhiệm chạy các công cụ. Sơ đồ này minh họa một kiến trúc agent, nơi một agent được xây dựng với Strands Agents SDK có thể sử dụng kết hợp cả các công cụ được lưu trữ trong môi trường backend và các công cụ chạy cục bộ thông qua một client application gọi tới agent.

![Run a mix of tools in the user's environment and in the backend](/images/3-Translated-Blogs/Blog1_6.png)

Bất kể kiến trúc cụ thể của bạn như thế nào, khả năng quan sát của agents vẫn rất quan trọng để hiểu cách chúng hoạt động trong môi trường sản phẩm. Strands cung cấp cơ chế đo đạc để thu thập quỹ đạo và số liệu từ các sản phẩm agents. Strands sử dụng OpenTelemetry (OTEL) để phát dữ liệu đo đạc từ xa đến bất kỳ backend nào tương thích với OTEL nhằm phục vụ trực quan hóa, xử lý sự cố và đánh giá. Hỗ trợ theo dõi phân tán của Strands cho phép bạn theo dõi các request đi qua các thành phần khác nhau trong kiến trúc của mình, nhằm cung cấp một bức tranh toàn diện về các phiên làm việc của agent.

## Tham gia cộng đồng Strands Agents

Strands Agents là một dự án mã nguồn mở được cấp phép theo Apache License 2.0. Chúng tôi rất háo hức khi có thể cùng bạn xây dựng Strands một cách công khai. Chúng tôi hoan nghênh mọi đóng góp cho dự án, bao gồm việc bổ sung hỗ trợ cho các mô hình và công cụ từ những nhà cung cấp khác, hợp tác phát triển các tính năng mới, hoặc mở rộng tài liệu. Nếu bạn phát hiện lỗi, có đề xuất, hoặc muốn đóng góp điều gì đó, hãy tham gia cùng chúng tôi trên [GitHub.](https://github.com/strands-agents)

Để tìm hiểu thêm về Strands Agents và bắt đầu xây dựng AI agent đầu tiên của bạn với Strands, hãy truy cập vào [tài liệu hướng dẫn](https://github.com/strands-agents/docs) của chúng tôi.

|                                                          |                                                                                                                                                                                                                                                                                          |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![Clare Liguori](/images/3-Translated-Blogs/Blog1_7.jpg) | **Clare Liguori** <br> Clare Liguori is a Senior Principal Software Engineer for AWS Agentic AI. She focuses on re-imagining how applications are built and how productive developers can be when their tools are powered by generative AI and AI agents, as part of Amazon Q Developer. |
