<h2 id="considerations" class="no-num">
  Considerations
</h2>

This section details security and privacy considerations.

Some of the normative references within this specification point to documents with a Living Standard or Draft status, meaning their contents can still change over time. It is advised to monitor these documents, as such changes might have implications.

## Security Considerations ## {#security-considerations}

*This section is non-normative.*

[=Resource servers=] are strongly discouraged from exposing information beyond the minimum amount necessary to enable a feature. [=Application clients=] are strongly discouraged from exposing information beyond the minimum amount necessary to receive updates about particular resources.

[=Application clients=] are discouraged from sending requests for notifications to untrusted [=resource servers=], including localhost or any other loopback IP address, in order to avoid making arbitrary requests.

## Privacy Considerations ## {#privacy-considerations}

*This section is non-normative.*

## Security and Privacy Review ## {#security-and-privacy-review}

*This section is non-normative.*

These questions provide an overview of security and privacy considerations for this specification as guided by [[SECURITY-PRIVACY-QUESTIONNAIRE]].

: [What information might this feature expose to Web sites or other parties, and for what purposes is that exposure necessary?](https://www.w3.org/TR/security-privacy-questionnaire/#purpose)
:: There are no known security impacts of the features in this specification.

: [Do features in your specification expose the minimum amount of information necessary to enable their intended uses?](https://www.w3.org/TR/security-privacy-questionnaire/#minimum-data)
:: Yes.

: [How do the features in your specification deal with personal information, personally-identifiable information (PII), or information derived from them?](https://www.w3.org/TR/security-privacy-questionnaire/#personal-data)
:: Access to [=resource server=] is only granted to authorized access subjects. Notification payloads can contain any data (including that which identifies or refers to agents that control the [=resource server=]). Meaningful consent to any personal data that [=resource servers=] include about agents associated with themselves or resources of interest are extended to the [=application client=]. [=Resource servers=] are discouraged from exposing information beyond the amount necessary to enable or use a feature.

: [How do the features in your specification deal with sensitive information?](https://www.w3.org/TR/security-privacy-questionnaire/#sensitive-data)
:: The features do not require sensitive information to be obtained or exposed.

: [Do the features in your specification introduce new state for an origin that persists across browsing sessions?](https://www.w3.org/TR/security-privacy-questionnaire/#persistent-origin-specific-state)
:: No.

: [Do the features in your specification expose information about the underlying platform to origins?](https://www.w3.org/TR/security-privacy-questionnaire/#underlying-platform-data)
:: No.

: [Does this specification allow an origin to send data to the underlying platform?](https://www.w3.org/TR/security-privacy-questionnaire/#send-to-platform)
:: No. Resources are described within the framework of HTTP. A [=resource server=] might be able to redirect to other resources, (e.g., the https: URLs to file:, data:, or blob: URLs), but no such behaviour is defined by this specification.

: [Do features in this specification allow an origin access to sensors on a user’s device](https://www.w3.org/TR/security-privacy-questionnaire/#sensor-data)
:: No.

: [What data do the features in this specification expose to an origin? Please also document what data is identical to data exposed by other features, in the same or different contexts.](https://www.w3.org/TR/security-privacy-questionnaire/#other-data)
:: No detail about another origin’s state is exposed. When [=resource servers=] participate in the CORS protocol [[!FETCH]], HTTP requests from different origins might be allowed. This feature does not add any new attack surface above and beyond normal CORS requests, so no extra mitigation is deemed necessary.

: [Do features in this specification enable new script execution/loading mechanisms?](https://www.w3.org/TR/security-privacy-questionnaire/#string-to-script)
:: No.

: [Do features in this specification allow an origin to access other devices?](https://www.w3.org/TR/security-privacy-questionnaire/#remote-device)
:: No.

: [Do features in this specification allow an origin some measure of control over a user agent’s native UI?](https://www.w3.org/TR/security-privacy-questionnaire/#native-ui)
:: No.

: [What temporary identifiers do the features in this specification create or expose to the web?](https://www.w3.org/TR/security-privacy-questionnaire/#temporary-id)
:: No.

: [How does this specification distinguish between behaviour in first-party and third-party contexts?](https://www.w3.org/TR/security-privacy-questionnaire/#first-third-party)
:: Not Applicable.

: [How do the features in this specification work in the context of a browser’s Private Browsing or Incognito mode?](https://www.w3.org/TR/security-privacy-questionnaire/#private-browsing)
:: No different than in the browser’s "normal" state.

: [Does this specification have both "Security Considerations" and "Privacy Considerations" sections?](https://www.w3.org/TR/security-privacy-questionnaire/#considerations)
:: Yes, in [[#security-considerations]] and [[#privacy-considerations]].

: [Do features in your specification enable origins to downgrade default security protections?](https://www.w3.org/TR/security-privacy-questionnaire/#relaxed-sop)
:: No.

: [How does your feature handle non-"fully active" documents?](https://www.w3.org/TR/security-privacy-questionnaire/#non-fully-active)
:: Not Applicable.
