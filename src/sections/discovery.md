# Discovery # {#notifications-discovery}

[=Application clients=] can engage in reactive content negotiation to discover if a [=resource server=] supports notifications using the [PROTOCOL] on a given resource.

## Request ## {#discovery-request}

An [=application client=] can discover the ability of a [=resource server=] to deliver [PREP] notifications for a target resource by sending a `HEAD` ([[!RFC9110]] [[RFC9110#section-9.3.2|§ 9.3.2 HEAD]]) request.

## Not Available ## {#discovery-response-when-notifications-not-available}

In the response to a `HEAD` request, a [=resource server=] that does not provide notifications for the target resource using the [PROTOCOL] MUST NOT list `PREP` as as one of the available protocols in the [:Accept-Events:] header field.

## Available ## {#discovery-response-when-notifications-available}

In response to a `HEAD` request, a [=resource server=] that serves notifications for the target resource using the [PROTOCOL] SHOULD include the [:Accept-Events:] header field, which MUST list `PREP` as one of the available protocols.

Associated with `PREP` list item, the [=resource server=] MUST include an `accept` [=event field=] with at least one acceptable media-type for notifications.

<div class="example">
  <span class="marker">Notifications Discovery</span>
  <br/>
  <cite>Discovery request:</cite>
  <prep-http-tabs>
    <div slot="http">
      <pre class="include-code">
        path: examples/discovery/request.http
        highlight: http
      </pre>
    </div>
    <div slot="http2">
      <pre class="include-code">
        path: examples/discovery/request.h2
        highlight: toml
      </pre>
    </div>
  </prep-http-tabs>
  <br/>
  <cite>Discovery response:</cite>
  <br/>
  <prep-http-tabs>
    <div slot="http">
      <pre class="include-code">
        path: examples/discovery/response.http
        highlight: http
      </pre>
    </div>
    <div slot="http2">
      <pre class="include-code">
        path: examples/discovery/response.h2
        highlight: toml
      </pre>
    </div>
  </prep-http-tabs>
</div>

<div class="advisement">
  <div class="marker">Implementation Guidance</div>

  When [=resource servers=] support HTTP/2 [[RFC9113]] for a resource, they are strongly encouraged to advertise it in the response.

  When a resource is accessible from different locations, [=resource servers=] are encouraged to advertise these locations in the response.

  HTTP Alternative Services [[RFC7838]], for example, describes a mechanism for advertising these capabilities.

</div>
