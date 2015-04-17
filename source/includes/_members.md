# Members

In addition to the various CRUD endpoints here related to members, you can also change the status of members, including opting them out.

You’ll notice that there are calls related to individual members, but we also provide quite a few calls to deal with bulk updates of members. Please try to use these whenever possible as opposed to looping through a list of members and calling the individual member calls.

Where this is especially important is when adding new members. To do a bulk import, you’ll POST to the `/#account_id/members` endpoint. In return, you’ll receive an import ID. You can use this ID to check the status and results of your import. Imports are generally pretty fast, but the time to completion can vary with greater system usage.

## List all members

> GET /100/members?start=0&end=2

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
  },
  {
    "status": "opt-out",
    "confirmed_opt_in": null,
    "account_id": 100,
    "fields": {
      "first_name": "Gladys",
      "last_name": "Jones",
      "favorite_food": "toast"
    },
    "member_id": 201,
    "last_modified_at": null,
    "member_status_id": "o",
    "plaintext_preferred": false,
    "email_error": null,
    "member_since": "@D:2011-01-03T15:54:13",
    "bounce_count": 0,
    "deleted_at": null,
    "email": "gladys@myemma.com"
  }
]
```

> GET /100/members?start=2&end=4&deleted=true

```json
[
  {
    "status": "active",
    "confirmed_opt_in": null,
    "account_id": 100,
    "fields": {
      "first_name": "Tony",
      "last_name": "Brown",
      "favorite_food": "pizza"
    },
    "member_id": 202,
    "last_modified_at": null,
    "member_status_id": "a",
    "plaintext_preferred": false,
    "email_error": null,
    "member_since": "@D:2010-11-12T09:12:33",
    "bounce_count": 0,
    "deleted_at": null,
    "email": "tony@myemma.com"
  },
  {
    "status": "active",
    "confirmed_opt_in": null,
    "account_id": 100,
    "fields": {

    },
    "member_id": 203,
    "last_modified_at": null,
    "member_status_id": "a",
    "plaintext_preferred": false,
    "email_error": null,
    "member_since": "@D:2011-01-01T00:00:00",
    "bounce_count": 0,
    "deleted_at": "@D:2011-01-02T10:27:43",
    "email": "greg@myemma.com"
  }
]
```

Get a basic listing of all members in an account.

|   |   |
|---|---|
| **Parameters**: | `deleted` (boolean) – Accepts True or 1. Optional flag to include deleted members.

|   |   |
|---|---|
| **Returns**: | A list of members in the given account.

## Get member by ID

> GET /100/members/201

```json
{
  "status": "opt-out", 
  "confirmed_opt_in": null, 
  "account_id": 100, 
  "fields": {
    "first_name": "Gladys", 
    "last_name": "Jones", 
    "favorite_food": "toast"
  }, 
  "member_id": 201, 
  "last_modified_at": null, 
  "member_status_id": "o", 
  "plaintext_preferred": false, 
  "email_error": null, 
  "member_since": "@D:2011-01-03T15:54:13", 
  "bounce_count": 0, 
  "deleted_at": null, 
  "email": "gladys@myemma.com"
}
```

Get detailed information on a particular member, including all custom fields.

|   |   |
|---|---|
| **Parameters**: | `deleted` (boolean) – Accepts True or 1. Optional flag to include deleted members.

|   |   |
|---|---|
| **Returns**: | A single member if one exists.

|   |   |
|---|---|
| **Raises**: | `Http404` if no member is found.

## Get member by email address

> GET /100/members/email/tony@myemma.com

```json
{
  "status": "active", 
  "confirmed_opt_in": null, 
  "account_id": 100, 
  "fields": {
    "first_name": "Tony", 
    "last_name": "Brown", 
    "favorite_food": "pizza"
  }, 
  "member_id": 202, 
  "last_modified_at": null, 
  "member_status_id": "a", 
  "plaintext_preferred": false, 
  "email_error": null, 
  "member_since": "@D:2010-11-12T09:12:33", 
  "bounce_count": 0, 
  "deleted_at": null, 
  "email": "tony@myemma.com"
}
```

Get detailed information on a particular member, including all custom fields, by email address instead of ID.

|   |   |
|---|---|
| **Parameters**: | `deleted` (boolean) – Accepts True or 1. Optional flag to include deleted members.

|   |   |
|---|---|
| **Returns**: | A single member if one exists.

|   |   |
|---|---|
| **Raises**: | `Http404` if no member is found.

## Get member opt out details by ID

> GET /100/members/201/optout

```json
[]
```

If a member has been opted out, returns the details of their optout, specifically date and `mailing_id`.

|   |   |
|---|---|
| **Returns**: | Member opt out date and mailing if member is opted out.

|   |   |
|---|---|
| **Raises**: | `Http404` if no member is found.

## Get member opt out details by email address

> PUT /100/members/email/optout/tony@myemma.com

```json
{}

true
```

Update a member’s status to optout keyed on email address instead of an ID.

|   |   |
|---|---|
| **Returns**: | `True` if member status change was successful or member was already opted out.

|   |   |
|---|---|
| **Raises**: | `Http404` if no member is found.

## Add new/Update existing members

> POST /100/members

```json
{
  "members": [
    {
      "email": "michelle@myemma.com"
    }, 
    {
      "email": "nathan@myemma.com"
    }
  ]
}

{
  "import_id": 1024
}
```

Add new members or update existing members in bulk. If you are doing actions for a single member please see the /members/add call below.

|   |   |
|---|---|
| **Parameters**: | `members` (array) – An array of members to update
|   | A member is a dictionary of member emails and field values to import. The only required field is "email". All other fields are treated as the name of a member field.
|   | `source_filename` (string) – An arbitrary string to associate with this import.
|   | This should generally be set to the filename that the user uploaded.
|   | `add_only` (boolean) – Optional. Only add new members, ignore existing members.
|   | `group_ids` (array of integers) – Optional. Add imported members to this list of groups.

|   |   |
|---|---|
| **Returns**: | An import id

## Add new/Update existing single member

> POST /100/members/add

```json
{
  "fields": {
    "first_name": "Benjamin"
  }, 
  "email": "benjamin@myemma.com"
}

{
  "status": "a", 
  "added": true, 
  "member_id": 1024
}
```

Adds or updates a single audience member. If you are performing actions on bulk members please use the /members call above.

Note: Members who are added to your audience using this API call will
not appear in "recent activity" searches within your account.

|   |   |
|---|---|
| **Parameters**: | `email` (string) – Email address of member to add or update
|   | `fields` (dictionary) – Names and values of user-defined fields to update
|   | `group_ids` (array of integers) – Optional. Add imported members to this list of groups.
|   | `field_triggers` (boolean) – Optional. Fires related field change autoresponders when set to true.

|   |   |
|---|---|
| **Returns**: | The `member_id` of the new or updated member, whether the member was added or an existing member was updated, and the status of the member. The status will be reported as ‘a’ (active), ‘e’ (error), or ‘o’ (optout).

## Add new member to a group

> POST /100/members/signup

```json
{
  "fields": {
    "first_name": "Ryan"
  }, 
  "group_ids": [
    "1", 
    "2"
  ], 
  "email": "example@myemma.com", 
  "signup_form_id": 1
}

{
  "status": "a", 
  "member_id": 1024
}
```

Takes the necessary actions to signup a member and enlist them in the provided group ids. You can send the same member multiple times and pass in new group ids to signup.

This process triggers the opt-out workflow, and will send a mailing to the member on new group enlistments. If no new group ids are provided for an existing member, the endpoint will respond back with their status and member_id, performing no additional actions.

Note: Members who are added to your audience using this API call will
not appear in "recent activity" searches within your account.

|   |   |
|---|---|
| **Parameters**: | `email` (string) – Email address of the member to sign-up.
|   | `group_ids` (Array of strings (group_ids)) – An array of group ids to associate sign-up with.
|   | `fields` (dictionary) – Optional. Names and values of user-defined fields to update.
|   | `signup_form_id` (integer) – Optional. Indicate that this member used a particular signup form. 
|   | This is important if you have custom mailings for a particular signup form and so that signup-based triggers will be fired.
|   | `opt_in_subject` (string) – Optional. Override the confirmation message subject with your own copy.
|   | `opt_in_message` (string) – Optional. Override the confirmation message body with your own copy. |   | Must include the following tags: [rsvp_name], [rsvp_email], [opt_in_url], [opt_out_url].
|   | `field_triggers` (boolean) – Optional. Fires related field change autoresponders when set to true.

|   |   |
|---|---|
| **Returns**: | The `member_id` of the member, and their status. The status will be reported as ‘a’ (active), ‘e’ (error), or ‘o’ (optout).

## Delete group of members

> PUT /100/members/delete

```json
{
  "member_ids": [
    100, 
    101
  ]
}

true
```

Delete an array of members.

The members will be marked as deleted and cannot be retrieved.

|   |   |
|---|---|
| **Parameters**: | `member_ids` (array) – An array of member ids to delete.

|   |   |
|---|---|
| **Returns**: | `True` if all members are successfully deleted, otherwise False.


## Change group of member's status

> PUT /100/members/status

```json
{
  "member_ids": [
    200, 
    202
  ], 
  "status_to": "e"
}

true
```

Change the status for an array of members.

The members will have their member_status_id updated.

|   |   |
|---|---|
| **Parameters**: | `member_ids` (array) – The array of member ids to change.
|   | `status_to` (string) – The new status for the given members. Accepts one of ‘a’ (active), ‘e’ (error), ‘o’ (optout).

|   |   |
|---|---|
| **Raises**: | `True` if the members are successfully updated, otherwise False.

## Update single member information

> PUT /100/members/200

```json
{
  "fields": {
    "first_name": "Ella", 
    "last_name": "Jones"
  }, 
  "email": "ella@myemma.com", 
  "status_to": "a"
}

true
```

Update a single member’s information.

Update the information for an existing member (even if they are marked as deleted). Note that this method allows the email address to be updated (which cannot be done with a POST, since in that case the email address is used to identify the member).

|   |   |
|---|---|
| **Parameters**: | `email` (string) – A new email address for the member.
|   | `status_to` (string) – A new status for the member. Accepts one of ‘a’ (active), ‘e’ (error), ‘o’ (opt-out).
|   | `fields` (array) – An array of fields with associated values for this member.
|   | `field_triggers` (boolean) – Optional. Fires related field change autoresponders when set to true.

|   |   |
|---|---|
| **Raises**: | `True` if the member was updated successfully
|   | `Http404` if no member is found.

## Delete single member

> DELETE /100/members/202

```json
true
```

Delete the specified member.

The member, along with any associated response and history information, will be completely removed from the database.

|   |   |
|---|---|
| **Raises**: | `True` if the member is deleted.
|   | `Http404` if no member is found.

## List member's groups

> GET /100/members/200/groups

```json
[
  {
    "active_count": 1,
    "deleted_at": null,
    "error_count": 0,
    "optout_count": 1,
    "group_type": "g",
    "member_group_id": 150,
    "purged_at": null,
    "account_id": 100,
    "group_name": "Monthly Newsletter"
  },
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
  },
  {
    "active_count": 4,
    "deleted_at": null,
    "error_count": 0,
    "optout_count": 0,
    "group_type": "g",
    "member_group_id": 152,
    "purged_at": null,
    "account_id": 100,
    "group_name": "Special Events"
  }
]
```

Get the groups to which a member belongs.

|   |   |
|---|---|
| **Raises**: | An array of groups.
|   | `Http404` if no member is found.

## Add single member to group(s)

> PUT /100/members/200/groups

```json
{
  "group_ids": [
    150, 
    151
  ]
}

[
  150, 
  151
]
```

Add a single member to one or more groups.

|   |   |
|---|---|
| **Parameters**: | `group_ids` (array) – Group ids to which to add this member.

|   |   |
|---|---|
| **Raises**: | An array of ids of the affected groups.
|   | `Http404` if no member is found.

## Remove a single member from group(s)

> PUT /100/members/200/groups/remove

```json
{
  "group_ids": [
    151
  ]
}

[
  151
]
```

Remove a single member from one or more groups.

|   |   |
|---|---|
| **Parameters**: | `group_ids` (array) – Group ids from which to remove this member.

|   |   |
|---|---|
| **Raises**: | An array of references to the affected groups.

|   |   |
|---|---|
| **Raises**: | `Http404` if no member is found.

## Delete all members

> DELETE /100/members?member_status_id=a

```json
true
```

Delete all members.

|   |   |
|---|---|
| **Parameters**: | `member_status_id` (string) – This is ‘a’ (active), ‘o’ (optout), or ‘e’ (error).

|   |   |
|---|---|
| **Raises**: | `True`.

## Remove single member from all groups

> DELETE /100/members/200/groups

```json
true
```

Remove the specified member from all groups.

|   |   |
|---|---|
| **Raises**: | `True` if the member is removed from all groups.
|   | `Http404` if no member is found.

## Remove multiple members from groups

> PUT /100/members/groups/remove

```json
{
  "group_ids": [
    151
  ], 
  "member_ids": [
    202
  ]
}

true
```

Remove multiple members from groups.

|   |   |
|---|---|
| **Parameters**: | `member_ids` (array) – Member ids to remove from the given groups.
|   | `group_ids` (array) – Group ids from which to remove the given members.

|   |   |
|---|---|
| **Raises**: | `True` if the members are deleted, otherwise False.
|   | `Http404` if any of the members or groups do not exist

## Get entire mailing history for member

> GET /100/members/200/mailings

```json
[
  {
    "delivery_type": "d",
    "clicked": "@D:2011-01-02T11:14:32",
    "opened": "@D:2011-01-02T11:13:51",
    "mailing_id": 200,
    "delivery_ts": "@D:2011-01-02T10:29:36",
    "name": "Sample Mailing",
    "forwarded": null,
    "shared": null,
    "subject": "Sample Mailing for [% member:first_name %] [% member:last_name %]",
    "sent": "@D:2011-01-02T10:27:43",
    "account_id": 100
  }
]
```

Get the entire mailing history for a member.

|   |   |
|---|---|
| **Raises**: | Message history details for the specified member.

## List members of a specific import

> GET /100/members/imports/200/members

```json
[
  {
    "member_id": 200,
    "change_type": "a",
    "member_status_id": "a",
    "email": "emma@myemma.com"
  }
]
```

Get a list of members affected by this import.

|   |   |
|---|---|
| **Raises**: | A list of members in the given account and import.

## Get single import information

> GET /100/members/imports/200

```json
{
  "import_id": 200, 
  "status": null, 
  "style": null, 
  "import_started": "@D:2010-12-13T23:12:44", 
  "account_id": 100, 
  "error_message": null, 
  "num_members_updated": 0, 
  "source_filename": null, 
  "fields_updated": [], 
  "num_members_added": 0, 
  "import_finished": null, 
  "groups_updated": [], 
  "num_skipped": 0, 
  "num_duplicates": 0
}
```

Get information and statistics about this import.

|   |   |
|---|---|
| **Raises**: | Import details for the given `import_id`.

## Get all imports information

> GET /100/members/imports

```json
[]
```

Get information about all imports for this account.

|   |   |
|---|---|
| **Raises**: | An array of import details.

## Update import status to deleted

Update an import record to be marked as ‘deleted’.

|   |   |
|---|---|
| **Raises**: | `True` if the import is marked as deleted.

|   |   |
|---|---|
| **Raises**: | `Http404` if the import record does not exist

## Copy all account members of one or more statuses into a group

> PUT /100/members/152/copy

```json
{
  "member_status_id": [
    "a", 
    "e"
  ]
}

true
```

Copy all account members of one or more statuses into a group.

|   |   |
|---|---|
| **Parameters**: | `member_status_id` (array of strings) – ‘a’ (active), ‘o’ (optout), and/or ‘e’ (error).

|   |   |
|---|---|
| **Raises**: | `True`

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.

## Update the status for a group of members

> PUT /100/members/status/e/to/a

```json
{
  "group_id": 1
}

true
```

Update the status for a group of members, based on their current
status. Valid statuses id are (‘a’,’e’, ‘f’, ‘o’) active, error, forwarded, optout.

|   |   |
|---|---|
| **Parameters**: | `group_id` – Optional. Limit the update to members of the specified group

|   |   |
|---|---|
| **Raises**: | `True`

|   |   |
|---|---|
| **Raises**: | `Http400` if the specified status is invalid