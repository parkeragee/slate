# Groups

## List all active groups

> GET /:account_id/groups?group_types=g,t

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

> GET /:account_id/groups?group_types=all

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

Get a basic listing of all active member groups for a single account.

|   |   |
|---|---|
| **Returns**: | An array of groups.

|   |   |
|---|---|
| **Parameters**: | `group_types` (string) – Accepts a comma-separated string with one or more of the following group_types: ‘g’ (group), ‘t’ (test), ‘h’ (hidden), ‘all’ (all). Defaults to ‘g’.

## Create new group(s)

> POST /:account_id/groups

```json
{
  "groups": [
    {
      "group_name": "My Grp"
    }
  ]
}

[
  {
    "member_group_id": 1024, 
    "group_name": "My Grp"
  }
]
```

Create one or more new member groups.

|   |   |
|---|---|
| **Returns**: | An array of the new group ids and group names.

|   |   |
|---|---|
| **Parameters**: | `groups` (array) – An array of group objects. Each object must contain a group_name parameter.

## Get single group information

> GET /:account_id/groups/:member_group_id

```json
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
}
```

Get the detailed information for a single member group.

|   |   |
|---|---|
| **Returns**: | A group.

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.

## Update single group information

> PUT /:account_id/groups/:member_group_id

```json
{
  "group_name": "New Group Name"
}

true
```

Update information for a single member group.

|   |   |
|---|---|
| **Parameters**: | `group_name` (string) – Updated group name.

|   |   |
|---|---|
| **Returns**: | `True` if the update was successful

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.

## Delete single group

> DELETE /:account_id/groups/:member_group_id

```json
true
```

Delete a single member group.

|   |   |
|---|---|
| **Returns**: | `True` if the group is deleted.

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.

## Get members in group

> GET /:account_id/groups/:member_group_id/members

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

Get the members in a single active member group.

|   |   |
|---|---|
| **Returns**: | An array of members.

|   |   |
|---|---|
| **Parameters**: | `deleted` (boolean) – include deleted members. Optional, defaults to false.

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.

## Add members to single group

> PUT /:account_id/groups/:member_group_id/members

```json
{
  "member_ids": [
    202
  ]
}

[
  202
]
```

Add a list of members to a single active member group.

|   |   |
|---|---|
| **Parameters**: | `member_ids` (array) – An array of member ids.

|   |   |
|---|---|
| **Returns**: | An array of references to the members added to the group. If a member already exists in the group or is not a valid member, that reference will not be returned.

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.

## Remove members from single group

> PUT /:account_id/groups/:member_group_id/members/remove

```json
{
  "member_ids": [
    200, 
    201
  ]
}

[
  200, 
  201
]
```

Remove members from a single active member group.

|   |   |
|---|---|
| **Parameters**: | `member_ids` (array) – An array of member ids.

|   |   |
|---|---|
| **Returns**: | An array of references to the removed members.

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.

## Delete all members from single group

> DELETE /:account_id/groups/:member_group_id/members

```json
2
```

> DELETE /:account_id/groups/:member_group_id/members?member_status_id=a

```json
2
```

Remove all members from a single active member group.

|   |   |
|---|---|
| **Parameters**: | `member_status_id` (string) – Optional. This is ‘a’ (active), ‘o’ (optout), or ‘e’ (error).

|   |   |
|---|---|
| **Returns**: | Returns the number of members removed from the group.

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.

## Remove all members from all active member groups

> DELETE /:account_id/groups/:member_group_id/members/remove?member_status_id=a

```json
true
```

> DELETE /:account_id/groups/:member_group_id/members/remove?member_status_id=a&delete_members=true

```json
true
```

Remove all members from all active member groups as a background job. The member_status_id parameter must be set.

|   |   |
|---|---|
| **Parameters**: | `member_status_id` (string) – This is ‘a’ (active), ‘o’ (optout), or ‘e’ (error).

|   |   |
|---|---|
| **Returns**: | `True`.

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.

## Copy all members from one group to another

> PUT /:account_id/groups/:member_group_id/:member_group_id/members/copy

```json
{
  "member_status_id": [
    "a"
  ]
}

true
```

Copy all the users of one group into another group.

|   |   |
|---|---|
| **Parameters**: | `member_status_id` (array) – This is ‘a’ (active), ‘o’ (optout), or ‘e’ (error).

|   |   |
|---|---|
| **Returns**: | `True`

|   |   |
|---|---|
| **Raises**: | `Http404` if the group does not exist.