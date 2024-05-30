## Conditions
> The target should correspond to the following conditions for CSRF to be possible.

- There must be a **relevant action**: any privileged action or any action on user-specific data.
- The session handling must be **cookie-based**: no other mechanism that tracks sessions or validates user requests should be in place.
- There should not be any **unpredictable request parameter**: the attacker should be able to determine/guess the value of each parameter.

## Craft a Payload
> Using Burp's CSRF PoC Generator.

To generate a payload based on a request that will be tested: Right Click -> Engagement Tools -> Generate CSRF PoC

### Example

```html
<html>
 <body>
    <form action="https://vulnerable.com/email/change" method="POST">
      <input type="hidden" name="email" value="pwned@evil-user.net" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
 </body>
</html>
```

## CSRF Tokens

Unique, secret and unpredictable value shared to the client. 
They are then included by the client in requests that are considered sensitive, and validated by the server.

CSRF Tokens can be in the form of a hidden field in a form but also as a HTTP headers and others.

### Common CSRF Validation Flaws

#### Validation Depends on Method

Some applications correctly validate the token when the request uses the POST method but skip the validation when the GET method is used.

For instance, try crafting a GET request passing the body parameters as query strings.

#### Validation Depends on Token Being Present

Some applications correctly validate the token when it is present but skip the validation if the token is omitted.

Try removing the token -> the entire parameter, not just the value.

#### Token not Tied to User Session

Some applications do not validate that the token belongs to the same session as the user who is making the request.

Try using the attacker's token in the CSRF payload.

#### Token is Tied to a Non-Session Cookie

Some applications do not tie the token to the same cookie that is used to track sessions. There are then 2 cookies: one for sessions handling and another one for CSRF protection.

TODO

#### Token is Simply Duplicated in a Cookie

TODO

## `SameSite` Cookie Restrictions

A browser security that determines when a website's cookies are included in request that originate from other websites.

The context of a `SameSite` cookie is TLD+1 (Top Level Domain). For instance, in `app.example.com`, `example.com` is the context. In addition, the URL scheme is important -> `http://` **≠** `https://`

Major browsers support 3 `SameSite` restriction levels:
- **Strict**: The cookie will not be included if the target site does not match the current site.
- **Lax**: Cookie is only included in `GET` requests that result from a top-level domain navigation (i.e. clicking on a link).
- **None**: `SameSite` restrictions are disabled.

Default behaviour is `None` on major browsers at the exception of Chrome which defaults to `Lax`.

### Bypassing Lax Restrictions Using `GET`

When it is possible to use GET requests to perform actions, CSRF attacks may still be possible while involving a top-level navigation.

For instance:
```html
<script>
	document.location = 'https://vulnerable.com/account/transfer-payment?recipient=hacker&amount=1000000';
</script>
```

### Bypassing Restrictions Using On-Site Gadgets

It is sometimes possible to bypass the `SameSite=Strict` restriction by leveraging gadgets on the page. For instance, a client-side redirect with an attacker-controllable input as URL.

Look for places where controllable data is used to build URLs.