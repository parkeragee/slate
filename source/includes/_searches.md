# Searches

## Get saved searches

> GET /100/searches

```json
[
  {
    "search_id": 200,
    "optout_count": 0,
    "error_count": 0,
    "name": "Test Search",
    "criteria": "[\"or\", [\"group\", \"eq\", \"Monthly Newsletter\"],[\"group\", \"eq\", \"Widget Buyers\"]]",
    "deleted_at": null,
    "purged_at": null,
    "last_run_at": null,
    "active_count": 0,
    "account_id": 100
  },
  {
    "search_id": 201,
    "optout_count": 0,
    "error_count": 0,
    "name": "Second Test Search",
    "criteria": "[\"group\", \"eq\", \"Special Events\"]",
    "deleted_at": null,
    "purged_at": null,
    "last_run_at": null,
    "active_count": 0,
    "account_id": 100
  }
]
```

Retrieve a list of saved searches.

|   |   |
|---|---|
| **Parameters**: | `deleted` (boolean) – Accepts True or 1. Optional flag to include deleted searches.

|   |   |
|---|---|
| **Returns**: | An array of searches.

## Get saved search information

> GET /100/searches/200

```json
{
  "search_id": 200, 
  "optout_count": 0, 
  "error_count": 0, 
  "name": "Test Search", 
  "criteria": "[\"or\", [\"group\", \"eq\", \"Monthly Newsletter\"],[\"group\", \"eq\", \"Widget Buyers\"]]", 
  "deleted_at": null, 
  "purged_at": null, 
  "last_run_at": null, 
  "active_count": 0, 
  "account_id": 100
}
```

Get the details for a saved search.

|   |   |
|---|---|
| **Parameters**: | `deleted` (boolean) – Accepts True or 1. Optional flag to include deleted searches.

|   |   |
|---|---|
| **Returns**: | A search.

|   |   |
|---|---|
| **Raises**: | `Http404` if the search does not exist.

## Create a saved search

```json
["and",
    ["or",
        ["group", "eq", "my list"],
        ["group", "contains", "old"]
    ],
    ["not", ["member_field", "soup", "eq", "lentil"]],
    ["opened", 123249, "between", "2011-01-22, 2011-01-31"],
    ["clicked", 83927]
]
```
> POST /100/searches

```json
{
  "name": "New Search", 
  "criteria": [
    "or", 
    [
      "group", 
      "eq", 
      "Monthly Newsletter"
    ], 
    [
      "group", 
      "eq", 
      "Widget Buyers"
    ]
  ]
}

1024
```

Create a saved search.

The detail of a search is specified in a JSON structure that describes the clauses to be applied using groups of filter type, operator and value. Where the filter type is `member_field`, the referenced field should be specified by joining with a colon. For example:

The following parameters are required:

|   |   |
|---|---|
| **Parameters**: | `criteria` (array) – A combination of search conditions, as described above.
|   | `name` (string) – A name used to describe this search.

|   |   |
|---|---|
| **Returns**: | The ID of the new search

|   |   |
|---|---|
| **Raises**: | `Http400` if the search is invalid

## Update a saved search

> PUT /100/searches/200

```json
{
  "criteria": [
    "or", 
    [
      "group", 
      "eq", 
      "Monthly Newsletter"
    ], 
    [
      "group", 
      "eq", 
      "Special Events"
    ]
  ]
}

true
```

Update a saved search.

No parameters are required, but either the name or criteria parameter must be present for an update to occur.

|   |   |
|---|---|
| **Parameters**: | `criteria` (array) – A combination of search conditions, as described above (see create_search).
|   | `name` (string) – A name used to describe this search.

|   |   |
|---|---|
| **Returns**: | `True` if the update was successful

|   |   |
|---|---|
| **Raises**: | `Http404` if the search does not exist.
|   | `Http400` if the search criteria is invalid

## Delete a saved search

> DELETE /100/searches/200

```json
true
```

Delete a saved search. The member records referred to by the search are not affected.

|   |   |
|---|---|
| **Returns**: | `True` if the search is deleted.

|   |   |
|---|---|
| **Raises**: | `Http404` if the search does not exist.

## List members matching a search

> GET /100/searches/201/members

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
  },
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
    "member_id": 204,
    "last_modified_at": null,
    "member_status_id": "a",
    "plaintext_preferred": false,
    "email_error": null,
    "member_since": "@D:2010-12-13T23:12:44",
    "bounce_count": 0,
    "deleted_at": null,
    "email": "bob@myemma.com"
  }
]
```

Get the members matching the search.

|   |   |
|---|---|
| **Returns**: | An array of members.

|   |   |
|---|---|
| **Raises**: | `Http404` if the search does not exist.