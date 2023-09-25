# Request # {#request}

The [PROTOCOL] extends the content negotiation mechanism provided by HTTP allowing [=application clients=] to negotiate notifications independent of the [=base response=].

In order to receive notifications using the [PROTOCOL] from a resource, an [=application client=] sends a `GET` request to [=resource server=], which:

+ MUST include the [:Accept-Events:] header field, which:
  + MUST list `PREP` as a preferred notification protocol.
    + MAY include zero or more [=event fields=]. For example, [=application clients=] MAY specify an `accept` [=event field=] to indicate a preferred media-type for notifications.
+ MAY include the `Last-Event-ID` header field ([[!EVENTSOURCE]] [[EVENTSOURCE#section-9.2.4|§ 9.2.4  The `Last-Event-ID` header]]) requesting the [=resource server=] to not send the [=base response=] body and resume notifications from the event occurring immediately after the one specified.

<div class="advisement">
  <div class="marker">Implementation Guidance</div>

  Set the `Last-Event-ID` to a wildcard `*` to explicitly request a [=resource server=] to not send the [=base response=] body in the response at all.

</div>

<div class="v-space"></div>

<div class="example">
  <span class="marker">Request for Notifications</span>
  <prep-http-tabs>
    <div slot="http">
      <pre class="include-code">
        path: examples/notifications/request.http
        highlight: http
      </pre>
    </div>
    <div slot="http2">
      <pre class="include-code">
        path: examples/notifications/request.h2
        highlight: toml
      </pre>
    </div>
  </prep-http-tabs>
</div>

A [=resource server=] MUST ignore [=event fields=] for `PREP` notifications in the [:Accept-Events:] header that it does not recognize or implement.
