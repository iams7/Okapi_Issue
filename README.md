# Okapi_Issue

As of now, we are trying to take input sources as files (16 file formats on development process). 
* Firstly, We'd like to take input source from the end-user as a simple text file _(*.txt)_ and extract the translatable text contents sentence-wise (text unit wise).
* Secondly, setting up Machine Translation Engines for Translation Process where each segmented sentences will be translated. 
* Finally, to finish up the process by merging back to the same file format.</br>

## Issue persisting in the implementation of Okapi Framework
In the current process, the problems persists on extracting translatable text content from the input **MS Word (.docx)** file. That is each sentences are segmented using following code.

```
ISegments segs = tc.getSegments();
segs.create(segmenter.getRanges());
```
Then reading the sengented sentences one by one using for loop.

```
 // 	Obtain segmented sentences one by one
	for (Segment seg : segs) {

	TextFragment tf = seg.getContent();
	String segmented_text = seg.toString();
	String coded_text = tf.getCodedText();
	String text = tf.getText();
	List<Code> codes = tf.getCodes();

	System.out.println("\n");
	System.out.println("SEG==>"+seg.toString());
	System.out.println("GET CODES==>"+codes);
	System.out.println("CODED Text ==>"+coded_text);
	System.out.println("JUST TEXT ==>" + text);
	System.out.println("\n");
	}
```
The output of the above code as follows,

![2021-04-08 13_09_07-translate – mainProcessor java](https://user-images.githubusercontent.com/15103613/113988425-17051b80-986d-11eb-8414-6d8f47ce52a2.png)

# Issues
But if we use the '**_coded_text_**' to show up on the workspace, we are getting these 2 major issues

## 1. Messing up with the **_coded_text_** at the workspace window.
![2021-04-08 12_48_13-Workspace Editor](https://user-images.githubusercontent.com/15103613/113986030-6ac23580-986a-11eb-88b4-1e3f33e50b4a.png)

### Coded text and its translation are getting added into the database table on saving each sentences as follows
![2021-04-08 12_20_58-DB Browser for SQLite - D__Sridhar Workspace_Ailaysa io_Backend_dj_ailaysa_db_ai](https://user-images.githubusercontent.com/15103613/113988708-5cc1e400-986d-11eb-87fa-111a027c9e2a.png)

### And the result we get using Postman API Browser as follows,
![2021-04-08 12_16_32-Postman](https://user-images.githubusercontent.com/15103613/113989214-d954c280-986d-11eb-8e10-5c0be194c773.png)
The above text will be written to the new MS Word (text) file which will be downloaded later

## 2. And the file gets corrupted during download (after post editing process). 
The respecting data obtaied from the console terminal are as follows,
```
web_1             | [08/Apr/2021 06:42:45] "POST /api/login HTTP/1.1" 200 217
web_1             | [08/Apr/2021 06:42:46] "GET /api/getLangs HTTP/1.1" 200 2879
web_1             | [08/Apr/2021 06:42:46] "GET /api/fileUpload HTTP/1.1" 200 28
web_1             | [08/Apr/2021 06:42:48] "POST /api/target-language-list HTTP/1.1" 200 811
web_1             | [08/Apr/2021 06:42:54] "POST /api/fileUpload HTTP/1.1" 200 128
spring-service_1  | [http-nio-8080-exec-1] INFO org.apache.catalina.core.ContainerBase.[Tomcat].[localhost].[/] - Initializing Spring DispatcherServlet 'dispatcherServlet'
spring-service_1  | [http-nio-8080-exec-1] INFO org.springframework.web.servlet.DispatcherServlet - Initializing Servlet 'dispatcherServlet'
spring-service_1  | [http-nio-8080-exec-1] DEBUG org.springframework.web.servlet.DispatcherServlet - Detected StandardServletMultipartResolver
spring-service_1  | [http-nio-8080-exec-1] DEBUG org.springframework.web.servlet.DispatcherServlet - enableLoggingRequestDetails='false': request parameters and headers will be masked to prevent unsafe logging of potentially sensitive data
spring-service_1  | [http-nio-8080-exec-1] INFO org.springframework.web.servlet.DispatcherServlet - Completed initialization in 10 ms
spring-service_1  | [http-nio-8080-exec-1] DEBUG org.springframework.web.servlet.DispatcherServlet - POST "/file/getDocument/", parameters={masked}
spring-service_1  | [http-nio-8080-exec-1] DEBUG org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping - Mapped to com.ailaysa.translate.controller.FileController#getDocument(String, String, String)
spring-service_1  | /code/media/Dummy Vendor/Test_4_3cRuLSx.docx
spring-service_1  | filename= /code/media/Dummy Vendor/Test_4_3cRuLSx.docx
spring-service_1  | open-xml-processor
spring-service_1  | event===>Start Document
spring-service_1  | event===>Document Part
spring-service_1  | event===>Document Part
spring-service_1  | event===>Document Part
spring-service_1  | event===>Start SubDocument
spring-service_1  | event===>Document Part
spring-service_1  | event===>Text Unit
spring-service_1  |
spring-service_1  |
spring-service_1  | SEG==>I need to <run1>complete </run1>the task by <run2>today</run2>.
spring-service_1  | GET CODES==>[<run1>, </run1>, <run2>, </run2>]
spring-service_1  | CODED Text ==>I need to complete the task by today.
spring-service_1  | JUST TEXT ==>I need to complete the task by today.
spring-service_1  |
spring-service_1  |
spring-service_1  |
spring-service_1  |
spring-service_1  | SEG==> I also need to <run3>go out </run3><run4>today</run4><run5>.</run5>
spring-service_1  | GET CODES==>[<run3>, </run3>, <run4>, </run4>, <run5>, </run5>]
spring-service_1  | CODED Text ==> I also need to go out today.
spring-service_1  | JUST TEXT ==> I also need to go out today.
spring-service_1  |
spring-service_1  |
spring-service_1  | event===>Document Part
spring-service_1  | event===>End SubDocument
spring-service_1  | event===>Document Part
spring-service_1  | event===>Start SubDocument
spring-service_1  | event===>Document Part
spring-service_1  | event===>Document Part
spring-service_1  | event===>Document Part
spring-service_1  | event===>End SubDocument
spring-service_1  | event===>Document Part
spring-service_1  | event===>Document Part
spring-service_1  | event===>Start SubDocument
spring-service_1  | event===>Document Part
spring-service_1  | event===>Text Unit
spring-service_1  |
spring-service_1  |
spring-service_1  | SEG==>Hp
spring-service_1  | GET CODES==>[]
spring-service_1  | CODED Text ==>Hp
spring-service_1  | JUST TEXT ==>Hp
spring-service_1  |
spring-service_1  |
spring-service_1  | event===>Document Part
spring-service_1  | event===>End SubDocument
spring-service_1  | event===>Document Part
spring-service_1  | event===>Document Part
spring-service_1  | event===>End Document
spring-service_1  | [http-nio-8080-exec-1] DEBUG org.springframework.web.servlet.mvc.method.annotation.RequestResponseBodyMethodProcessor - Using 'application/json', given [*/*] and supported [application/json, application/*+json, application/json, application/*+json]
spring-service_1  | [http-nio-8080-exec-1] DEBUG org.springframework.web.servlet.mvc.method.annotation.RequestResponseBodyMethodProcessor - Writing [Document{filename='Test_4_3cRuLSx.docx', isFileProcessed=true, numberOfWords=16, text={NFDBB2FA9-tu1 (truncated)...]
spring-service_1  | [http-nio-8080-exec-1] DEBUG org.springframework.web.servlet.DispatcherServlet - Completed 200 OK
web_1             | Result = <JsonResponse status_code=200, "application/json"> in DocumentMapToModelAndStoreApi.post
web_1             | document = {'filename': 'Test_4_3cRuLSx.docx', 'numberOfWords': 16, 'text': {'NFDBB2FA9-tu1': [{'original': 'I need to \ue101\ue110complete \ue102\ue111the task by \ue101\ue112today\ue102\ue113.', 'translate': None}, {'original': ' I also need to \ue101\ue110go out \ue102\ue111\ue101\ue112today\ue102\ue113\ue101\ue114.\ue102\ue115', 'translate': None}], 'tu1': [{'original': 'Hp', 'translate': None}]}, 'fileProcessed': True} in DocumentMapToModelAndStoreApi.post
web_1             | filePath = /code/media/Dummy Vendor/Test_4_3cRuLSx.docx in DocumentMapToModelAndStoreApi.post
web_1             | [08/Apr/2021 06:42:58] "POST /api/documentMapToModelAndStoreApi HTTP/1.1" 200 43
web_1             | [08/Apr/2021 06:42:58] "GET /api/tcPagin/4 HTTP/1.1" 301 0
web_1             | [08/Apr/2021 06:42:58] "GET /tcPaginInter/4?page=1 HTTP/1.1" 301 0
web_1             | /usr/local/lib/python3.9/site-packages/rest_framework/pagination.py:200: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <class 'models_only.models.translateContentInterMediate'> QuerySet.
web_1             |   paginator = self.django_paginator_class(queryset, page_size)
web_1             | [08/Apr/2021 06:42:58] "GET /tcPaginInter/4/?page=1 HTTP/1.1" 200 300
web_1             | [08/Apr/2021 06:42:58] "GET /documentPG/4 HTTP/1.1" 200 119
web_1             | src_lan_code = en in LangCodesGet.__call__
web_1             | tar_lan_code = ta in LangCodesGet.__call__
web_1             | [08/Apr/2021 06:42:59] "GET /api/tcPagin/4/ HTTP/1.1" 200 933
web_1             | [08/Apr/2021 06:42:59] "GET /api/cost/4 HTTP/1.1" 200 62
web_1             | [08/Apr/2021 06:42:59] "POST /api/getWorkSpaceOfFile HTTP/1.1" 200 995
web_1             | [08/Apr/2021 06:43:00] "GET /api/tcPagin/4 HTTP/1.1" 301 0
web_1             | [08/Apr/2021 06:43:00] "GET /tcPaginInter/4?page=1 HTTP/1.1" 301 0
web_1             | /usr/local/lib/python3.9/site-packages/rest_framework/pagination.py:200: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <class 'models_only.models.translateContentInterMediate'> QuerySet.
web_1             |   paginator = self.django_paginator_class(queryset, page_size)
web_1             | [08/Apr/2021 06:43:00] "GET /tcPaginInter/4/?page=1 HTTP/1.1" 200 300
web_1             | [08/Apr/2021 06:43:00] "GET /documentPG/4 HTTP/1.1" 200 119
web_1             | src_lan_code = en in LangCodesGet.__call__
web_1             | tar_lan_code = ta in LangCodesGet.__call__
web_1             | [08/Apr/2021 06:43:00] "GET /api/tcPagin/4/ HTTP/1.1" 200 933
web_1             | [08/Apr/2021 06:43:00] "GET /api/cost/4 HTTP/1.1" 200 62
web_1             | [08/Apr/2021 06:43:00] "POST /api/getWorkSpaceOfFile HTTP/1.1" 200 995
web_1             | [08/Apr/2021 06:43:03] "OPTIONS /api/tcPagin/4 HTTP/1.1" 200 0
web_1             | [08/Apr/2021 06:43:03] "GET /api/tcPagin/4 HTTP/1.1" 301 0
web_1             | [08/Apr/2021 06:43:03] "OPTIONS /api/tcPagin/4/ HTTP/1.1" 200 0
web_1             | [08/Apr/2021 06:43:03] "GET /tcPaginInter/4?page=1 HTTP/1.1" 301 0
web_1             | /usr/local/lib/python3.9/site-packages/rest_framework/pagination.py:200: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <class 'models_only.models.translateContentInterMediate'> QuerySet.
web_1             |   paginator = self.django_paginator_class(queryset, page_size)
web_1             | [08/Apr/2021 06:43:03] "GET /tcPaginInter/4/?page=1 HTTP/1.1" 200 300
web_1             | [08/Apr/2021 06:43:03] "GET /documentPG/4 HTTP/1.1" 200 119
web_1             | src_lan_code = en in LangCodesGet.__call__
web_1             | tar_lan_code = ta in LangCodesGet.__call__
web_1             | [08/Apr/2021 06:43:03] "GET /api/tcPagin/4/ HTTP/1.1" 200 933
web_1             | [08/Apr/2021 06:43:49] "POST /api/updateTranslationById HTTP/1.1" 200 46
web_1             | [08/Apr/2021 06:43:50] "POST /api/updateTranslationById HTTP/1.1" 200 46
web_1             | [08/Apr/2021 06:43:50] "POST /api/updateTranslationById HTTP/1.1" 200 47
web_1             | [08/Apr/2021 06:43:52] "GET /document/4 HTTP/1.1" 200 589
spring-service_1  | [http-nio-8080-exec-2] DEBUG org.springframework.web.servlet.DispatcherServlet - POST "/file/testTranslate/", parameters={masked}
spring-service_1  | [http-nio-8080-exec-2] DEBUG org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping - Mapped to com.ailaysa.translate.controller.FileController#testFile(String, String, String, String, HttpSession)
spring-service_1  | {"filename": "/code/media/Dummy Vendor/Test_4_3cRuLSx.docx", "numberOfWords": 19, "text": {"NFDBB2FA9-tu1": [{"original": "I need to \ue101\ue110complete \ue102\ue111the task by \ue101\ue112today\ue102\ue113.", "translate": "\u0ba8\u0bbe\u0ba9\u0bcd \u0b87\u0ba9\u0bcd\u0bb1\u0bc1 \u0bae\u0bc1\u0ba4\u0bb2\u0bcd \u0baa\u0ba3\u0bbf\u0baf\u0bc8 \u0bae\u0bc1\u0b9f\u0bbf\u0b95\u0bcd\u0b95 \u0bb5\u0bc7\u0ba3\u0bcd\u0b9f\u0bc1\u0bae\u0bcd."}, {"original": " I also need to \ue101\ue110go out \ue102\ue111\ue101\ue112today\ue102\ue113\ue101\ue114.\ue102\ue115", "translate": " \u0ba8\u0bbe\u0ba9\u0bcd odtoday\ue102\ue113\ue101\ue114.\ue102\ue115 \u0b90 \u0bb5\u0bc6\u0bb3\u0bbf\u0baf\u0bc7\u0bb1\u0bcd\u0bb1 \u0bb5\u0bc7\u0ba3\u0bcd\u0b9f\u0bc1\u0bae\u0bcd"}], "tu1": [{"original": "Hp", "translate": "\u0bb9\u0bc6\u0b9a\u0bcd.\u0baa\u0bbf."}]}, "fileProcessed": true} A11A4A04D91BE7B05E29B2890FD142AD
spring-service_1  | 1
spring-service_1  | open-xml-processor
spring-service_1  | Document{filename='/code/media/Dummy Vendor/Test_4_3cRuLSx.docx', isFileProcessed=true, numberOfWords=19, text={NFDBB2FA9-tu1=[TextPair{original='I need to complete the task by today.', translate='நான் இன்று முதல் பணியை முடிக்க வேண்டும்.'}, TextPair{original=' I also need to go out today.', translate=' நான் odtoday. ஐ வெளியேற்ற வேண்டும்'}], tu1=[TextPair{original='Hp', translate='ஹெச்.பி.'}]}}
spring-service_1  | /code/media/Dummy Vendor/Test_4_3cRuLSx.out.docx
spring-service_1  | [http-nio-8080-exec-2] DEBUG org.springframework.web.servlet.DispatcherServlet - Failed to complete request: net.sf.okapi.common.resource.InvalidContentException: Markers in coded text: 6. Listed codes: 10. New text="நான் இன்று முதல் பணியை முடிக்க வேண்டும். I also need to go out today."
spring-service_1  | [http-nio-8080-exec-2] ERROR org.apache.catalina.core.ContainerBase.[Tomcat].[localhost].[/].[dispatcherServlet] - Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; nested exception is net.sf.okapi.common.resource.InvalidContentException: Markers in coded text: 6. Listed codes: 10. New text="நான் இன்று முதல் பணியை முடிக்க வேண்டும். I also need to go out today." ] with root cause
spring-service_1  | net.sf.okapi.common.resource.InvalidContentException: Markers in coded text: 6. Listed codes: 10. New text="நான் இன்று முதல் பணியை முடிக்க வேண்டும். I also need to go out today."
spring-service_1  |     at net.sf.okapi.common.resource.TextFragment.setCodedText(TextFragment.java:1264)
spring-service_1  |     at net.sf.okapi.common.resource.TextFragment.setCodedText(TextFragment.java:1175)
spring-service_1  |     at com.ailaysa.translate.processor.impl.mainProcessor.translate(mainProcessor.java:237)
spring-service_1  |     at com.ailaysa.translate.processor.impl.OpenXmlFileProcessor.translate(OpenXmlFileProcessor.java:47)
spring-service_1  |     at com.ailaysa.translate.service.DocumentProcessService.translate(DocumentProcessService.java:82)
spring-service_1  |     at com.ailaysa.translate.service.FileService.translate(FileService.java:56)
spring-service_1  |     at com.ailaysa.translate.controller.FileController.testFile(FileController.java:130)
spring-service_1  |     at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
spring-service_1  |     at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
spring-service_1  |     at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
spring-service_1  |     at java.lang.reflect.Method.invoke(Method.java:498)
spring-service_1  |     at org.springframework.web.method.support.InvocableHandlerMethod.doInvoke(InvocableHandlerMethod.java:190)
spring-service_1  |     at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:138)
spring-service_1  |     at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:106)
spring-service_1  |     at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:888)
spring-service_1  |     at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:793)
spring-service_1  |     at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:87)
spring-service_1  |     at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:1040)
spring-service_1  |     at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:943)
spring-service_1  |     at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:1006)
spring-service_1  |     at org.springframework.web.servlet.FrameworkServlet.doPost(FrameworkServlet.java:909)
spring-service_1  |     at javax.servlet.http.HttpServlet.service(HttpServlet.java:660)
spring-service_1  |     at org.springframework.web.servlet.FrameworkServlet.service(FrameworkServlet.java:883)
spring-service_1  |     at javax.servlet.http.HttpServlet.service(HttpServlet.java:741)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:231)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
spring-service_1  |     at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
spring-service_1  |     at org.springframework.web.filter.RequestContextFilter.doFilterInternal(RequestContextFilter.java:100)
spring-service_1  |     at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
spring-service_1  |     at org.springframework.web.filter.FormContentFilter.doFilterInternal(FormContentFilter.java:93)
spring-service_1  |     at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
spring-service_1  |     at org.springframework.web.filter.CharacterEncodingFilter.doFilterInternal(CharacterEncodingFilter.java:201)
spring-service_1  |     at org.springframework.web.filter.OncePerRequestFilter.doFilter(OncePerRequestFilter.java:119)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:193)
spring-service_1  |     at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:166)
spring-service_1  |     at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:202)
spring-service_1  |     at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:96)
spring-service_1  |     at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:526)
spring-service_1  |     at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:139)
spring-service_1  |     at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:92)
spring-service_1  |     at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:74)
spring-service_1  |     at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:343)
spring-service_1  |     at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:408)
spring-service_1  |     at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:66)
spring-service_1  |     at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:861)
spring-service_1  |     at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1579)
spring-service_1  |     at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:49)
spring-service_1  |     at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
spring-service_1  |     at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
spring-service_1  |     at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
spring-service_1  |     at java.lang.Thread.run(Thread.java:748)
spring-service_1  | [http-nio-8080-exec-2] DEBUG org.springframework.web.servlet.DispatcherServlet - "ERROR" dispatch for POST "/error", parameters={masked}
spring-service_1  | [http-nio-8080-exec-2] DEBUG org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping - Mapped to com.ailaysa.translate.controller.ErrorController#handleError(HttpServletRequest)
spring-service_1  | [http-nio-8080-exec-2] DEBUG org.springframework.web.servlet.view.ContentNegotiatingViewResolver - Selected '*/*' given [*/*]
spring-service_1  | [http-nio-8080-exec-2] DEBUG org.springframework.web.servlet.DispatcherServlet - Exiting from "ERROR" dispatch, status 500
web_1             | {'Status ': '<!DOCTYPE html>\r\n<html>\r\n<body>\r\n<h1>Something went wrong! </h1>\r\n<h2>Our Engineers are on it</h2>\r\n<a href="/">Go Home</a>\r\n</body>\r\n</html>', 'output_file_name': '/code/media/Dummy Vendor/Test_4_3cRuLSx.docx'}
web_1             | [08/Apr/2021 06:43:53] "POST /api/translatedContentToFile HTTP/1.1" 200 240
web_1             | [08/Apr/2021 06:45:50] "GET /document/4 HTTP/1.1" 200 589
web_1             | [08/Apr/2021 06:46:00] "GET /document/4 HTTP/1.1" 200 589
```
</br>
# In another way
Also, I have tried using obtained segments directly instead of using coded text from the segmentation. And the flow of that follows as below,

```
// Obtain segmented sentences one by one
	for (Segment seg : segs) {
		String segmented_text = seg.toString();

		TextPair textPair = new TextPair();
		textPair.setOriginal(segmented_text);
		List<TextPair> list = textMap.containsKey(id) ? textMap.get(id) : new ArrayList<>();
		list.add(textPair);
		textMap.put(id, list);
	}
```

## Output Obtained in the Postman API Browser as follows,
![2021-04-08 12_14_46-Postman](https://user-images.githubusercontent.com/15103613/113993342-01462500-9872-11eb-87fb-6743d7c6a7e9.png)

And the same data were received in the workspace window,
![2021-04-08 09_48_39-Workspace Editor](https://user-images.githubusercontent.com/15103613/113994871-5b93b580-9873-11eb-9101-feb870bbd638.png)

which then gets saved in the database table as well but when downloading the saved file, there is no translation were written on the target file (downloaded translated file is same as the source file).

# References
Please find the code base of source files from the below GitHub Links.

## Spring Boot codebase
Please find the [GitHub link to the Java Spring Boot code base](https://github.com/Ailaysa/spring_ailaysa) where the Okapi classes had been used for our implementation. Please clone **_sridhar_dev_** branch.

## Django codebase
Please find the [GitHub link to the Python - Django code base](https://github.com/Ailaysa/dj_ailaysa) where the result of Okapi implementations are received as Json response. Please clone **_sridhar_dev_** branch.

## Quick Start for Guidance Requirement
We ensure the tools provided by Okapi Framework for Text Filters, Sentence Segmentation, Text Extraction, Translation using Google Machine Translation Engine (v2, paid services), Translated Content Merging and Translated File Output.

1.	We’d like to let our End-users to do translations sentence-wise segmented text instead of Hugh Paragraphs. And the method we have tried are as follows.
*  [Using IFilter](https://okapiframework.org/javadoc/net/sf/okapi/common/filters/IFilter.html)
*  [Reading Document using Okapi Framework](https://okapiframework.org/devguide/gettingstarted.html#readingDocument)
*  [Performing Segmentation](https://okapiframework.org/devguide/segmentation.html#performingSegmentation)
*  [Okapi Inline Codes](https://okapiframework.org/devguide/gettingstarted.html#textUnits)

2.	Also we would like to know whether we have any other possibilities to achieve this sentence-wise segmentation using other methods rather using Okapi Framework.

### Version Details
Okapi: 1.41.0</br>
Ratel: 1.41.0</br>
File Filters: 1.41.0 [PlainTextFilter]</br>
JDK: 15.0.2</br>
Python: 3.9.2</br>
Django: 3.0.9</br>
