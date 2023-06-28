# Header Fields # {#header-fields}

The [PROTOCOL] introduces new header fields. These header fields are not specific to the [PROTOCOL]. They can be used by other protocols that augment resources with the ability to send notifications.

Protocols MUST ensure that the semantics be so defined that these header fields are safely ignored by recipients that do not recognize them ([[!RFC9110]] [[RFC9110#section-5.1|§ 5.1 Field Names]]).


<h3 id="accept-events-header" data-dfn-type="http-header">
  Accept-Events
</h3>

The [:Accept-Events:] header field can be used by [=application clients=] to specify their preferred protocol for receiving notifications. For example, the [:Accept-Events:] header field can be used to indicate that the request is specifically limited to a small set of desired protocols, as in the case for notifications with the [PROTOCOL].

When sent by a [=resource server=] in a response, the [:Accept-Events:] header field provides information about which notification protocols are preferred in a subsequent request to the same resource.

An [=application client=] MAY generate a request for notifications regardless of having received an [:Accept-Events:] header field. The information only provides advice for the sake of improving performance and reducing unnecessary network transfers.

Conversely, an [=application client=] MUST NOT assume that receiving an [:Accept-Events:] header field means that future requests will return notifications. The content might change, the server might only support notifications requests at certain times or under certain conditions, or a different intermediary might process the next request.

### Validity ### {#accept-events-header-validity}

A recipient MUST ignore the [:Accept-Events:] header field received with a request method that is unrecognized or for which notifications response is not defined for a particular notifications protocol.

A recipient MUST ignore the [:Accept-Events:] header field received in response to a request method that is unrecognized or for which notifications discovery and/or response is not defined for a particular notifications protocol.

A recipient MUST ignore the [:Accept-Events:] header field that it does not understand.

<div class="issue">

Where a recipient receives an illegal [:Accept-Events:] header field, can we:

+ Ignore it (as we do now)
+ Send an error message. If so what could be the form of the error message?

</div>

A recipient MUST ignore the protocols specified in the [:Accept-Events:] header field that they are not aware of.

### Syntax ### {#accept-events-header-syntax}

[:Accept-Events:] is a List structured ([[!STRUCTURED-FIELDS]] [[RFC8941#section-3.1|§ 3.1 Lists]]) header field. Its members MUST be of type string that identifies a notification protocol. A protocol identifier MAY be followed with zero or more parameters defined by the given protocol, which MAY be followed by a [:q:] parameter.

ISSUE: Could the protocol identifier be a URL?

ISSUE: If the protocol is an identifier, do we need to define a registry?

The <dfn data-dfn-type="http-header">`q`</dfn> parameter assigns a relative "weight" to the sender's preference for a notification protocol with semantics as defined in [[!RFC9110]] [[RFC9110#section-12.4.2|§ 12.4.2 Quality Values]]. Senders using weights SHOULD send [:q:] last (after all protocol parameters). Recipients SHOULD process any parameter named [:q:] as weight, regardless of parameter ordering.

Note: Use of the [:q:] parameter name to negotiate notification protocols would interfere with any parameter having the same name. Hence, protocol parameters named [:q:] are disallowed.


<h3 id="events-header" data-dfn-type="http-header">
  Events
</h3>

The [:Events:] header field is sent by a [=resource server=] to provide event fields in response to a request for notifications.

Where the [:Accept-Events:] header field sent in the request is ignored, a [=resource server=] MUST NOT send the [:Events:] header field in a response.

Conversely, a [=resource server=] MUST send the [:Events:] header field in a response, if the [:Accept-Events:] header field sent in the request is not ignored.

### Validity ### {#events-header-validity}

If the [:Events:] header field is sent in response to a request that does not contain the [:Accept-Events:] header field, the recipient MUST treat the response as invalid.

ISSUE: Does compliance with HTTP demand that we ignore the [:Events:] header? How will recipient process such a response that presumably contains notifications that it has not requested?

If the response contains a [:Events:] header field that the recipient does not understand or the [:Events:] header field specifies a `protocol` that the recipient does not understand, the recipient MUST NOT process the response. A proxy that receives such a message SHOULD forward it downstream.

ISSUE: Could the constraints on the [:Events:] field not being understood be more specific?
<br/>
<br/>
Such as:
<br/>
"A recipient MUST ignore the [:Events:] header field received in response to a request method that is unrecognized or for which notifications response is not defined for a particular notifications protocol."

### Syntax ### {#events-header-syntax}

[:Events:] is a Dictionary structured ([[!STRUCTURED-FIELDS]] [[STRUCTURED-FIELDS#section-3.1|§ 3.1 Dictionaries]]) header field. It MUST contain one member with the key `protocol` whose value identifies the notification protocol used in the response. It MAY contain other members that are defined by the given notification protocol.
