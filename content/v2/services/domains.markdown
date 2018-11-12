---
title: Domain services API | Services | DNSimple API v2
excerpt: This page documents the DNSimple domain services API v2.
---

# Domain services API

* TOC
{:toc}


## List applied services {#applied}

    GET /:account/domains/:domain/services

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:domain` | `string`, `integer` | The domain name or id

### Example

List applied services for domain with ID `1` in the account `1010`:

    curl -H 'Authorization: Bearer <token>' \
         -H 'Accept: application/json' \
         https://api.dnsimple.com/v2/1010/domains/1/services

### Response

~~~json
<%= pretty_print_fixture("/api/appliedServices/success.http") %>
~~~

## Apply a service {#apply}

    POST /:account/domains/:domain/services/:service

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:domain` | `string`, `integer` | The domain name or id
`:service` | `string`, `integer` | The service name or id

### Example

Apply a service with ID `2` to domain with ID `1`, in the account `1010`:

    curl -H 'Authorization: Bearer <token>' \
         -H 'Accept: application/json' \
         -H 'Content-Type: application/json' \
         -X POST \
         -d '<json>' \
         https://api.dnsimple.com/v2/1010/domains/1/services/2

### Input

Name | Type | Description
-----|------|------------
`settings` | `hash` | Optional hash of settings for some specific services, corresponding to the [service fields](https://github.com/aetrion/dnsimple-services#fields). For instance, [Heroku requires a `settings[app]` setting](https://github.com/aetrion/dnsimple-services/blob/master/services/heroku/config.json#L8-L14).

### Response

Responds with HTTP 204 on success.


## Unapply a service {#unapply}

    DELETE /:account/domains/:domain/services/:service

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:domain` | `string`, `integer` | The domain name or id
`:service` | `string`, `integer` | The service name or id

### Example

Unapply the service with ID `2` from domain with ID `1`, in the account `1010`:

    curl -H 'X-DNSimple-Token: <email>:<token>' \
         -H 'Accept: application/json' \
         -X DELETE \
         https://api.dnsimple.com/v2/1010/domains/1/services/2

### Response

Responds with HTTP 204 on success.
