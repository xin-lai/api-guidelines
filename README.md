# api-guidelines
翻译自微软的Microsoft REST API Guidelines
* 原文链接：https://github.com/Microsoft/api-guidelines/blob/vNext/Guidelines.md


# Microsoft REST API Guidelines

<div style="font-size:150%">

Document editors: John Gossman (C+E), Chris Mullins (ASG), Gareth Jones (ASG), Rob Dolin (C+E), Mark Stafford (C+E)<br/>

</div>





# Microsoft REST API Guidelines

## 1 Abstract 摘要

The Microsoft REST API Guidelines, as a design principle, encourages application developers to have resources accessible to them via a RESTful HTTP interface.

作为设计原则，Microsoft REST API 指南鼓励程序开发人员通过 RESTful HTTP 接口访问获取资源。

To provide the smoothest possible experience for developers on platforms following the Microsoft REST API Guidelines, REST APIs SHOULD follow consistent design guidelines to make using them easy and intuitive.

为了给遵循 Microsoft REST API 指南的平台上的开发者提供最流畅的体验，REST API 应该遵循一致的设计准则，以便使它们用起来简单直观。

This document establishes the guidelines Microsoft REST APIs SHOULD follow so RESTful interfaces are developed consistently.

本文档建立了 Microsoft REST APIs 设计指南，用以保证 RESTful 接口开发的一致性。



## 2 Table of contents 目录

<!-- TOC depthFrom:1 depthTo:3 withLinks:1 updateOnSave:1 orderedList:0 -->



- [Microsoft REST API Guidelines 2.3](#microsoft-rest-api-guidelines-23)

    - [Microsoft REST API Guidelines Working Group](#microsoft-rest-api-guidelines-working-group)

- [Microsoft REST API Guidelines](#microsoft-rest-api-guidelines)

    - [1 Abstract](#1-abstract)

    - [2 Table of contents](#2-table-of-contents)

    - [3 Introduction](#3-introduction)

        - [3.1 Recommended reading](#31-recommended-reading)

    - [4    Interpreting the guidelines](#4-interpreting-the-guidelines)

        - [4.1    Application of the guidelines](#41-application-of-the-guidelines)

        - [4.2    Guidelines for existing services and versioning of services](#42-guidelines-for-existing-services-and-versioning-of-services)

        - [4.3    Requirements language](#43-requirements-language)

        - [4.4    License](#44-license)

    - [5 Taxonomy](#5-taxonomy)

        - [5.1    Errors](#51-errors)

        - [5.2    Faults](#52-faults)

        - [5.3    Latency](#53-latency)

        - [5.4    Time to complete](#54-time-to-complete)

        - [5.5    Long running API faults](#55-long-running-api-faults)

    - [6    Client guidance](#6-client-guidance)

        - [6.1    Ignore rule](#61-ignore-rule)

        - [6.2    Variable order rule](#62-variable-order-rule)

        - [6.3    Silent fail rule](#63-silent-fail-rule)

    - [7    Consistency fundamentals](#7-consistency-fundamentals)

        - [7.1    URL structure](#71-url-structure)

        - [7.2    URL length](#72-url-length)

        - [7.3    Canonical identifier](#73-canonical-identifier)

        - [7.4    Supported methods](#74-supported-methods)

        - [7.5    Standard request headers](#75-standard-request-headers)

        - [7.6    Standard response headers](#76-standard-response-headers)

        - [7.7    Custom headers](#77-custom-headers)

        - [7.8    Specifying headers as query parameters](#78-specifying-headers-as-query-parameters)

        - [7.9    PII parameters](#79-pii-parameters)

        - [7.10   Response formats](#710-response-formats)

        - [7.11   HTTP Status Codes](#711-http-status-codes)

        - [7.12   Client library optional](#712-client-library-optional)

    - [8    CORS](#8-cors)

        - [8.1    Client guidance](#81-client-guidance)

        - [8.2    Service guidance](#82-service-guidance)

    - [9    Collections](#9-collections)

        - [9.1    Item keys](#91-item-keys)

        - [9.2    Serialization](#92-serialization)

        - [9.3    Collection URL patterns](#93-collection-url-patterns)

        - [9.4    Big collections](#94-big-collections)

        - [9.5    Changing collections](#95-changing-collections)

        - [9.6    Sorting collections](#96-sorting-collections)

        - [9.7    Filtering](#97-filtering)

        - [9.8    Pagination](#98-pagination)

        - [9.9    Compound collection operations](#99-compound-collection-operations)

    - [10    Delta queries](#10-delta-queries)

        - [10.1    Delta links](#101-delta-links)

        - [10.2    Entity representation](#102-entity-representation)

        - [10.3    Obtaining a delta link](#103-obtaining-a-delta-link)

        - [10.4    Contents of a delta link response](#104-contents-of-a-delta-link-response)

        - [10.5    Using a delta link](#105-using-a-delta-link)

    - [11    JSON standardizations](#11-json-standardizations)

        - [11.1    JSON formatting standardization for primitive types](#111-json-formatting-standardization-for-primitive-types)

        - [11.2    Guidelines for dates and times](#112-guidelines-for-dates-and-times)

        - [11.3    JSON serialization of dates and times](#113-json-serialization-of-dates-and-times)

        - [11.4    Durations](#114-durations)

        - [11.5    Intervals](#115-intervals)

        - [11.6    Repeating intervals](#116-repeating-intervals)

    - [12    Versioning](#12-versioning)

        - [12.1    Versioning formats](#121-versioning-formats)

        - [12.2    When to version](#122-when-to-version)

        - [12.3    Definition of a breaking change](#123-definition-of-a-breaking-change)

    - [13    Long running operations](#13-long-running-operations)

        - [13.1    Resource based long running operations (RELO)](#131-resource-based-long-running-operations-relo)

        - [13.2    Stepwise long running operations](#132-stepwise-long-running-operations)

        - [13.3    Retention policy for operation results](#133-retention-policy-for-operation-results)

    - [14    Push notifications via webhooks](#14-push-notifications-via-webhooks)

        - [14.1    Scope](#141-scope)

        - [14.2    Principles](#142-principles)

        - [14.3    Types of subscriptions](#143-types-of-subscriptions)

        - [14.4    Call sequences](#144-call-sequences)

        - [14.5    Verifying subscriptions](#145-verifying-subscriptions)

        - [14.6    Receiving notifications](#146-receiving-notifications)

        - [14.7    Managing subscriptions programmatically](#147-managing-subscriptions-programmatically)

        - [14.8    Security](#148-security)

    - [15    Unsupported requests](#15-unsupported-requests)

        - [15.1    Essential guidance](#151-essential-guidance)

        - [15.2    Feature allow list](#152-feature-allow-list)

    - [16    Naming guidelines](#16-naming-guidelines)

        - [16.1    Approach](#161-approach)

        - [16.2    Casing](#162-casing)

        - [16.3    Names to avoid](#163-names-to-avoid)

        - [16.4    Forming compound names](#164-forming-compound-names)

        - [16.5    Identity properties](#165-identity-properties)

        - [16.6    Date and time properties](#166-date-and-time-properties)

        - [16.7    Name properties](#167-name-properties)

        - [16.8    Collections and counts](#168-collections-and-counts)

        - [16.9    Common property names](#169-common-property-names)

    - [17     Appendix](#17-appendix)

        - [17.1    Sequence diagram notes](#171-sequence-diagram-notes)



<!-- /TOC -->



## 3 Introduction 引言

Developers access most Microsoft Cloud Platform resources via HTTP interfaces.

开发者基本都通过 HTTP 接口访问微软云平台的资源。

Although each service typically provides language-specific frameworks to wrap their APIs, all of their operations eventually boil down to HTTP requests.

尽管每个服务通过特定语言的框架封装了它们的 API，但它们的所有操作最终都归结为 HTTP 请求。

Microsoft must support a wide range of clients and services and cannot rely on rich frameworks being available for every development environment.

微软必须支持多种类型的客户端和服务，且不能依赖于各个开发环境丰富的框架。

Thus a goal of these guidelines is to ensure Microsoft REST APIs can be easily and consistently consumed by any client with basic HTTP support.

因此，这些准则的一个目标是确保任何支持基本 HTTP 协议的客户端都可以简单且一致地使用 Microsoft REST API。



To provide the smoothest possible experience for developers, it's important to have these APIs follow consistent design guidelines, thus making using them easy and intuitive.

为了给开发人员提供最流畅的开发体验，让这些 API 遵循一致的设计准则非常重要，从而使它们用起来简单直观。

This document establishes the guidelines to be followed by Microsoft REST API developers for developing such APIs consistently.

本文档建立了 Microsoft REST API 开发人员应该遵循的指南, 以便统一一致地开发 API。



The benefits of consistency accrue in aggregate as well; consistency allows teams to leverage common code, patterns, documentation and design decisions.

一致性的好处也会累积起来;一致性使团队拥有通用的代码、模式设计、文档风格和设计决策。



These guidelines aim to achieve the following:

- Define consistent practices and patterns for all API endpoints across Microsoft.

- Adhere as closely as possible to accepted REST/HTTP best practices in the industry at-large.*

- Make accessing Microsoft Services via REST interfaces easy for all application developers.

- Allow service developers to leverage the prior work of other services to implement, test and document REST endpoints defined consistently.

- Allow for partners (e.g., non-Microsoft entities) to use these guidelines for their own REST endpoint design.



这些准则旨在实现以下目标:

- 为 Microsoft 技术平台中的所有 API 端点定义一致的实现和模式。

- 尽可能地遵循行业普遍接受的 REST/HTTP 最佳实践。

- 使所有程序开发人员都可以简单地通过 REST 接口访问 Microsoft 服务。

- 允许 Service 开发人员利用其他 Service 的工作基础来开发一致的 REST 端点。

- 允许合作伙伴 (如非微软团队) 使用这些准则进行其自己的 REST 端点设计。



*Note: The guidelines are designed to align with building services which comply with the REST architectural style, though they do not address or require building services that follow the REST constraints.

The term "REST" is used throughout this document to mean services that are in the spirit of REST rather than adhering to REST by the book.*

*注：本指南旨在构建符合 REST 架构风格的服务，但不涉及或要求构建遵循 REST 约束的服务。

本文档中使用的“REST”术语代指具有 REST 灵魂的服务，而不是仅仅遵循 REST。



### 3.1 Recommended reading 推荐阅读

Understanding the philosophy behind the REST Architectural Style is recommended for developing good HTTP-based services.

了解 REST 架构风格背后的一些理念，更有助于开发优秀的基于 HTTP 的服务。

If you are new to RESTful design, here are some good resources:

如果您对 RESTful 设计不熟悉，请参阅以下优秀资源：



[REST on Wikipedia][rest-on-wikipedia] -- Overview of common definitions and core ideas behind REST.

概述 REST 背后的常见定义和核心思想。



[REST Dissertation][fielding] -- The chapter on REST in Roy Fielding's dissertation on Network Architecture, "Architectural Styles and the Design of Network-based Software Architectures"

在 Roy Fielding 关于网络体系结构的论文中"架构风格与基于网络的软件体系结构设计" 一章。



[RFC 7231][rfc-7231] -- Defines the specification for HTTP/1.1 semantics, and is considered the authoritative resource.

HTTP/1.1 语义规范的权威资料。



[REST in Practice][rest-in-practice] -- Book on the fundamentals of REST.

关于 REST 的入门书籍。



## 4 Interpreting the guidelines 详解准则

### 4.1 Application of the guidelines 准则的应用

These guidelines are applicable to any REST API exposed publicly by Microsoft or any partner service.

Private or internal APIs SHOULD also try to follow these guidelines because internal services tend to eventually be exposed publicly.

这些准则适用于 Microsoft 或其合作伙伴公开发布的服务的所有 REST API。

私人或内部的 API 也大致遵循这些准则, 因为内部服务往往最终会公开。

Consistency is valuable to not only external customers but also internal service consumers, and these guidelines offer best practices useful for any service.

保证一致性不仅对外部客户有利, 而且对内部服务的使用者也很有价值, 这些准则为所有的服务提供了最佳实践。



There are legitimate reasons for exemption from these guidelines.

Obviously a REST service that implements or must interoperate with some externally defined REST API must be compatible with that API and not necessarily these guidelines.

Some services MAY also have special performance needs that require a different format, such as a binary protocol.

当然有许多合理理由可以不遵循这些准则。

显然，实现或必须与某些外部定义的 REST API 互操作的 REST 服务必须与那些 API 兼容，而无法遵循这些准则。

还有一些服务可能由于对性能的特殊的需求，必须使用其他格式，如采用二进制协议。



### 4.2 Guidelines for existing services and versioning of services 现有服务和服务版本控制的准则

We do not recommend making a breaking change to a service that pre-dates these guidelines simply for compliance sake.

我们不建议为了遵循这些准则，而过早的对老服务进行重大更改。

The service SHOULD try to become compliant at the next version release when compatibility is being broken anyway.

When a service adds a new API, that API SHOULD be consistent with the other APIs of the same version.

无论如何，当兼容性被破坏时，该服务应该尝试在下一版本中变得合规。

当一个服务添加新的 API 时，该 API 应该与同一版本的其他 API 保持一致。

So if a service was written against version 1.0 of the guidelines, new APIs added incrementally to the service SHOULD also follow version 1.0. The service can then upgrade to align with the latest version of the guidelines at the service's next major release.

因此，如果服务是针对 1.0 版本的指南编写的，那么增量添加到服务的新 API 也应该遵循 1.0 版本指南。然后该服务在下一次主要版本更新时，再去遵循最新版指南。

### 4.3 Requirements language 用词释义

The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

本文档中的"MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", 和 "OPTIONAL" 等关键字的详细解释见 [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt)。

### 4.4 License 许可

This work is licensed under the Creative Commons Attribution 4.0 International License.

To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/ or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA.





## 5 Taxonomy 分类

As part of onboarding to Microsoft REST API Guidelines, services MUST comply with the taxonomy defined below.

Microsoft REST API 准则基本要求的一方面就是 服务的分类必须符合以下定义。



### 5.1 Errors

Errors, or more specifically Service Errors, are defined as a client passing invalid data to the service and the service _correctly_ rejecting that data.

错误或更具体的服务错误，被定义为客户端将无效数据传递给服务并且服务_明确地_拒绝该数据。

Examples include invalid credentials, incorrect parameters, unknown version IDs, or similar.

例如无效的证书，错误的参数，未知的版本 ID 等。

These are generally "4xx" HTTP error codes and are the result of a client passing incorrect or invalid data.

通常是由于客户端传递错误或无效数据的导致的服务器返回 “4xx” 的 HTTP 错误代码。



Errors do _not_ contribute to overall API availability.

错误*不*会影响整体 API 的可用性。



### 5.2 Faults

Faults, or more specifically Service Faults, are defined as the service failing to correctly return in response to a valid client request.

Faults 或更具体地说服务故障被定义为服务无法正确返回数据以响应有效的客户端请求。

These are generally "5xx" HTTP error codes.

通常会返回 “5xx” HTTP 错误代码。



Faults _do_ contribute to the overall API availability.

故障_会_影响整体 API 的可用性。



Calls that fail due to rate limiting or quota failures MUST NOT count as faults.

由于速率限制或配额不足导致失败的调用**绝不能**算作故障。

Calls that fail as the result of a service fast-failing requests (often for its own protection) do count as faults.

由于服务 fast-failing 请求而失败的调用（通常是为了保护自己）**会**被视为错误。



### 5.3 Latency 延迟

Latency is defined as how long a particular API call takes to complete, measured as closely to the client as possible.

延迟定义为具体 API 被调用完成所需的时长, 尽可能用客户端进行测量。

This metric applies to both synchronous and asynchronous APIs in the same way.

这种测量方法同样适用于同步和异步 API。

For long running calls, the latency is measured on the initial request and measures how long that call (not the overall operation) takes to complete.

对于长时间运行的调用，延迟定义为第一次调用它所需的时长，而非它长时间运行的时长。



### 5.4 Time to complete 完成时间

Services that expose long operations MUST track "Time to Complete" metrics around those operations.

暴露长时间操作的服务**必须**跟踪这些操作的 "Time to Complete" 指标。



### 5.5 Long running API faults 长时间运行故障

For a Long Running API, it's possible for both the initial request to begin the operation and the request to retrieve the results to technically work (each passing back a 200), but for the underlying operation to have failed.

对于长时间运行的 API，很可能出现初始请求成功，且后续每次去获取结果时 API 也处于*正常运行*（每次都回传 200）中，但其底层操作已经失败了的情况。

Long Running faults MUST roll up as Faults into the overall Availability metrics.

长时间运行故障**必须**作为故障汇总到总体可用性指标中。



## 6 Client guidance 客户端指南

To ensure the best possible experience for clients talking to a REST service, clients SHOULD adhere to the following best practices:

为了保证与 REST Service 进行通讯的客户端有最好的体验，客户端应该遵循以下最佳实践：



### 6.1 Ignore rule 

For loosely coupled clients where the exact shape of the data is not known before the call, if the server returns something the client wasn't expecting, the client MUST safely ignore it.

对于在调用前数据格式不确定的松耦合客户端，如果服务器返回的数据格式错误，客户端**必须**安全地忽略掉它。

  

Some services MAY add fields to responses without changing versions numbers.

Services that do so MUST make this clear in their documentation and clients MUST ignore unknown fields.

有的 Service 可能会未更新版本号就在响应中增加字段。

Service 这样做的**必须**在它的文档中进行明确说明，且客户端必须忽略掉这些未知的字段。



### 6.2 Variable order rule 变量排序规则

Clients MUST NOT rely on the order in which data appears in JSON service responses.

客户端处理响应数据时**一定不能**依赖 服务器 JSON 响应数据的顺序。

For example, clients SHOULD be resilient to the reordering of fields within a JSON object.

When supported by the service, clients MAY request that data be returned in a specific order.

例如，当服务器返回的 JSON 对象中的字段顺序变了，客户端也能正确进行解析处理。

当服务端支持时，客户端**可以**请求有特定顺序的数据。

For example, services MAY support the use of the _$orderBy_ querystring parameter to specify the order of elements within a JSON array.

Services MAY also explicitly specify the ordering of some elements as part of the service contract.

例如，服务器**可以**支持使用查询参数 orderBy 来使服务器返回有序的 JSON 数组。

服务端也**可以**在协议中明确指定某些元素按特定方式进行排序。

For example, a service MAY always return a JSON object's "type" information as the first field in an object to simplify response parsing on the client.

例如，服务端**可以**每次返回 JSON 对象时都把 JSON 对象的*类型*放在最前面，进而简化客户端解析返回数据的难度。

Clients MAY rely on ordering behavior explicitly identified by the service.

客户端处理数据时**可以**依赖于服务端明确指定了的排序行为。



### 6.3 Silent fail rule

Clients requesting OPTIONAL server functionality (such as optional headers) MUST be resilient to the server ignoring that particular functionality.

当客户端请求带可选功能的 Service 时（例如带可选的 header）**必须**对 Service 有弹性，可以忽略该特定功能。



## 7 Consistency fundamentals 一致性基础

### 7.1 URL structure URL 链接格式

Humans SHOULD be able to easily read and construct URLs.

URL 应该是易读且格式清晰明了的。



This facilitates discovery and eases adoption on platforms without a well-supported client library.

这有助于简化和推广在没有良好支持的客户端库平台上的使用。



An example of a well-structured URL is:

格式良好的 URL：

```

https://api.contoso.com/v1.0/people/jdoe@contoso.com/inbox

```



An example URL that is not friendly is:

格式不友好的 URL：

```

https://api.contoso.com/EWS/OData/Users('jdoe@microsoft.com')/Folders('AAMkADdiYzI1MjUzLTk4MjQtNDQ1Yy05YjJkLWNlMzMzYmIzNTY0MwAuAAAAAACzMsPHYH6HQoSwfdpDx-2bAQCXhUk6PC1dS7AERFluCgBfAAABo58UAAA=')

```



A frequent pattern that comes up is the use of URLs as values.

Services MAY use URLs as values.

For example, the following is acceptable:

一个常见的模式是使用 URL 作为值。

Service **可以**使用 URL 作为值。

例如下例：

```

https://api.contoso.com/v1.0/items?url=https://resources.contoso.com/shoes/fancy

```



### 7.2 URL length 长度

The HTTP 1.1 message format, defined in RFC 7230, in section [3.1.1][rfc-7230-3-1-1], defines no length limit on the Request Line, which includes the target URL.

在 RFC 7230 [3.1.1] [rfc-7230-3-1-1] 章节中定义的 HTTP 1.1 消息格式，定义 Requset（包括其中的目标 URL）没有长度限制。

From the RFC:



> HTTP does not place a predefined limit on the length of a

   request-line. [...] A server that receives a request-target longer than any URI it wishes to parse MUST respond

   with a 414 (URI Too Long) status code.



Services that can generate URLs longer than 2,083 characters MUST make accommodations for the clients they wish to support.

当 Service 生成的 URL 长度超过 2083 个字符时**必须**考虑如何兼容将支持的客户端。

Here are some sources for determining what target clients support:

不同客户端支持的最长 URL 长度参见以下资料：



 * [http://stackoverflow.com/a/417184](http://stackoverflow.com/a/417184)

 * [https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/](https://blogs.msdn.microsoft.com/ieinternals/2014/08/13/url-length-limits/)

 

Also note that some technology stacks have hard and adjustable url limits, so keep this in mind as you design your services.

另请注意，某些技术栈具有难以调整的 url 限制，因此请在设计 service 时牢记这点。



### 7.3 Canonical identifier 规范的标识符

In addition to friendly URLs, resources that can be moved or be renamed SHOULD expose a URL that contains a unique stable identifier.

除了友好的 URL 之外，可以移动或重命名的资源也**应该**暴露一个包含唯一固定标识符的 URL。

It MAY be necessary to interact with the service to obtain a stable URL from the friendly name for the resource, as in the case of the "/my" shortcut used by some services.

在与 Service 进行交互时**可能**需要通过友好的名称来获取资源固定的 URL，例如某些 Service 使用的“/my”快捷方式。



The stable identifier is not required to be a GUID.

固定标识符不一定必需得是 GUID。



An example of a URL containing a canonical identifier is:

包含规范标识符的 URL 示例：

```

https://api.contoso.com/v1.0/people/7011042402/inbox

```
