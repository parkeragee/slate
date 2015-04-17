# Mailings

With these endpoints, you can get information about your mailings including their HTML contents. You can retrieve the members to whom the mailing was sent. You can also pause mailings and cancel mailings that are pending or paused.

## GET /#account_id/mailings

> GET /100/mailings

```json
[
  {
    "mailing_type": "m",
    "send_started": null,
    "cancel_by_user_id": null,
    "mailing_id": 201,
    "recipient_count": 0,
    "cancel_ts": null,
    "mailing_status": "p",
    "month": null,
    "failure_ts": null,
    "reply_to": null,
    "year": null,
    "datacenter": null,
    "started_or_finished": null,
    "account_id": 100,
    "disabled": false,
    "created_ts": "@D:2013-08-22T09:41:45",
    "sender": "Kevin McConnell",
    "plaintext_only": false,
    "name": "Cancellable mailing",
    "parent_mailing_id": null,
    "failure_message": null,
    "send_finished": null,
    "send_at": null,
    "signup_form_id": null,
    "subject": "Cancellable mailing",
    "purged_at": null,
    "archived_ts": null
  },
  {
    "mailing_type": "m",
    "send_started": null,
    "cancel_by_user_id": null,
    "mailing_id": 200,
    "recipient_count": 0,
    "cancel_ts": null,
    "mailing_status": "c",
    "month": null,
    "failure_ts": null,
    "reply_to": null,
    "year": null,
    "datacenter": null,
    "started_or_finished": null,
    "account_id": 100,
    "disabled": false,
    "created_ts": "@D:2013-08-22T09:41:45",
    "sender": "Kevin McConnell",
    "plaintext_only": false,
    "name": "Sample Mailing",
    "parent_mailing_id": null,
    "failure_message": null,
    "send_finished": null,
    "send_at": null,
    "signup_form_id": null,
    "subject": "Sample Mailing for [% member:first_name %] [% member:last_name %]",
    "purged_at": null,
    "archived_ts": null
  }
]
```

> GET /100/mailings?mailing_statuses=p,c&mailing_types=m,t

```json
[
  {
    "mailing_type": "m",
    "send_started": null,
    "cancel_by_user_id": null,
    "mailing_id": 201,
    "recipient_count": 0,
    "cancel_ts": null,
    "mailing_status": "p",
    "month": null,
    "failure_ts": null,
    "reply_to": null,
    "year": null,
    "datacenter": null,
    "started_or_finished": null,
    "account_id": 100,
    "disabled": false,
    "created_ts": "@D:2013-08-22T09:41:45",
    "sender": "Kevin McConnell",
    "plaintext_only": false,
    "name": "Cancellable mailing",
    "parent_mailing_id": null,
    "failure_message": null,
    "send_finished": null,
    "send_at": null,
    "signup_form_id": null,
    "subject": "Cancellable mailing",
    "purged_at": null,
    "archived_ts": null
  },
  {
    "mailing_type": "m",
    "send_started": null,
    "cancel_by_user_id": null,
    "mailing_id": 200,
    "recipient_count": 0,
    "cancel_ts": null,
    "mailing_status": "c",
    "month": null,
    "failure_ts": null,
    "reply_to": null,
    "year": null,
    "datacenter": null,
    "started_or_finished": null,
    "account_id": 100,
    "disabled": false,
    "created_ts": "@D:2013-08-22T09:41:45",
    "sender": "Kevin McConnell",
    "plaintext_only": false,
    "name": "Sample Mailing",
    "parent_mailing_id": null,
    "failure_message": null,
    "send_finished": null,
    "send_at": null,
    "signup_form_id": null,
    "subject": "Sample Mailing for [% member:first_name %] [% member:last_name %]",
    "purged_at": null,
    "archived_ts": null
  }
]
```

|   |   |
|---|---|
| **Parameters**: | `include_archived` (boolean) – Optional flag to include archived mailings in the list.
|   | `mailing_types` (string) – Accepts a comma-separated string with one or more of the following mailing types: ‘m’ (standard), ‘t’ (test), ‘r’ (trigger), ‘s’ (split). Defaults to ‘m,t’, standard and test mailings, when none are specified.
|   | `mailing_statuses` (string) – Accepts a comma-separated string with one or more of the following mailing statuses: ‘p’ (pending), ‘a’ (paused), ‘s’ (sending), ‘x’ (canceled), ‘c’ (complete), ‘f’ (failed). Defaults to ‘p,a,s,x,c,f’, all statuses, when none are specified.
|   | `is_scheduled` (boolean) – Mailings that have a scheduled timestamp.
|   | `with_html_body` (boolean) – Include the html_body content.
|   | `with_plaintext` (boolean) – Include the plaintext content.

|   |   |
|---|---|
| **Returns**: | An array of mailings.

|   |   |
|---|---|
| **Raises**: | `Http400` if invalid mailing types or statuses are specified.

## GET /#account_id/mailings/#mailing_id

> GET /100/mailings/200

```json
{
  "recipient_groups": [
    {
      "member_group_id": 151, 
      "name": "Widget Buyers"
    }
  ], 
  "heads_up_emails": [], 
  "send_started": null, 
  "signup_form_id": null, 
  "links": [
    {
      "mailing_id": 200, 
      "plaintext": false, 
      "link_id": 200, 
      "link_name": "Emma", 
      "link_target": "http://www.myemma.com", 
      "link_order": 1
    }
  ], 
  "mailing_id": 200, 
  "plaintext": "Hello [% member:first_name %]!", 
  "recipient_count": 0, 
  "public_webview_url": "http://localhost/webview/uf/6db0cc7e6fdb2da589b65f29d90c96b6", 
  "mailing_type": "m", 
  "parent_mailing_id": null, 
  "recipient_searches": [], 
  "account_id": 100, 
  "recipient_members": [
    {
      "email": "emma@myemma.com", 
      "member_id": 200
    }
  ], 
  "mailing_status": "c", 
  "sender": "Kevin McConnell", 
  "name": "Sample Mailing", 
  "send_finished": null, 
  "send_at": null, 
  "subject": "Sample Mailing for [% member:first_name %] [% member:last_name %]", 
  "archived_ts": null, 
  "html_body": "<p>Hello [% member:first_name %]!</p>"
}
```

Get detailed information for one mailing.

|   |   |
|---|---|
| **Returns**: | A mailing.

|   |   |
|---|---|
| **Raises**: | `Http404` if no mailing is found.

## GET /#account_id/mailings/#mailing_id/members

> GET /100/mailings/200/members

```json 
[
  {
    "status": "active",
    "confirmed_opt_in": null,
    "account_id": 100,
    "fields": {
      "first_name": "Emma",
      "last_name": "Smith",
      "favorite_food": "tacos"
    },
    "member_id": 200,
    "last_modified_at": null,
    "member_status_id": "a",
    "plaintext_preferred": false,
    "email_error": null,
    "member_since": "@D:2010-11-12T11:23:45",
    "bounce_count": 0,
    "deleted_at": null,
    "email": "emma@myemma.com"
  }
]
```

Get the list of members to whom the given mailing was sent. This does not include groups or searches.

|   |   |
|---|---|
| **Returns**: | An array of members including status and member fields.

|   |   |
|---|---|
| **Raises**: | `Http404` if no mailing is found.

## GET /#account_id/mailings/#mailing_id/messages/#member_id

> GET /100/mailings/200/messages/200

```json
{
  "plaintext": "Hello !", 
  "subject": "Sample Mailing for  ", 
  "html_body": "<p>Hello !</p>"
}
```

> GET /100/mailings/200/messages/200?type=html

```json
{
  "html_body": "<p>Hello !</p>"
}
```

Gets the personalized message content as sent to a specific member as part of the specified mailing.

|   |   |
|---|---|
| **Parameters**: | `type` (string) – Accepts: ‘all’, ‘html’, ‘plaintext’, ‘subject’. Defaults to ‘all’, if not provided.

|   |   |
|---|---|
| **Returns**: | Message content from a mailing, personalized for a member. The response will contain all parts of the mailing content by default, or just the type of content specified by type.

|   |   |
|---|---|
| **Raises**: | `Http404` if no message is found.

## GET /#account_id/mailings/#mailing_id/groups

> GET /100/mailings/200/groups

```json
[
  {
    "active_count": 2,
    "deleted_at": null,
    "error_count": 0,
    "optout_count": 0,
    "group_type": "g",
    "member_group_id": 151,
    "purged_at": null,
    "account_id": 100,
    "group_name": "Widget Buyers"
  }
]
```

Get the groups to which a particular mailing was sent.

|   |   |
|---|---|
| **Returns**: | An array of groups.

|   |   |
|---|---|
| **Raises**: | `Http404` if no mailing is found.

## GET /#account_id/mailings/#mailing_id/searches

> GET /100/mailings/200/searches

```json
[]
```

Get all searches associated with a sent mailing.

|   |   |
|---|---|
| **Returns**: | An array of searches.

|   |   |
|---|---|
| **Raises**: | `Http404` if no mailing is found.

## PUT /#account_id/mailings/#mailing_id

Update status of a current mailing

The status can be one of `canceled`, `paused` or `ready`. This method can be used to control the progress of a mailing by pausing, canceling or resuming it. Once a mailing is canceled it can’t be resumed, and will not show in the normal mailing_list output.

Returns the mailing’s new status

## DELETE /#account_id/mailings/#mailing_id

> DELETE /100/mailings/200

```json
true
```

Sets archived timestamp for a mailing so it is no longer included in mailing_list.

|   |   |
|---|---|
| **Returns**: | `True` if the mailing is successfully archived.

## DELETE /#account_id/mailings/cancel/#mailing_id

> DELETE /100/mailings/cancel/201

```json
true
```

Cancels a mailing that has a current status of pending or paused. All other statuses will result in a 404.

|   |   |
|---|---|
| **Returns**: | `True` if mailing marked as cancelled.

## POST /#account_id/mailings

Pass public create_mailing requests through a filter to determine if the account is permitted to use the private api create mailing

## POST /#account_id/forwards/#mailing_id/#member_id

> POST /100/forwards/200/200

```json
{
  "recipient_emails": [
    "alex@myemma.com", 
    "chris@myemma.com"
  ]
}

{
  "mailing_id": 1024
}
```

Forward a previous message to additional recipients. If these recipients are not already in the audience, they will be added with a status of FORWARDED.

|   |   |
|---|---|
| **Parameters**: | `recipient_emails` (array) – An array of email addresses to which to forward the specified message.
|   | `note` (string) – A note to include in the forward. This note will be HTML encoded and is limited to 500 characters.

|   |   |
|---|---|
| **Returns**: | A reference to the new mailing.

|   |   |
|---|---|
| **Raises**: | `Http404` if no message is found.

## POST /#account_id/mailings/#mailing_id

> POST /100/mailings/200

```json
{
  "recipient_emails": [
    "michelle@myemma.com"
  ]
}

{
  "mailing_id": 1024
}
```

Send a prior mailing to additional recipients. A new mailing will be created that inherits its content from the original.

|   |   |
|---|---|
| **Parameters**: | `sender` (string) – The message sender. If this is not supplied, the sender of the original mailing will be used.
|   | `heads_up_emails` (array) – A list of email addresses that heads up notification emails will be sent to.
|   | `recipient_emails` (array) – An array of email addresses to which the new mailing should be sent.
|   | `recipient_groups` (array) – An array of member groups to which the new mailing should be sent.
|   | `recipient_searches` (array) – A list of searches that this mailing should be sent to.

|   |   |
|---|---|
| **Returns**: | A reference to the new mailing.

|   |   |
|---|---|
| **Raises**: | `Http404` if no mailing is found.

## GET /#account_id/mailings/#mailing_id/headsup

> GET /100/mailings/200/headsup

```json
[]
```

Get heads up email address(es) related to a mailing.

|   |   |
|---|---|
| **Returns**: | An array of heads up email addresses.

|   |   |
|---|---|
| **Raises**: | `Http404` if no mailing is found.

## POST /#account_id/mailings/validate

> POST /100/mailings/validate

```json
{
  "subject": "Another Fine Test"
}

true
```

Validate that a mailing has valid personalization-tag syntax. Checks tag syntax in three params:

|   |   |
|---|---|
| **Parameters**: | `html_body` (string) – The html contents of the mailing
|   | `plaintext` (string) – The plaintext contents of the mailing. Unlike in create_mailing, this param is not required.
|   | `subject` (string) – The subject of the mailing

|   |   |
|---|---|
| **Returns**: | `True`

|   |   |
|---|---|
| **Raises**: | `Http400` if any tags are invalid. The response body will have information about the invalid tags.

## POST /#account_id/mailings/#mailing_id/winner/#winner_id

> POST /100/mailings/202/winner/203

```json
{}

true
```

Declare the winner of a split test manually. In the event that the test duration has not elapsed, the current stats for each test will be frozen and the content defined in the user declared winner will sent to the remaining members for the mailing. Please note, any messages that are pending for each of the test variations will receive the content assigned to them when the test was initially constructed.

|   |   |
|---|---|
| **Returns**: | `True` on success.

|   |   |
|---|---|
| **Raises**: | `Http403` if the winner cannot be manually declared.