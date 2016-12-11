---
title: Registrar API | DNSimple API v2 (Beta)
excerpt: This page documents the DNSimple registry/registrar API v2.
---

# Registrar API

* TOC
{:toc}


## Check domain availability {#check}

    GET /:account/registrar/domains/:domain/check

Check if the domain is available for registration or transfer.

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:domain` | `string` | The domain name

### Example

Check the domain `example.com` in the account `1010`:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          -X GET \
          https://api.dnsimple.com/v2/1010/registrar/domains/example.com/check

### Response

Responds with HTTP 200 on success, returns the domain availability information.

~~~json
<%= pretty_print_fixture("/checkDomain/success.http") %>
~~~

<warning>
  If the domain is premium (`premium: true`), please [check the premium price](#premium-price) before to try to [register](#register), [renew](#renew), [transfer](#transfer).
</warning>

## Check domain premium price {#premium-price}

    GET /:account/registrar/domains/:domain/premium_price

Return a premium price for a domain.

**Please note** that a premium price can be different for [registration](#register), [renewal](#renew), [transfer](#transfer).
By default this endpoint returns the premium price for registration.
If you need to check a different price, you should specify it with the `action` param.

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:domain` | `string` | The domain name

### Input

Name | Type | Description
-----|------|------------
`action` | `string` | Optional action between `"registration"`, `"renewal"`, and `"transfer"`. If omitted, it defaults to `"registration"`.

### Examples

Check the premium price for `example.com` domain:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          -X GET \
          https://api.dnsimple.com/v2/1010/registrar/domains/example.com/premium_price

Check the premium price for `example.com` domain renewal:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          -X GET \
          https://api.dnsimple.com/v2/1010/registrar/domains/example.com/premium_price?action=renewal

### Response

Responds with HTTP 200 on success, returns the domain premium price.

~~~json
<%= pretty_print_fixture("/getDomainPremiumPrice/success.http") %>
~~~

Responds with HTTP 400, if the domain isn't premium.

~~~json
<%= pretty_print_fixture("/getDomainPremiumPrice/failure.http") %>
~~~

## Register a domain {#register}

    POST /:account/registrar/domains/:domain/registration

Register a domain name with DNSimple.

Your account must be active for this command to complete successfully.
You will be automatically charged the registration fee upon successful registration, so please be careful with this command.

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:domain` | `string` | The domain name

### Example

Register the domain `example.com` in the account `1010`:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          -X POST \
          -d '<json>' \
          https://api.dnsimple.com/v2/1010/registrar/domains/example.com/registration

### Input

Name | Type | Description
-----|------|------------
`registrant_id` | `integer` | **Required**. The ID of an existing contact in your account.
`private_whois` | `bool` | Set to true to enable the whois privacy service. An extra cost may apply. Default: `false`.
`auto_renew` | `bool` | Set to true to enable the auto-renewal of the domain. Default: `true`.
`extended_attributes` | `hash` | **Required** for TLDs that require [extended attributes](/v2/tlds/#extended-attributes).
`premium_price` | `string` | **Required** as confirmation of the price, only if the domain is premium.

<note>
The `registrant_id` can be fetched via the [contacts endpoint](/v2/contacts) and will be the registered contact for this domain.
</note>

<note>
The `premium_price` can be fetched via the [premium price endpoint](#premium-price).
</note>

##### Example

~~~json
{
  "registrant_id": 1
}
~~~

### Response

Responds with HTTP 201 on success, returns the domain.

~~~json
<%= pretty_print_fixture("/registerDomain/success.http") %>
~~~

Responds with HTTP 400 if the validation fails.


## Transfer a domain {#transfer}

    POST /:account/registrar/domains/:domain/transfer

Transfer a domain name from another domain registrar into DNSimple.

Your account must be active for this command to complete successfully.
You will be automatically charged the 1-year transfer fee upon successful transfer, so please be careful with this command.
The transfer may take anywhere from a few minutes up to 7 days.

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:domain` | `string` | The domain name

### Example

Transfer the domain `example.com` in the account `1010`:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          -X POST \
          -d '<json>' \
          https://api.dnsimple.com/v2/1010/registrar/domains/example.com/transfer

### Input

Name | Type | Description
-----|------|------------
`registrant_id` | `integer` | **Required**. The ID of an existing contact in your account.
`auth_code` | `string` | **Required** for TLDS that require authorization-based transfer (the vast majority of TLDs).
`private_whois` | `bool` | Set to true to enable the whois privacy service. An extra cost may apply. Default: `false`.
`auto_renew` | `bool` | Set to true to enable the auto-renewal of the domain. Default: `true`.
`extended_attributes` | `hash` | **Required** for TLDs that require [extended attributes](/v2/tlds/#extended-attributes).
`premium_price` | `string` | **Required** as confirmation of the price, only if the domain is premium.

##### Example

~~~json
{
  "registrant_id": 1,
  "auth_code": "xjfjfjvhc293"
}
~~~

##### Example with extended attributes

~~~json
{
  "registrant_id": 1,
  "auth_code": "xjfjfjvhc293",
  "extended_attribute": {
    "us_nexus": "C11",
    "us_purpose": "P3"
  }
}
~~~

### Response

Responds with HTTP 201 on success, returns the domain.

~~~json
<%= pretty_print_fixture("/transferDomain/success.http") %>
~~~

Responds with HTTP 400 if the validation fails.


## Renew a domain {#renew}

    POST /:account/registrar/domains/:domain/renewal

Renew a domain name already registered with DNSimple.

Your account must be active for this command to complete successfully.
You will be automatically charged the renewal fee upon successful renewal, so please be careful with this command.

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:domain` | `string` | The domain name

### Example

Renew the domain `example.com` in the account `1010`:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          -X POST \
          https://api.dnsimple.com/v2/1010/registrar/domains/example.com/renewal

### Input

Name | Type | Description
-----|------|------------
`period` | `integer` | The number of years. Unless specified it will default to whatever value is set for the TLD.
`premium_price` | `string` | **Required** as confirmation of the price, only if the domain is premium.

##### Example with optional period

~~~json
{
  "period": 2
}
~~~

### Response

Responds with HTTP 201 on success, returns the domain.

~~~json
<%= pretty_print_fixture("/renewDomain/success.http") %>
~~~

Responds with HTTP 400 if the validation fails.


## Transfer out a domain {#transfer-out}

    POST /:account/registrar/domains/:domain/transfer_out

Prepare a domain for transferring out. This will unlock a domain and send the authorization code to the domain's administrative contact.

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:domain` | `string` | The domain name

### Example

Transfer out the domain `example.com` in the account `1010`:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          -H 'Content-Type: application/json' \
          -X POST \
          https://api.dnsimple.com/v2/1010/registrar/domains/example.com/transfer_out

### Response

Responds with HTTP 204 on success.

Responds with HTTP 400 if the validation fails.
