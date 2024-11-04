
为了构建生成式AI应用，需要完成两个部分：


* AI大模型服务：有两种方式实现，可以使用大厂的API，也可以自己部署，本文将采用ollama来构建
* 应用构建：调用AI大模型的能力实现业务逻辑，本文将采用Spring Boot \+ Spring AI来实现


## Ollama安装与使用


![](https://static.didispace.com/images3/d888726bdabc2f682d2c0019e54b2289.png)


进入官网：[https://ollama.com/](https://github.com) ，下载、安装、启动 ollama


具体步骤可以参考我之前的这篇文章：[手把手教你本地运行Meta最新大模型：Llama3\.1](https://github.com)


## 构建 Spring 应用


1. 通过[spring initializr](https://github.com):[FlowerCloud机场](https://hanlianfangzhi.com)创建Spring Boot应用
2. 注意右侧选择Spring Web和Spring AI对Ollama的支持依赖


![](https://static.didispace.com/images3/72e62dbb3e952d2fe56299d2cf441cf2.png)


3. 点击“generate”按钮获取工程
4. 使用IDEA或者任何你喜欢的工具打开该工程，工程结构如下；


![](https://static.didispace.com/images3/09bb98a0568febe3851e50ea14dee103.png)


5. 写个单元测试，尝试在Spring Boot应用里调用本地的ollama服务



```
@SpringBootTest(classes = DemoApplication.class)
class DemoApplicationTests {

    @Autowired
    private OllamaChatModel chatModel;

    @Test
    void ollamaChat() {
        ChatResponse response = chatModel.call(
                new Prompt(
                        "Spring Boot适合做什么？",
                        OllamaOptions.builder()
                                .withModel(OllamaModel.LLAMA3_1)
                                .withTemperature(0.4)
                                .build()
                ));
        System.out.println(response);
    }

}

```

运行得到如下输出：



```
ChatResponse [metadata={ id: , usage: { promptTokens: 17, generationTokens: 275, totalTokens: 292 }, rateLimit: org.springframework.ai.chat.metadata.EmptyRateLimit@7b3feb26 }, generations=[Generation[assistantMessage=AssistantMessage [messageType=ASSISTANT, toolCalls=[], textContent=Spring Boot是一个基于Java的快速开发框架，主要用于创建独立的、生产级别的应用程序。它提供了一个简化的配置过程，使得开发者能够快速构建和部署Web应用程序。

Spring Boot适合做以下几件事情：

1. **快速开发**: Spring Boot提供了一系列的自动配置功能，可以帮助开发者快速创建基本的应用程序，减少手动编写配置代码的时间。
2. **独立运行**: Spring Boot可以作为一个独立的应用程序运行，不需要额外的容器或服务器支持。
3. **生产级别的应用程序**: Spring Boot提供了许多生产级别的特性，例如安全、监控和部署等功能，可以帮助开发者创建高性能、可靠的应用程序。
4. **Web 应用程序**: Spring Boot可以用于创建Web应用程序，包括RESTful API、WebSockets和其他类型的Web应用程序。
5. **微服务架构**: Spring Boot支持微服务架构，允许开发者将一个大型应用程序分解成多个小型服务，每个服务都可以独立运行和部署。

总之，Spring Boot是一个强大的框架，可以帮助开发者快速创建、测试和部署生产级别的应用程序。, metadata={messageType=ASSISTANT}], chatGenerationMetadata=ChatGenerationMetadata{finishReason=stop,contentFilterMetadata=null}]]]

```


> 上述样例工程打包放公众号了，如果需要的话，关注"程序猿DD"，发送关键词`spring+ollama`获得下载链接。


## 小结


通过本文的介绍，我们就已经完成了Spring Boot应用与Ollama运行的AI模型之间的对接。剩下的就是与业务逻辑的结合实现，这里读者根据自己的需要去实现即可。


**可能存在的一些疑问**


1. 如何使用其他AI模型


通过ollama的 [Models](https://github.com) 页面，可以找到各种其他模型：


![](https://static.didispace.com/images3/d36e6e37d302d7de3578d143f24af8d9.png)


选择你要使用的模型来启动即可。


2. 如何植入现有应用？


打开上面工程的`pom.xml`，可以看到主要就下面两个依赖：



```
<dependency>
  <groupId>org.springframework.bootgroupId>
  <artifactId>spring-boot-starter-webartifactId>
dependency>
<dependency>
  <groupId>org.springframework.aigroupId>
  <artifactId>spring-ai-ollama-spring-boot-starterartifactId>
dependency>

```

所以，如果要在现有工程引入的话只要引入`spring-ai-ollama-spring-boot-starter`依赖就可以了。


好了，今天的分享就到这里。最近较忙，分享较少，感谢持续的关注与支持 \_


如果您学习过程中如遇困难？可以加入我们超高质量的[技术交流群](https://github.com)，参与交流与讨论，更好的学习与进步！


