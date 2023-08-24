# Introduction # {#introduction}

*This section is non-normative.*

The [PROTOCOL] defines a minimal HTTP-based framework by which clients can securely receive update notifications directly from any resource of interest on the Open Web Platform.

## Motivation ## {#motivation}

*This section is non-normative.*

HTTP was originally designed to transfer a static documents within a single request and response. If the document changes, HTTP does not automatically update clients with the new versions. This design was adequate for web pages that were mostly static and written by hand.

But web-applications today are dynamic. They provide (near-)instantaneous updates across multiple clients and servers. The many workarounds developed over the years to provide real-time updates over HTTP have often proven to be inadequate. Web programmers instead resort to implementing custom messaging systems over alternate protocols such as WebSockets, which requires additional layers of code in the form of non-standard JavaScript frameworks to synchronize changes of state.

Per Resource Events is a minimal protocol built on top of HTTP that allows clients to receive notifications directly from a resource of interest. Unlike other HTTP based solutions, [PROTOCOL] supports the use of arbitrary media-types for notifications, which can be negotiated just like representations; thus giving implementers a lot of flexibility to customize notifications according to the needs of their application.

## How it Works ## {#how-it-works}

*This section is non-normative.*

A client application that wishes to receive notifications about updates to a resource simply places a `GET` request on that resource with just one additional `Accept-Events` header. The server responds by sending, first the current representation of the resource (though a client can request for this to be skipped), followed by notifications sent in response to resource events as parts of a multipart response while the request is open.

If a server does not implement the [PROTOCOL], the `Accept Events` header in a `GET` request is simply ignored. The resource returns the current representation thereby preserving backwards compatibility.

## Scope ## {#scope}

*This section is non-normative.*

The [PROTOCOL] specifies:

+ a mechanism for the discovery of notification capabilities per resource,
+ a mechanism to request notifications from a resource,
+ representation and semantics for the response that transmits notifications (but not the representation and semantics for notifications themselves, see below),

<div class="v-space"></div>

The [PROTOCOL] does not specify:

+ a specific authentication and authorization mechanism to be used with the [PROTOCOL]. Implementations are encouraged to use existing approaches for authentication and authorization.
+ representation and semantics for notifications.
  + Implementations are free to use any media-type for notifications, which can be negotiated just like the content-type for the representation of the state of the resource.
  + Implementations are also free to define additional semantics for a given media-type when used to transmit notifications using the [PROTOCOL]. As an appendix to this specification, we define additional semantics for the `message/rfc822` MIME type when used for [PREP] notifications.

## Audience ## {#intended-audience}

*This section is non-normative.*

This specification is for:

+ [Server developers](http://data.europa.eu/esco/occupation/a7c1d23d-aeca-4bee-9a08-5993ed98b135) who wish to enable clients to listen for updates to particular resources.
+ [Application developers](http://data.europa.eu/esco/occupation/c40a2919-48a9-40ea-b506-1f34f693496d) who wish to implement a client to listen for updates to particular resources.
