# Response

We know that you want to do some fancy pivot tables with your response data, so we’ve provided quite a few endpoints here to give you access to that response data. You can get overview numbers for all of your mailings and also drill down into finding out the actual members who opened a particular mailing.

## Get account response summary

> GET /:account_id/response

```json
[]
```

> GET /:account_id/response?range=2011-04-01~

```json
[]
```

> GET /:account_id/response?range=2011-04-01~2011-09-01

```json
[]
```

Get the response summary for an account.

This method will return a month-based time series of data including sends, opens, clicks, mailings, forwards, and opt-outs. Test mailings and forwards are not included in the data returned.

|   |   |
|---|---|
| **Raises**: | A list of objects with each object representing one month. Each object contains:
|   | `account_id` – The account_id for this line of stats
|   | `month` – two digit integer for month
|   | `year` – four digit integer for year
|   | `mailings` – number of mailings sent
|   | `sent` – number of messages sent
|   | `delivered` – number of messages delivered
|   | `bounced` – number of messages that failed delivery due to a hard or soft bounce
|   | `opened` – number of messages opened
|   | `clicked_unique` – link clicks, unique on message
|   | `clicked` – total link clicks, including duplicates
|   | `forwarded` – times the mailing was forwarded
|   | `shared` – total times a mailing has been shared
|   | `share_clicked` – total times a shared mailing has been seen
|   | `webview_shared` – total times a customer has shared their mailing
|   | `webview_share_clicked` – total times a customer-shared mailing has been seen
|   | `opted_out` – people who opted out based on this mailing
|   | `signed_up` – people who signed up based on this mailing

|   |   |
|---|---|
| **Parameters**: | `include_archived` (boolean) – Accepts 1. All other values are False. Optional flag to include archived mailings in the list.
|   | `range` (string) – Accepts 2 dates (YYYY-MM-DD) delimited by a tilde (~). Example: 2011-04-01~2011-09-01 Optional argument to limit results to a date range. If one of the dates is omitted, the default will be either min date or now. If a single date is provided with no tilde, then only mailings sent on that date will be included.

## Get response summary of single mailing

> GET /:account_id/response/200

```json
{
  "delivered": 0, 
  "signed_up": 0, 
  "clicked": 0, 
  "forwarded": 0, 
  "clicked_unique": 0, 
  "webview_share_clicked": 0, 
  "subject": "Sample Mailing for [% member:first_name %] [% member:last_name %]", 
  "opened": 0, 
  "opted_out": 0, 
  "share_clicked": 0, 
  "shared": 0, 
  "webview_shared": 0, 
  "in_progress": 0, 
  "bounced": 0, 
  "recipient_count": 0, 
  "sent": 0, 
  "name": "Sample Mailing"
}
```

Get the response summary for a particular mailing.

This method will return the counts of each type of response activity for a particular mailing.

|   |   |
|---|---|
| **Returns**: | A single object with the following fields:
|   | `name` – name of mailing
|   | `subject` – subject of the mailing
|   | `sent` – messages sent
|   | `delivered` – messages delivered
|   | `bounced` – messages that failed delivery due to a hard or soft bounce
|   | `opened` – messages opened
|   | `clicked_unique` – link clicks, unique on message
|   | `clicked` – total link clicks, including duplicates
|   | `forwarded` – times the mailing was forwarded
|   | `opted_out` – people who opted out based on this mailing
|   | `signed_up` – people who signed up based on this mailing
|   |`shared` – people who shared this mailing
|   | `share_clicked` – number of clicks shares of this mailing received
|   | `webview_shared` – number of times the customer has shared
|   | `webview_share_clicked` – number of clicks customer-shares of this mailing received

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List messages that have been sent to an MTA for delivery

> GET /:account_id/response/200/sends

```json
[
  {
    "fields": {
      "first_name": "Emma",
      "last_name": "Smith",
      "favorite_food": "tacos"
    },
    "timestamp": "@D:2011-01-02T10:27:43",
    "member_id": 200,
    "member_since": "@D:2010-11-12T11:23:45",
    "email_domain": "myemma.com",
    "email_user": "emma",
    "email": "emma@myemma.com",
    "member_status_id": "a"
  },
  {
    "fields": {
      "first_name": "Gladys",
      "last_name": "Jones",
      "favorite_food": "toast"
    },
    "timestamp": "@D:2011-01-02T10:27:43",
    "member_id": 201,
    "member_since": "@D:2011-01-03T15:54:13",
    "email_domain": "myemma.com",
    "email_user": "gladys",
    "email": "gladys@myemma.com",
    "member_status_id": "o"
  },
  {
    "fields": {

    },
    "timestamp": "@D:2011-01-02T10:27:43",
    "member_id": 204,
    "member_since": "@D:2010-12-13T23:12:44",
    "email_domain": "myemma.com",
    "email_user": "bob",
    "email": "bob@myemma.com",
    "member_status_id": "a"
  }
]
```

Get the list of messages that have been sent to an MTA for delivery.

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `timestamp` – time the message was sent
|   | `member_id` – id of the message addressee
|   | `email` – email of the message addressee

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List messages that are in the queue

> GET /:account_id/response/200/in_progress

```json
[
  {
    "fields": {
      "first_name": "Gladys",
      "last_name": "Jones",
      "favorite_food": "toast"
    },
    "member_id": 201,
    "member_since": "@D:2011-01-03T15:54:13",
    "email_domain": "myemma.com",
    "email_user": "gladys",
    "email": "gladys@myemma.com",
    "member_status_id": "o"
  },
  {
    "fields": {

    },
    "member_id": 204,
    "member_since": "@D:2010-12-13T23:12:44",
    "email_domain": "myemma.com",
    "email_user": "bob",
    "email": "bob@myemma.com",
    "member_status_id": "a"
  }
]
```

Get the list of messages that are in the queue, possibly sent, but not yet delivered.

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields: 
|  | `member_id` – id of the message addressee 
|  | `email` – email of the message addressee 

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist. |
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List delivered messages

> GET /:account_id/response/200/deliveries

```json
[
  {
    "delivery_type": "delivered",
    "email_domain": "myemma.com",
    "fields": {
      "first_name": "Emma",
      "last_name": "Smith",
      "favorite_food": "tacos"
    },
    "mailing_id": 200,
    "timestamp": "@D:2011-01-02T10:29:36",
    "member_id": 200,
    "member_status_id": "a",
    "member_since": "@D:2010-11-12T11:23:45",
    "mailing_name": "Sample Mailing",
    "email_user": "emma",
    "email": "emma@myemma.com"
  }
]
```

Get the list of messages that have finished delivery.

This will include those that were successfully delivered, as well as those that failed due to hard or soft bounces.

This list can be limited by `delivery_type`.

|   |   |
|---|---|
| **Parameters**: | `result` (string) – Accepted options: ‘all’, ‘delivered’, ‘bounced’, ‘hard’, ‘soft’. Defaults to ‘all’, if not provided.

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `timestamp` – time the message was delivered
|   | `delivery_type` – delivery outcome: delivered, hard, (bounce), or soft (bounce)
|   | `member_id` – id of the message addressee
|   | `email` – email of the message addressee

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List opened messages for a campaign

> GET /:account_id/response/200/opens

```json
[
  {
    "fields": {
      "first_name": "Emma",
      "last_name": "Smith",
      "favorite_food": "tacos"
    },
    "timestamp": "@D:2011-01-02T11:13:51",
    "member_id": 200,
    "member_since": "@D:2010-11-12T11:23:45",
    "email_domain": "myemma.com",
    "email_user": "emma",
    "email": "emma@myemma.com",
    "member_status_id": "a"
  },
  {
    "fields": {

    },
    "timestamp": "@D:2011-01-02T13:55:51",
    "member_id": 204,
    "member_since": "@D:2010-12-13T23:12:44",
    "email_domain": "myemma.com",
    "email_user": "bob",
    "email": "bob@myemma.com",
    "member_status_id": "a"
  }
]
```

Get the list of opened messages for this campaign.

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `timestamp` – time the message was opened
|   | `member_id` – id of the message addressee
|   | `email` – email of the message addressee

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List links for a mailing

> GET /:account_id/response/200/links

```json
[
  {
    "link_order": 1,
    "link_name": "Emma",
    "unique_clicks": 1,
    "plaintext": false,
    "link_target": "http://www.myemma.com",
    "total_clicks": 1,
    "link_id": 200
  }
]
```

Get the list of links for this mailing.

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `link_id` – If the mailing is not a trigger, the individual id of the link will be included
|   | `link_order` – order of the link in the mailing
|   | `link_name` – friendly name for the link
|   | `link_target` – link URL
|   | `unique_clicks` – clicks on the link, unique per message
|   | `total_clicks` – clicks on the link, total

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List clicks for a mailing

> GET /:account_id/response/200/clicks

```json
[
  {
    "link_id": 200,
    "fields": {
      "first_name": "Emma",
      "last_name": "Smith",
      "favorite_food": "tacos"
    },
    "timestamp": "@D:2011-01-02T11:14:32",
    "member_id": 200,
    "member_since": "@D:2010-11-12T11:23:45",
    "email_domain": "myemma.com",
    "email_user": "emma",
    "email": "emma@myemma.com",
    "member_status_id": "a"
  }
]
```

Get the list of clicks for this mailing.

This list can also be limited by member_id or link_id.

|   |   |
|---|---|
| **Parameters**: | `member_id` (int) – Limits results to a single member.
|   | `link_id` (int) – Limits results to a single link.

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `link_id` – id of the clicked link
|   | `timestamp` – time the link was clicked
|   | `member_id` – id of the message addressee
|   | `email` – email of the message addressee

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List forwards for a mailing

> GET /:account_id/response/200/forwards

```json
[]
```

Get the list of forwards for this mailing.

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `timestamp` – time the mailing was forwarded
|   | `forward_mailing_id` – id of the new mailing created to send the forward
|   | `member_id` – id of the forwarding member
|   | `email` – email of the forwarding member

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List optouts for a mailing

> GET /:account_id/response/200/optouts

```json
[
  {
    "fields": {

    },
    "timestamp": "@D:2011-01-02T13:56:16",
    "member_id": 204,
    "member_since": "@D:2010-12-13T23:12:44",
    "email_domain": "myemma.com",
    "email_user": "bob",
    "email": "bob@myemma.com",
    "member_status_id": "a"
  }
]
```

Get the list of optouts for this mailing.

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `timestamp` – time of the optout
|   | `member_id` – id of the opted out member
|   | `email` – email of the opted out member
|   | `first_name` – first name of the opted out member
|   | `last_name` – last name of the opted out member

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List signups for a mailing

> GET /:account_id/response/200/signups

```json
[
  {
    "ref_member_id": 200,
    "mailing_mailing_id": 200,
    "fields": {

    },
    "timestamp": "@D:2011-01-05T15:28:11",
    "member_id": 205,
    "member_since": "@D:2011-01-05T15:28:11",
    "email_domain": "myemma.com",
    "email_user": "newbie",
    "email": "newbie@myemma.com",
    "member_status_id": "a"
  }
]
```

Get the list of signups for this mailing.

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `member_id` – id of the signed up member
|   | `email` – email of the signed up member
|   | `timestamp` – time of the signup
|   | `first_name` – first name of the signed up member
|   | `last_name` – last name of the signed up member
|   | `ref_member_id` – id of the referring member

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List shares for a mailing

Get the list of shares for this mailing

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `member_id` – id of the shared message
|   | `network` – name of the network the share happened on
|   | `clicks` – number of clicks that the shared link received

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List customer shares for a mailing

Get the list of customer shares for this mailing

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `timestamp` – timestamp of the shared link click
|   | `network` – name of the network the share happened on

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## List customer share clicks for a mailing

Get the list of customer share clicks for this mailing

|   |   |
|---|---|
| **Returns**: | An array of objects with the following fields:
|   | `timestamp` – timestamp of the shared link click
|   | `network` – name of the network the share happened on
|   | `clicks` – number of clicks that the shared link received

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## Get customer share information

Get the customer share associated with the share id.

|   |   |
|---|---|
| **Returns**: | An object with the following fields:
|   | `timestamp` – timestamp of the share
|   | `network` – name of the network the share happened on
|   | `share_status` – status of the share

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid mailing type - ‘m’ for standard mailings, ‘t’ for test mailings and ‘r’ for trigger mailings.

## Get overview of shares

Get overview of shares pertaining to this `mailing_id`.

|   |   |
|---|---|
| **Returns**: | An object with the following fields:
|   | `network` – name of the network the share happened on
|   | `share_clicks` – number of clicks that the shared link received
|   | `share_count` – number of shares to that network

|   |   |
|---|---|
| **Raises**: | `Http404` if the mailing does not exist.
|   | `Http404` if the mailing is not valid