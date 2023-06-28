# Introduction # {#introduction}

*This section is non-normative.*

The [PROTOCOL] defines a minimal HTTP-based framework by which clients can securely receive update notifications directly from any resource of interest on the Open Web Platform.

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
