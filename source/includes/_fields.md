# Fields

The API calls are organized by their main entity type in the following sections. Each call includes sample request & response data.

## List account's fields

> GET /:account_id/fields

```json

[
  {
    "shortcut_name": "first_name",
    "display_name": "First Name",
    "account_id": 100,
    "field_type": "text",
    "required": false,
    "field_id": 200,
    "widget_type": "text",
    "short_display_name": null,
    "column_order": 1,
    "deleted_at": null,
    "options": null
  },
  {
    "shortcut_name": "last_name",
    "display_name": "Last Name",
    "account_id": 100,
    "field_type": "text",
    "required": false,
    "field_id": 201,
    "widget_type": "text",
    "short_display_name": null,
    "column_order": 2,
    "deleted_at": null,
    "options": null
  },
  {
    "shortcut_name": "favorite_food",
    "display_name": "Favorite Food",
    "account_id": 100,
    "field_type": "text",
    "required": false,
    "field_id": 202,
    "widget_type": "text",
    "short_display_name": null,
    "column_order": 3,
    "deleted_at": null,
    "options": null
  },
  {
    "shortcut_name": "birthday",
    "display_name": "Birthday",
    "account_id": 100,
    "field_type": "date",
    "required": false,
    "field_id": 203,
    "widget_type": "date",
    "short_display_name": null,
    "column_order": 4,
    "deleted_at": null,
    "options": null
  }
]
```
Gets a list of this account’s defined fields.

|   |   |
|---|---|
| **Parameters**: | `deleted` (boolean) – Accepts True or 1. Optional flag to include deleted fields.

|   |   |
|---|---|
| **Returns**: | An array of fields.

## Get field information

> GET /:account_id/fields/:field_id

```json

{
  "shortcut_name": "first_name", 
  "display_name": "First Name", 
  "account_id": 100, 
  "field_type": "text", 
  "required": false, 
  "field_id": 200, 
  "widget_type": "text", 
  "short_display_name": null, 
  "column_order": 1, 
  "deleted_at": null, 
  "options": null
}
```

Gets the detailed information about a particular field.

|   |   |
|---|---|
| **Parameters**: | `deleted` (boolean) – Accepts True or 1. Optionally show a field even if it has been deleted.

|   |   |
|---|---|
| **Returns**: | A field.

|   |   |
|---|---|
| **Raises**: |  `Http404` if the field does not exist.

## Create new field

> POST /:account_id/fields

```json
{
  "shortcut_name": "new_boolean", 
  "column_order": 3, 
  "display_name": "A New Boolean Field", 
  "field_type": "boolean"
}

1024
```

Create a new field.

_There must not already be a field with this name._

|   |   |
|---|---|
| **Parameters**: | `shortcut_name` (string) – The internal name for this field.
|   | `display_name` (string) – Display name, used for forms and reports.
|   | `field_type` (string) – The type of value this field will contain. Accepts one of text, text[], numeric, boolean, date, timestamp.
|   | `widget_type` (string) – The type of widget this field will display as. Accepts one of text, long, checkbox, select multiple, check_multiple, radio, date, select one, number.
|   | `column_order` (integer) – Order of this column in lists.

|   |   |
|---|---|
| **Returns**: | A reference to the new field.

## Delete field

> DELETE /:account_id/fields/:field_id

```json
true
```

Deletes a field.

|   |   |
|---|---|
| **Returns**: | `True` if the field is deleted, False otherwise.

## Clear field data

> POST /:account_id/fields/:field_id/clear

```json
{}

true
```

Clear the member data for the specified field.

|   |   |
|---|---|
| **Returns**: | `True` if all of the member field data is deleted

## Update field data

> PUT /:account_id/fields/:field_id

```json
{
  "display_name": "Your Birthday"
}

202
```

Updates an existing field.

|   |   |
|---|---|
| **Returns**: | A reference to the updated field.