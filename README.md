# A Simple SAML Identity Provider (IdP) for Testing SSO Interfaces

## Using test-saml-idp at Sitka

* If using locally against the existing okta dev IDP, ask Renaud or Micah for the test certificates and save to this folder.

### One time setup
```python
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### Running
```python
source venv/bin/activate
python3 saml-idp.py
```

### Testing
* have all the sitka api and web services running like normal
* when redirected to okta to login, enter a domain that is routed to the test saml in okta settings (e.g. test@dataconcise.com)
* for any certificate warnings, click "Advanced" then proceed despite warning.  This is a standard warning for self-signed certs.
* For the username, enter any valid email.  Note that the email will get written to the Sitka user on successful login.
* For the External User ID, this will get matched to the existing User.external_user_id in Sitka's database if the idp_id of the user is `0oa13b51s1ItyodrM1d7`.
* Click submit, then submit again.  You should be redirected back to the local sitka dev server and be authenticated as that user. 


# Below is the original content from the author


This project contains a SAML IdP developed at Randori for testing SSO
interfaces in other products.

It is not intended to provide any actual security, but rather as a starting
point to test SAML interfaces in products supporting external identity providers.

As released, this code provides a near-bare-minimum set of functionality to
complete a SAML sign-on process. It doesn't implement all of the spec, but the
parts that are implemented are done so in a way that is easy to hack in new test
functionality.

We hope you find it useful for testing!

## Building

```
./make-key.sh
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Configuration

There are a few variables at the top of saml-idp.py to control the host, port
and whether requests should be decompressed or not (this isn't autodetected).

This is meant to be a starting point for your modifications, so most
'configuration' should be done by editing code to suit whatever SAML function
you are attempting to probe.

There is a limited ability to change a few behaviors at runtime, such as which
parts of the response to sign, and which username to return as the logged in
user.

## Running

```
source venv/bin/activate
python3 saml-idp.py
```

If you haven't changed the defaults, your IdP is running on 0.0.0.0:10443 with
metadata at https://localhost:10443/metadata. The default ACS endpoint is
https://localhost:10443/, so visiting the site directly will give you an error
related to no SAMLRequest value in your request.

## Gotchas

Even though the XML is shown decoded on the login page, only the encoded
value is returned, so editing the XML response directly (in the preview
textarea in your browser) will have no effect unless you want to add some
javascript to update the encoded value.

## Contributing

Any improvements that keep with the spirit of the project are welcome as PRs!

Better as a PR: Adding attributes to the XML response.

Better as a fork: Replacing the XML and signature generating code with a
third-party library which just "takes care of it."


