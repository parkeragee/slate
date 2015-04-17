# Triggers

These endpoints provide CRUD operations for our trigger system. Creating a trigger is probably the most widely used of these operations.

## List triggers

> GET /:account_id/triggers

```json
[
  {
    "parent_mailing": {
      "mailing_type": "m",
      "send_started": null,
      "signup_form_id": null,
      "mailing_id": 200,
      "plaintext": "Hello [% member:first_name %]!",
      "recipient_count": 0,
      "cancel_ts": null,
      "created_ts": "@D:2013-08-22T09:41:45",
      "month": null,
      "failure_ts": null,
      "year": null,
      "datacenter": null,
      "started_or_finished": null,
      "account_id": 100,
      "disabled": false,
      "mailing_status": "c",
      "plaintext_only": false,
      "sender": "Kevin McConnell",
      "parent_mailing_id": null,
      "failure_message": null,
      "name": "Sample Mailing",
      "send_finished": null,
      "cancel_by_user_id": null,
      "send_at": null,
      "reply_to": null,
      "subject": "Sample Mailing for [% member:first_name %] [% member:last_name %]",
      "purged_at": null,
      "archived_ts": null,
      "html_body": "<p>Hello [% member:first_name %]!</p>"
    },
    "surveys": null,
    "event_type": "r",
    "links": null,
    "field_id": 203,
    "signup_integrations": null,
    "push_offset_units": "0:-14:0:0",
    "start_ts": "@D:2013-08-22T09:41:45",
    "trigger_id": 100,
    "name": "Birthday triggers",
    "signups": null,
    "push_offset": "@I:-1209600.0",
    "groups": null,
    "parent_mailing_id": 200,
    "deleted_at": null,
    "is_disabled": false,
    "account_id": 100
  },
  {
    "parent_mailing": {
      "mailing_type": "m",
      "send_started": null,
      "signup_form_id": null,
      "mailing_id": 200,
      "plaintext": "Hello [% member:first_name %]!",
      "recipient_count": 0,
      "cancel_ts": null,
      "created_ts": "@D:2013-08-22T09:41:45",
      "month": null,
      "failure_ts": null,
      "year": null,
      "datacenter": null,
      "started_or_finished": null,
      "account_id": 100,
      "disabled": false,
      "mailing_status": "c",
      "plaintext_only": false,
      "sender": "Kevin McConnell",
      "parent_mailing_id": null,
      "failure_message": null,
      "name": "Sample Mailing",
      "send_finished": null,
      "cancel_by_user_id": null,
      "send_at": null,
      "reply_to": null,
      "subject": "Sample Mailing for [% member:first_name %] [% member:last_name %]",
      "purged_at": null,
      "archived_ts": null,
      "html_body": "<p>Hello [% member:first_name %]!</p>"
    },
    "surveys": null,
    "event_type": "s",
    "links": null,
    "field_id": null,
    "signup_integrations": null,
    "push_offset_units": "0:3:0:0",
    "start_ts": "@D:2013-08-22T09:41:45",
    "trigger_id": 101,
    "name": "Test Signup Form triggers",
    "signups": [
      1,
      2,
      3
    ],
    "push_offset": "@I:259200.0",
    "groups": null,
    "parent_mailing_id": 200,
    "deleted_at": null,
    "is_disabled": false,
    "account_id": 100
  }
]
```

## Create new trigger

> POST /:account_id/triggers

```json
{
  "parent_mailing_id": 200, 
  "object_ids": [
    10, 
    20, 
    30
  ], 
  "name": "Test", 
  "event_type": "c"
}

1024
```

Create a new trigger.

|   |   |
|---|---|
| **Parameters**: | `name` (string) – A descriptive name for the trigger.
|   | `event_type` – The type of event that causes this trigger to fire.
|   | Accepts one of ‘s’ (signup), ‘c’ (click), ‘f’ (trigger flag), ‘u’ (survey), ‘si’ (signup_integration), ‘d’ (date), ‘r’ (recurring date). :type event_type: string
|   | `parent_mailing_id` (integer) – The id of the mailing this trigger will use as a template for message generation.
|   | `groups` (array) – An optional array of group ids to which this trigger will apply.
|   | `links` (array) – An array of link_ids for click triggers.
|   | `signups` (array) – An array of signup_form_ids for signup triggers.
|   | `signup_integrations` (array) – An array of oauth app ids for signup integration triggers.
|   | `surveys` (array) – An array of survey_ids for survey triggers.
|   | `field_id` (integer) – A field id to which this trigger will apply if it is a date or recurring date trigger.
|   | `push_offset` (integer) – An optional delay interval for messages created from this trigger (specified in seconds).
|   | `is_disabled` (boolean) – An optional flag to disable the trigger.
|   | `from_operator` (string) – A string that specifies an operator value for the from operator field.
|   | `from_value` (string) – A string that specifies the value of the from member data field.
|   | `to_operator` (string) – A string that specifies an operator value for the to operator field.
|   | `to_value` (string) – A string that specifies the value of the to member data field.

|   |   |
|---|---|
| **To/From Values**: | These are the specific values used in the from_operator and to_operator parameters.
|   | anything
|   | contains
|   | equals
|   | does-not-equal
|   | empty
|   | greater-than
|   | greater-than-or-equal-to
|   | less-than
|   | less-than-or-equal-to

|   |   |
|---|---|
| **Returns**: | The new trigger’s id.

|   |   |
|---|---|
| **Raises**: | `Http500` if object_ids is null and event_type is one of: ‘s’ (signup), ‘c’ (click), ‘u’ (survey).
|   | `Http500` if field_id is null and event_type is one of: ‘d’ (date), ‘r’ (recurring date).

## Get trigger by ID

> GET /:account_id/triggers/100

```json
{
  "parent_mailing": {
    "mailing_type": "m", 
    "send_started": null, 
    "signup_form_id": null, 
    "mailing_id": 200, 
    "plaintext": "Hello [% member:first_name %]!", 
    "recipient_count": 0, 
    "cancel_ts": null, 
    "created_ts": "@D:2013-08-22T09:41:45", 
    "month": null, 
    "failure_ts": null, 
    "year": null, 
    "datacenter": null, 
    "started_or_finished": null, 
    "account_id": 100, 
    "disabled": false, 
    "mailing_status": "c", 
    "plaintext_only": false, 
    "sender": "Kevin McConnell", 
    "parent_mailing_id": null, 
    "failure_message": null, 
    "name": "Sample Mailing", 
    "send_finished": null, 
    "cancel_by_user_id": null, 
    "send_at": null, 
    "reply_to": null, 
    "subject": "Sample Mailing for [% member:first_name %] [% member:last_name %]", 
    "purged_at": null, 
    "archived_ts": null, 
    "html_body": "<p>Hello [% member:first_name %]!</p>"
  }, 
  "surveys": null, 
  "event_type": "r", 
  "links": null, 
  "field_id": 203, 
  "signup_integrations": null, 
  "push_offset_units": "0:-14:0:0", 
  "start_ts": "@D:2013-08-22T09:41:45", 
  "trigger_id": 100, 
  "name": "Birthday triggers", 
  "signups": null, 
  "push_offset": "@I:-1209600.0", 
  "groups": null, 
  "parent_mailing_id": 200, 
  "deleted_at": null, 
  "is_disabled": false, 
  "account_id": 100
}
````

Look up a trigger by trigger id.

|   |   |
|---|---|
| **Returns**: | A trigger.

|   |   |
|---|---|
| **Raises**: | `Http404` if no trigger is found.

## Update a trigger

> PUT /:account_id/triggers/100

```json
{
  "name": "A Better Trigger"
}

100
```

Update or edit a trigger.

|   |   |
|---|---|
| **Returns**: | The id of the updated trigger.

|   |   |
|---|---|
| **Raises**: | `Http404` if no trigger is found.

## Delete a trigger

> DELETE /:account_id/triggers/100

```json
true
```

Delete a trigger.

|   |   |
|---|---|
| **Returns**: | True if the trigger is deleted.

|   |   |
|---|---|
| **Raises**: | `Http404` if no trigger is found.

## List mailings sent by a trigger

> GET /:account_id/triggers/101/mailings

```json
[
  {
    "mailing_type": "m",
    "send_started": null,
    "signup_form_id": null,
    "mailing_id": 200,
    "plaintext": "Hello [% member:first_name %]!",
    "recipient_count": 0,
    "cancel_ts": null,
    "created_ts": "@D:2013-08-22T09:41:45",
    "month": null,
    "failure_ts": null,
    "year": null,
    "datacenter": null,
    "started_or_finished": null,
    "account_id": 100,
    "disabled": false,
    "mailing_status": "c",
    "plaintext_only": false,
    "sender": "Kevin McConnell",
    "parent_mailing_id": null,
    "failure_message": null,
    "name": "Sample Mailing",
    "send_finished": null,
    "cancel_by_user_id": null,
    "send_at": null,
    "reply_to": null,
    "subject": "Sample Mailing for [% member:first_name %] [% member:last_name %]",
    "purged_at": null,
    "archived_ts": null,
    "html_body": "<p>Hello [% member:first_name %]!</p>"
  }
]
```

Get mailings sent by a trigger.

|   |   |
|---|---|
| **Returns**: | An array of mailings.

|   |   |
|---|---|
| **Raises**: | `Http404` if no trigger is found.