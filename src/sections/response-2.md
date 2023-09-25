# Notifications Response # {#notifications-response}

<!--

In response to a `GET` request with a [:Accept-Events:] header field with `PREP` as the preferred notifications protocol, a [=resource server=] providing notifications:

+ MUST respond with a status code identical to the one that would have been sent with the response had notifications not been requested.
+ MUST include the message body that would have been transmitted had notifications not been requested, unless the `Prefer` header field [[!RFC7240]] indicates a preference of `return=minimal` ([[!RFC7240]] [[RFC7240#section-4.2|§ 4.2 The "return=representation" and "return=minimal" Preferences]]).

-->

## Common Headers ## {#notifications-response-common-headers}

A [=resource server=] providing notifications using the [PROTOCOL]:

+ MUST include the following header fields in the response:

  + `Date` ([[!RFC9110]] [[RFC9110#section-6.6.1|§ 6.6.1 Date]]).
  + `Content-Type` ([[!RFC9110]] [[RFC9110#section-8.3|§ 8.3 Content-Type]]) with the media type set to `multipart` ([[!RFC9110]] [[RFC9110#section-8.3.3|§ 8.3.3 Multipart Types]]). The subtype will depend on whether the base response is included in the response body.
  + [:Events:] which MUST include the following [=event fields=]:
    + `protocol` set to the value `PREP`.
    + `status` set to the `200 (OK)` ([[!RFC9110]] [[RFC9110#section-15.3.1|§ 15.3.1 200 OK]]) status code.
    + `expires` with an integer value in seconds indicating an interval after `Date` when notifications will no longer be sent and the response stream is closed.

    NOTE: Since caching is meaningless in the context of notifications, this specifications repurposes the deprecated `Expires` header field to specify when the notifications response ends.

  <div class="v-space"></div>

+ SHOULD include the following header fields in the response:

  + `Vary` ([[!RFC9110]] [[RFC9110#section-12.5.5|§ 12.5.5 Vary]]) which MUST include [:Accept-Events:] as one of the values.
  + `Last-Modified` ([[!RFC9110]] [[RFC9110#section-8.8.2|§ 8.8.2 Last-Modified]]).
  + `ETag` ([[!RFC9110]] [[RFC9110#section-8.8.3|§ 8.8.3 ETag]]).

  <div class="v-space"></div>

+ MAY include the following header fields in the response:

  + [:Accept-Events:] which MUST list `PREP` as one of the available notification protocols. Associated with the `PREP` list item, the [=resource server=]:
    + MUST include an `accept` [=event field=] with at least one acceptable media-type for notifications.

## Only Notifications ## {#only-notifications-response}

An [=application client=] requesting only notifications using the [PROTOCOL] needs to explicitly opt out of receiving the [=base response=] as described in [[#request]].

A [=resource server=] MUST NOT send the [[=base response=]], if the request for [PREP] notifications includes the `Last-Event-ID` header field([[!EVENTSOURCE]] [[EVENTSOURCE#section-9.2.4|§ 9.2.4 The `Last-Event-ID` header]]) which matches the `Event-ID` of the last event on the resource.

### Headers ### {#only-notifications-response-headers}

A [=resource server=] that provides only notifications using the [PROTOCOL] MUST include the `Vary` header field
([[!RFC9110]] [[RFC9110#section-12.5.5|§ 12.5.5 Vary]]) with `Last-Event-ID` as one of the values.

### Body ### {#only-notifications-response-body}

A [=resource server=] MUST transmit [PREP] notifications as a multipart message body ([[!RFC2046]] [[RFC2046#section-5.1|§ 5.1 Multipart Media Type]]), with a media type of `multipart/digest` ([[!RFC2046]] [[RFC2046#section-5.1.5|§ 5.1.5 Digest Subtype]]) that MAY include zero or more body parts. Each body part of the multipart message body MAY contain at most one notification.

<div class="example">
  <span class="marker">Response with Notifications Only</span>
  <prep-http-tabs>
    <div slot="http">
      <pre class="include-code">
        path: examples/notifications/basic-response.http
        highlight: http
      </pre>
    </div>
    <div slot="http2">
      <pre class="include-code">
        path: examples/notifications/basic-response.h2
        highlight: toml
      </pre>
    </div>
  </prep-http-tabs>
</div>

<div class="advisement">
  <div class="marker">Implementation Guidance</div>

  While not strictly necessary, it is strongly encouraged that [=resource servers=] send notifications in a manner such that the boundary delimiter ([[!RFC2046]] [[RFC2046#section-5.1.2|§ 5.1 Multipart Media Type > § 5.1.2 Common Syntax]]) is always at the end of a chunk ([[!RFC9112]] [[RFC9112#section-7.1|§ 7.1 Chunked Transfer Coding]]) or data frame ([[!RFC9113]] [[RFC9113#section-6.1|§ 6 Frame Definitions > § 6.1 DATA]]) as shown in the example above. This way an [=application  client=] does not have to wait until the next chunk or frame (which might be a while or in cases of error never arrive) to be certain that the immediate message is complete.

</div>

## Composite Response ## {#composite-response}

By default, the [PROTOCOL] requires a [=resource server=] to transmit the [=base response=] body before notifications. An [=application client=] MAY opt out of this behaviour as described in [[#request]].

NOTE: Not only does this behaviour ensure interoperability, it is also desirable in most scenarios. It short-circuits an extra round trip that would be otherwise needed to fetch the current representation before notifications and eliminates the need to co-ordinate the two responses.

### Headers ### {#composite-response-headers}

If the request for [PREP] notifications includes the `Last-Event-ID` header field, a [=resource server=] MUST include `Vary` header field
([[!RFC9110]] [[RFC9110#section-12.5.5|§ 12.5.5 Vary]]) with `last-event-id` as one of the values.

### Body ### {#composite-response-body}

Where the response includes a [=base response=] body prior to [PREP] notifications, a [=resource server=] MUST transmit a multipart message body ([[!RFC2046]] [[RFC2046#section-5.1|§ 5.1 Multipart Media Type]]) with a media type of `multipart/mixed` ([[!RFC2046]] [[RFC2046#section-5.1.3|§ 5.1.3 Mixed Subtype]]) with two body parts in the order specified below:

1. the message body that would have been sent had notifications not been requested.
2. the multipart response body with body parts containing notifications as defined in [[#only-notifications-response]].

<div class="example">
  <span class="marker">Full Response</span>
  <prep-http-tabs>
    <div slot="http">
      <pre class="include-code">
        path: examples/notifications/full-response.http
        highlight: http
      </pre>
    </div>
    <div slot="http2">
      <pre class="include-code">
        path: examples/notifications/full-response.h2
        highlight: toml
      </pre>
    </div>
  </prep-http-tabs>
</div>

## Termination ## {#notifications-response-termination}

A [=resource server=] MUST end the notification response in any one of the following scenarios:

+ Once the time specified in the `expires` parameter of the `Events` header field has elapsed.
+ Immediately after sending a notification upon a `DELETE` request on the resource that results in a response with 200 (OK) ([[!RFC9110]] [[RFC9110#section-15.3.1|§ 15.3.1 200 OK]]) or 204 (No Content) ([[!RFC9110]] [[RFC9110#section-15.3.5|§ 15.3.5 204 No Content]]) status codes.

A [=resource server=] MUST properly terminate the multipart response as defined in [[!RFC2046]] [[RFC2046#section-5.1.2|§ 5.1 Multipart Media Type > § 5.1.2 Common Syntax]], before closing the notification response stream.

<div class="advisement">
  <div class="marker">Implementation Guidance</div>

  When a user navigates away from a website or an application using PREP notifications, [=application clients=] are strongly encouraged to properly close the response stream to ensure that servers do not keep sending notifications.

</div>
