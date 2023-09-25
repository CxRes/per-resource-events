# Protocol # {#protocol}

## Event Fields ## {#event-fields}

The [PROTOCOL] reuses existing HTTP fields ([[!RFC9110]] [[RFC9110#section-5|§ 5 Fields]]) as event fields. Any HTTP field MAY be used as an [=event field=]. For the limited context of notifications using the [PROTOCOL], an [=event field=] with the same name as an HTTP field MUST have identical semantics to that HTTP field, unless otherwise specified.

This specification restricts [:Accept-Events:] and [:Events:] as Structured header fields [[!STRUCTURED-FIELDS]]. From this it follows:

+ An event field whose value is anything except an item without parameters MUST be specified in an Inner List ([[!STRUCTURED-FIELDS]] [[STRUCTURED-FIELDS#section-3.1.1|§ 3.1.1 Inner List]]).
+ Unless otherwise specified, an [=event field=] that is not already defined as a Structured Field, therefore, MUST be handled as a Retrofit Structured Field [[!RETROFIT-FIELDS]] when such handling is defined.
+ An [=event field=] that is not already defined as a Structured Field but cannot be handled as a Retrofit Structured Field either, MUST be explicitly specified by the implementation.

## Methods ## {#methods}

For the [PROTOCOL], `HEAD` ([[!RFC9110]] [[RFC9110#section-9.3.2|§ 9.3.2 HEAD]]) and `GET` ([[!RFC9110]] [[RFC9110#section-9.3.1|§ 9.3.1 GET]]) are the only methods in response to which notifications are advertised.

A [=resource server=] MUST NOT send the [:Accept-Events:] header field with `PREP` as a protocol in response to a request with any method other than `HEAD` or `GET`.

For the [PROTOCOL], `GET` ([[!RFC9110]] [[RFC9110#section-9.3.1|§ 9.3.1 GET]]) is the only method by which notifications are requested and for which notifications response is defined.

An [=application client=] MUST NOT send the [:Accept-Events:] header field with `PREP` as a protocol in a request with any method other than `GET`.

An [=resource server=] MUST NOT send the [:Events:] header field with the parameter `protocol` with a value of `PREP` in response to a request with any method other than `GET`.

A [=resource server=] MUST NOT send the [:Events:] header field except in response to a `GET` request.

## Status Codes ## {#status-codes}

The [PROTOCOL] reuses existing HTTP status codes ([[!RFC9110]] [[RFC9110#section-15|§ 15 Status Codes]]) to describe the result of the request for notifications and the semantics of notifications in the response.

In response to a request where [:Accept-Events:] header field indicates `PREP` as the preferred protocol, a [=resource server=] that supports notifications using the [PROTOCOL] MUST communicate the status code for the notifications response using the `status` parameter in the [:Events:] header field.

For the limited context of notifications using the [PROTOCOL], the status code communicated using the `status` parameter in the [:Events:] header field MUST have identical semantics to the corresponding HTTP status code, unless otherwise specified.
