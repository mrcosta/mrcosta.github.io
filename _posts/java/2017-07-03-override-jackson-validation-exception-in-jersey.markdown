---
layout: post
title: "How to Override Jersey's Jackson Exceptions"
date: 2017-07-03 12:00:00 
comments: true
description: " These days I was working with Jetty (*version 2.19*) to build a Restful API and it's come in hand with Jackson. What it means that some basic JSON validations will happen automagically. Nice, right? Hum...Let'see."
categories:
- java, api, rest, parser
permalink: sample-post
---
These days I was working with Jetty (*version 2.19*) to build a Restful API and it's come in hand with Jackson. What it means that some basic JSON validations will happen automagically. Nice, right? Hum...Let'see.

Consider a post endpoint where is necessary to send a *startTime* in a date format like *yyyy-MM-dd'T'HH:mm:ss* and you send something in the wrong format, like below:

```json
{
  "description": "string ",
  "type": "RUNNING",
  "startTime": "21"
}
```

And the message that you receive back is something like:

```json
Failed to parse Date value '21' (format: "yyyy-MM-dd'T'HH:mm:ss"): Unparseable date: "21" (through reference chain: de.egym.recruiting.codingtask.domain.Exercise["startTime"])
```

Or Worst, for another error:

```json
Unexpected character ('t' (code 116)): was expecting double-quote to start field name
 at [Source: org.glassfish.jersey.message.internal.ReaderInterceptorExecutor$UnCloseableInputStream@8c0a6b1; line: 4, column: 4]
 ```

Ok, the message is understandble, and I think almost any developer would be capable to came through the problem. But when thinking about robust, clean and understandable APIs, is good to give more information and a more structured response for errors for who is using (imagine the scenario where you could validate all the fields from body at once). 

# Solution

After some research I discover that was just a matter to implement the *ExceptionMapper* interface.

```java
@Provider
public class JsonMappingExceptionResponse implements ExceptionMapper<JsonMappingException> {

    @Override
    public Response toResponse(JsonMappingException exception) {
        return errorResponse(Response.Status.BAD_REQUEST.getStatusCode(), exception.getOriginalMessage());
    }
}
```

Where basically I was including the body to be customized, adding just one part from the exception message that I was receiving.

Restart the server (I was using Jetty) and didn't work =/  
Again, after some research I discovered that was necessary to exclude one specific provider from my *web.xml* related to Jackson to override the exception that I wanted:

```xml
<init-param>
	<param-name>jersey.config.server.provider.classnames</param-name>
		<param-value>
			com.fasterxml.jackson.jaxrs.json.JacksonJaxbJsonProvider
		</param-value>
</init-param>
```

After this I got my new Jackson's error messages working:

```json
 {
  "errorCode": 400,
  "errorValue": "Failed to parse Date value '21' (format: \"yyyy-MM-dd'T'HH:mm:ss\"): Unparseable date: \"21\""
}
```

The message is clearer than before and hides unnecessary details, like the object that holds *startTime* property. I hope you enjoy =) 
