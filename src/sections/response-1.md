<!--

# Process

1. Calculate Base Response R
2. Does the resource provide notifications?
   No -> return R
3. Does the request have an [:Accept-Events:] header?
   No -> return R
4. Can the [:Accept-Events:] header be Parsed?
   No -> return R
5. Loop: Can you identify a preferred protocol?
   No -> return R
6. Do you support the Preferred protocol?
   No -> Reject Preference; GOTO 5
7. Can you serve notifications with that protocol?
   No -> Set Events header with protocol and any failed status; GOTO 5
8. Serve with protocol

Step 7. For PREP Protocol
0. Does the response R allow for Notifications
   No -> Failed Status: 412 (Precondition Failed)
1. Does the RS understand the event fields
   No -> Failed Status: 400
2. Can RS serve notifications consistent with event fields
   No -> Failed Status: 403, 406
3. Calculate notifications N headers
4. Does `last-event-id` exist and match?
   No -> send R headers + N headers + R body
   Yes -> send R headers + N headers
6. Loop: Has time elapsed?
   Yes -> GOTO 10
7. Has there been an event?
   No -> GOTO 6
8. Send Notifications;
9. Is the event Delete;
   No -> GOTO 6
10. Close Stream

-->

# Response without Notifications # {#response-without-notifications}

## Not Available ## {#response-when-notifications-not-available}

A [=resource server=] not implementing the [PROTOCOL] (or not supporting it for the target resource) can ignore a request for notifications and respond as if it received a normal request on that resource without impacting interoperability.

A [=resource server=] that does not provide notifications using the [PROTOCOL] MUST NOT:

+ list `PREP` as as one of the available protocols in the [:Accept-Events:] header field, if sent in the response.
+ send the [:Events:] header field in the response with the `protocol` event field set to `PREP`.

<div class="advisement">
  <div class="marker">Implementation Guidance</div>

  Implementations are advised against sending notifications for long-lived resources. A resource might be considered long-lived, if the [=resource server=] determines that the resource is unlikely to change in the duration of the notification response. In such instances, [=resource servers=] are strongly advised to respond with the `Cache-Control` header and set the `max-age` parameter in it.

  </div>
</div>

## Error Response ## {#error-response}

A [=resource server=] MUST NOT include [PREP] notifications in a response, unless request results in one of the following status codes:

+ `200 (OK)` ([[!RFC9110]] [[RFC9110#section-15.3.1|§ 15.3.1 200 OK]]),
+ `204 (No Content)` ([[!RFC9110]] [[RFC9110#section-15.3.5|§ 15.3.5 204 No Content]]),
+ `206 (Partial Content)` ([[!RFC9110]] [[RFC9110#section-15.3.7|§ 15.3.7 206 Partial Content]]),
+ `226 (IM Used)` ([[!RFC3229]] [[RFC3229#section-10.4.1|§ 10.4.1 226 IM Used]]).

A [=resource server=] that does not serve [PREP] notifications, on account of the response not having one of the above-mentioned status codes:

+ SHOULD include the [:Events:] header fields where:
  + the `protocol` [=event field=] MUST be set to a value of `PREP`,
  + the `status` [=event field=] MUST be set to `412 (Precondition Failed)` ([[!RFC9110]] [[RFC9110#section-15.5.13|§ 15.5.13 412 Precondition Failed]]) status code.

## Notification Errors ## {#response-with-notification-errors}

A [=resource server=] might still not be able to send notifications using the [PROTOCOL] requested by an [=application client=] despite a valid response.

A [=resource server=] unable to serve [PREP] notifications, even when the request results in a status code mentioned in [[#error-response]]:

+ SHOULD include the [:Events:] header fields in the response where:
  + the `protocol` [=event field=] MUST be set to a value of `PREP`,
  + the `status` [=event field=] MUST be set to appropriate  status code indicating the reason for not serving notifications.
