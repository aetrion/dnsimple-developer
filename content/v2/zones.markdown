---
title: Zones API | DNSimple API v2 (Beta)
excerpt: This page documents the DNSimple zones API v2.
---

# Zones API

* TOC
{:toc}


## List zones {#list}

    GET /:account/zones

List zones in the account.

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id

### Example

List all zones in the account `1010`:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          https://api.dnsimple.com/v2/1010/zones

### Response

Responds with HTTP 200.

~~~json
<%= pretty_print_fixture("/listZones/success.http") %>
~~~

### Filters

Name | Description
-----|------------
`:name_like` | Only include zones containing given string

### Example

List all zones in the account `1010` that have name matching `"example"`:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          https://api.dnsimple.com/v2/1010/zones?name_like=example

### Sorting

For general information about sorting, please refer to the [main guide](/v2/#sorting).

Name | Description
-----|------------
`id` | Sort zones by ID
`name` | Sort zones by name (alphabetical order)

The default sorting policy is by ascending `name`.


## Get a zone {#get}

    GET /:account/zones/:zone

### Parameters

Name | Type | Description
-----|------|------------
`:account` | `integer` | The account id
`:zone` | `string` | The zone name

### Example

Get the zone `example.com` in the account `1010`:

    curl  -H 'Authorization: Bearer <token>' \
          -H 'Accept: application/json' \
          https://api.dnsimple.com/v2/1010/zones/example.com

### Response

Responds with HTTP 200, renders the zone.

~~~json
<%= pretty_print_fixture("/getZone/success.http") %>
~~~

