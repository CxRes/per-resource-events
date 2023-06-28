## Classes of Products ## {#classes-of-products}

The [PROTOCOL] identifies the following Classes of Products for conforming implementations. These products are referenced throughout this specification.

<dl>

  : <dfn>Resource Server</dfn>
  :: A *resource server* that builds on HTTP Origin Server ([[!RFC9110]] [[RFC9110#section-3.6|§ 3.6 Origin Server]]) by defining header fields ([[!RFC9110]] [[RFC9110#section-6.3|§ 6.3 Header Fields]]) and media types ([[!RFC9110]] [[RFC9110#section-8.3.1|§ 8.3.1 Media Types]]).

  : <dfn>Application Client</dfn>
  :: An *application client* that builds on HTTP User Agent ([[!RFC9110]] [[RFC9110#section-3.5|§ 3.5 User Agents]]) by defining behaviour in terms of fetching [[!FETCH]] across the platform.

</dl>

## Interoperability ## {#interoperability}

Interoperability occurs between [Application Client—Resource Server](#application-client—resource-server-interoperability) as defined by the [PROTOCOL].

<dl>

  : <dfn id="application-client—resource-server-interoperability">Application Client—Resource Server Interoperability</dfn>
  :: Interoperability of implementations for [=application clients=] and [=resource servers=] is tested by evaluating an implementation’s ability to request and respond to HTTP messages that conform to the [PROTOCOL].

</dl>
