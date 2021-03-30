# Okapi_Issue

## Issue persisting in the implementation of Okapi Framework
As of now, we are trying to take input sources as files (16 file formats on development process). 
* Firstly, We'd like to take input source from the end-user as a simple text file _(*.txt)_ and extract the translatable text contents sentence-wise (text unit wise).
* Secondly, setting up Machine Translation Engines for Translation Process where each segmented sentences will be translated. 
* Finally, to finish up the process by merging back to the same file format.</br>

In the current process, the problems persists on extracting translatable text content from the input text file. This process is known as Segmentation ( aka tokenizing in technical term). Each segments are splited on paragraph-wise instead of sentence-wise.

For Better understanding, kindly go through the following steps or simply try on your way of testing. Please don't hesitate to raise an opition to understand more regarding this workflow or to point out if you've any idea to resolve this issue.

## Steps
### Sample Input text file "Test 4.docx"
[Please download the Raw Document](https://github.com/Ailaysa/dj_ailaysa/blob/dev_txt/Input_Samples/Test%204.docx)

## Current Approach
1.	Using Okapi Framework, Set of File filters are being used in the implementation for File Formatting to extract translatable text from the input text source (e.g. Sample.txt, sample.docx. sample.html, etc). 
	A Simple ".docx" format is chosen for the implementing basic process now.
2.	Extracted translatable content from the files using File filter (OpenXMLFilter) is about to segment the text as sentences. </br>
    For example, When the document consists of set of paragraphs, each paragraph would consist number of sentences. Here, in the extraction process, we need the segmented on sentence-wise in order to reduce the complexity on the workspace.
3.	While working with Rainbow tool, it seems, it has been designed to use SRX Segmentary [SRXSegmenter] for segmenting each sentence from the input source/document (i.e. file content). Segmentation rules is required for SRXSegmenter. So, we have taken a Default SRX Rules from the Rainbow tool for sentence-wise segmentation (which is tested using Ratel tool as well - by applying it's regex patterns) from Okapi framework. 
4.	The results are verified in the Ratel tool by the given Regex pattern. An SRX file is hence created post verification.

## Segmentation Rules
Here is the retrived [Default SRX file](https://github.com/Ailaysa/dj_ailaysa/blob/dev_txt/SegmentationRules/okapi_default_icu4j.srx) from Okapi Framework. 


## Spring Boot codebase
Please find the [GitHub link to the Java Spring Boot code base](https://github.com/Ailaysa/spring_ailaysa) where the Okapi classes had been used for our implementation.

## Django codebase
Please find the [GitHub link to the Python - Django code base](https://github.com/Ailaysa/dj_ailaysa) where the result of Okapi implementations are received as Json response.

## The Result we are getting while running mainProcessor.java
![Wokspace_Capture](https://user-images.githubusercontent.com/15103613/112992601-eeeb2d80-9185-11eb-8126-5178bb0d2edc.PNG)


From the above process, we are about to take the Text content from the input file (i.e. source2.txt) are being extracted by Sentences, but the issue is that we are unaware of any methods for processing text which is in different formats.
For example, if a MS Office Word file consists, different number of formats for text, like formatting the text with bold, italic, underlined, aligned places, highlights, text colors, and so on.</br></br>
Hence, we are unable to process and show it up the complete sentence on the workspace.</br></br>
As of now, what we did is, we can able to make the sentence-wise segmentation in Plain text format.
This case actually passes only if there is no formattings appended in **DOCX, XLSX, PPTX**, but completely fails in the cases of using formats in **_between_** or **_in_** the sentences of **DOCX, XLSX, PPTX** file inputs.

### Ratel Tool from Okapi Framework
But on the other hand, when we’re testing the same file using Ratel tool, segmentation process happens as follows (using the same SRX Rules using "SegmentationRules.srx").

![Sentence_wise_Segmented_using_Ratel](https://user-images.githubusercontent.com/15103613/112991113-68821c00-9184-11eb-95e7-711c9e2f8b0f.PNG)


## Quick Start for Guidance Requirement
We ensure the tools provided by Okapi Framework for Text Filters, Sentence Segmentation, Text Extraction, Translation using Google Machine Translation Engine (v2, paid services), Translated Content Merging and Translated File Output.

1.	We’d like to let our End-users to do translations sentence-wise segmented text instead of Hugh Paragraphs. And the method we have tried are as follows.
*  [Using IFilter](https://okapiframework.org/javadoc/net/sf/okapi/common/filters/IFilter.html)
*  [Reading Document using Okapi Framework](https://okapiframework.org/devguide/gettingstarted.html#readingDocument)
*  [Performing Segmentation](https://okapiframework.org/devguide/segmentation.html#performingSegmentation)

2.	Also we would like to know whether we have any other possibilities to achieve this sentence-wise segmentation using other methods rather using Okapi Framework.

### Version Details
Okapi: 1.41.0</br>
Ratel: 1.41.0</br>
File Filters: 1.41.0 [PlainTextFilter]</br>
JDK: 15.0.2</br>
Python: 3.9.2</br>
Django: 3.0.9</br>
