---
title: API Reference

#language_tabs:
 # - php
  #- python
  #- javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

#includes:
 # - errors

search: true
---

# Introduction

## Welcome to the Emma API!

### A Word To The Wise...

Use the documentation here to set up and troubleshoot your API calls. We don’t offer one-on-one support for Emma’s public API, but our system is monitored around the clock. If there’s ever a dropped connection or outage, we’ll notify users on our status page.

### Overview

Emma’s platform is accessible through our public API. The API provides access to the following areas:

1. Managing member lists
    - importing, editing, deleting
    - organizing members into groups
    - searching for members
2. Mailings
    - viewing past mailings
    - controlling the status of pending mailings
3. Retrieving response
    - Access mailing response information by mailing, member of time period
    - Access response at summary and detail levels

## API Wrappers

Emma’s wrappers make it easy to connect to our API in the programming language you’re working in. Just select your language to integrate with Emma without having to study all the ins and outs of our API first.

This collection is young, but growing, and includes wrappers commissioned by Emma, as well as those built by members of our community. Unless otherwise noted, all wrappers provide full coverage of our current API. If you see something that needs to be fixed or improved, please don’t hesitate to fork the repo and submit a pull request. And if you’ve built a wrapper that you would like listed here, let us know!

### PHP
[EmmaPHP](https://github.com/myemma/EmmaPHP): Commissioned by Emma and built by Nashville developer Dennis Monsewicz.

[OhMyEmma](https://github.com/jwoodcock/OhMyEmma): Built by Nashville developer Jacques Woodcock.

[Emma](https://github.com/markroland/emma): Built by Nashville-based Abenity, Inc.

### Python
[EmmaPython](https://github.com/myemma/EmmaPython): Built by Emma’s own Doug Hurst.

### Ruby
[EmmaRuby](https://github.com/myemma/EmmaRuby): Commissioned by Emma and built by Nashville developer Dennis Monsewicz.

### Objective-C
[EmmaSDK](https://github.com/myemma/EmmaSDK): Commissioned by Emma and built by Portland developer Benjamin Van Der Veen.

### Node.js
[emma-sdk](https://github.com/nathanpeck/emma-sdk): Built by New York City developer Nathan Peck for StoryDesk.

# Quickstart

## Authentication

> So, let’s add these authentication bits to our code.

```php
// Authentication Variables
$account_id = "xxx";
$public_api_key = "xxx";
$private_api_key = "xxx";
```

Let’s start by building some PHP code that will add a member to our audience.

First, you&rsquo;ll want to figure out how to authenticate. All calls require HTTP Basic Auth authentication. Inside your Emma account, in the “Account Settings” section, you’ll find your API keys. There’s a public key and a private key and an account ID. It should look like this.

![./images/api_keys.jpg](./images/api_keys.jpg)

## Capture variables

```php
// Form variable(s)
$email = $_POST['email'];
$first_name = $_POST['first_name'];
$last_name = $_POST['last_name'];
$groups = array(123456, 654321);

// Member data other than email should be passed in an array called "fields"
$member_data = array(
  "email" => $email,
  "fields" => array(
    "first_name" => $first_name,
    "last_name" => $last_name
  ),
  "group_ids" => $groups
);
```

Then, we’ll want to capture the POSTed form variables from the request.

Please note that the `group_ids` to which the member will be added are passed as an array of integers. Of course, you’ll want to make sure you sanitize any of these form inputs to protect your application.

Let’s set the URL for the API call. The endpoint for all of our API calls is https://api.e2ma.net/. In this case, we’re using the endpoint that corresponds to adding a single member. There’s a separate call for adding bulk members.

## Set URL

```php
// Set URL
$url = "https://api.e2ma.net/".$account_id."/members/add";
```

## Execute cURL command

```php
// setup and execute the cURL command
$ch = curl_init();
curl_setopt($ch, CURLOPT_USERPWD, $public_api_key . ":" . $private_api_key);
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_POST, count($member_data));
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($member_data));
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-type: application/json'));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
$head = curl_exec($ch);
$http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);
```

Next, we’ll do a little dance with cURL that I’m sure is familiar. I’ll explain some particulars below the code block.

On line 3 is where we use the public/private keys to authenticate our call. On lines 6-7, we force the API call to make its request in JSON and expect JSON in return.

## Results

```php
//execute post
if($http_code > 200) {
  $app_message = "Error sending subscription request";
} else {
  $app_message = "Success!";
}

echo $app_message;
```

At this point, the API call has been made.We might want to inspect the result to see what happened. The API returns true HTTP response codes which you can examine to determine a request’s disposition.

You can [see the complete PHP script here](http://api.myemma.com/php_signup_example.html). Additionally, there’s a PHP example to [get members using their email address](http://api.myemma.com/php_get_member_example.html).

To extend this example further, one could make the API call associate the new member with a particular signup form. As written above, the member would be added to the audience but would not appear in the “recent activity” type searches or really any searches related to signups. We are working on a cleaner way to handle this signup form issue in the future.

# Fields

The API calls are organized by their main entity type in the following sections. Each call includes sample request & response data.

### GET /#account_id/fields

> GET /100/fields

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

## GET /#account_id/fields/#field_id

> GET /100/fields/200

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

## POST /#account_id/fields

> POST /100/fields

```json
{
  "shortcut_name": "new_boolean", 
  "column_order": 3, 
  "display_name": "A New Boolean Field", 
  "field_type": "boolean"
}

1024
```

Create a new field field.

There must not already be a field with this name.

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

## DELETE /#account_id/fields/#field_id

> DELETE /100/fields/200

```json
true
```

Deletes a field.

|   |   |
|---|---|
| **Returns**: | `True` if the field is deleted, False otherwise.

## POST /#account_id/fields/#field_id/clear

> POST /100/fields/200/clear

```json
{}

true
```

Clear the member data for the specified field.

|   |   |
|---|---|
| **Returns**: | `True` if all of the member field data is deleted

## PUT /#account_id/fields/#field_id

> PUT /100/fields/202

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

# Groups

## GET /#account_id/groups

> GET /100/groups?group_types=g,t

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

> GET /100/groups?group_types=all

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

## POST /#account_id/groups

> POST /100/groups

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

## GET /#account_id/groups/#member_group_id

> GET /100/groups/150

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

## PUT /#account_id/groups/#member_group_id

> PUT /100/groups/150

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

## DELETE /#account_id/groups/#member_group_id

> DELETE /100/groups/150

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

## GET /#account_id/groups/#member_group_id/members

> GET /100/groups/150/members

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

## PUT /#account_id/groups/#member_group_id/members

> PUT /100/groups/150/members

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

## PUT /#account_id/groups/#member_group_id/members/remove

> PUT /100/groups/150/members/remove

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

## DELETE /#account_id/groups/#member_group_id/members

> DELETE /100/groups/151/members

```json
2
```

> DELETE /100/groups/151/members?member_status_id=a

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

## DELETE /#account_id/groups/#member_group_id/members/remove

> DELETE /100/groups/151/members/remove?member_status_id=a

```json
true
```

> DELETE /100/groups/151/members/remove?member_status_id=a&delete_members=true

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

## PUT /#account_id/groups/#from_group_id/#to_group_id/members/copy

> PUT /100/groups/151/152/members/copy

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

# Members

In addition to the various CRUD endpoints here related to members, you can also change the status of members, including opting them out.

You’ll notice that there are calls related to individual members, but we also provide quite a few calls to deal with bulk updates of members. Please try to use these whenever possible as opposed to looping through a list of members and calling the individual member calls.

Where this is especially important is when adding new members. To do a bulk import, you’ll POST to the `/#account_id/members` endpoint. In return, you’ll receive an import ID. You can use this ID to check the status and results of your import. Imports are generally pretty fast, but the time to completion can vary with greater system usage.

## GET /#account_id/members

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

## GET /#account_id/members/#member_id

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

## GET /#account_id/members/email/:email

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

## GET /#account_id/members/#member_id/optout

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

## PUT /#account_id/members/email/optout/:email

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

## POST /#account_id/members

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

## POST /#account_id/members/add

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

## POST /#account_id/members/signup

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

## PUT /#account_id/members/delete

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


## PUT /#account_id/members/status

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

## PUT /#account_id/members/#member_id

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

## DELETE /#account_id/members/#member_id

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

## GET /#account_id/members/#member_id/groups

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

## PUT /#account_id/members/#member_id/groups

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

## PUT /#account_id/members/#member_id/groups/remove

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

## DELETE /#account_id/members

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

## DELETE /#account_id/members/#member_id/groups

> DELETE /100/members/200/groups

```json
true
```

Remove the specified member from all groups.

|   |   |
|---|---|
| **Raises**: | `True` if the member is removed from all groups.
|   | `Http404` if no member is found.

## PUT /#account_id/members/groups/remove

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

## GET /#account_id/members/#member_id/mailings

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

## GET /#account_id/members/imports/#import_id/members

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

## GET /#account_id/members/imports/#import_id

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

## GET /#account_id/members/imports

> GET /100/members/imports

```json
[]
```

Get information about all imports for this account.

|   |   |
|---|---|
| **Raises**: | An array of import details.

## DELETE /#account_id/members/imports/delete

Update an import record to be marked as ‘deleted’.

|   |   |
|---|---|
| **Raises**: | `True` if the import is marked as deleted.

|   |   |
|---|---|
| **Raises**: | `Http404` if the import record does not exist

## PUT /#account_id/members/#group_id/copy

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

## PUT /#account_id/members/status/:status_from/to/:status_to

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

# Response

We know that you want to do some fancy pivot tables with your response data, so we’ve provided quite a few endpoints here to give you access to that response data. You can get overview numbers for all of your mailings and also drill down into finding out the actual members who opened a particular mailing.

## GET /#account_id/response

> GET /100/response

```json
[]
```

> GET /100/response?range=2011-04-01~

```json
[]
```

> GET /100/response?range=2011-04-01~2011-09-01

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

## GET /#account_id/response/#mailing_id

> GET /100/response/200

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

## GET /#account_id/response/#mailing_id/sends

> GET /100/response/200/sends

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

## GET /#account_id/response/#mailing_id/in_progress

> GET /100/response/200/in_progress

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

## GET /#account_id/response/#mailing_id/deliveries

> GET /100/response/200/deliveries

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

## GET /#account_id/response/#mailing_id/opens

> GET /100/response/200/opens

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

## GET /#account_id/response/#mailing_id/links

> GET /100/response/200/links

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

## GET /#account_id/response/#mailing_id/clicks

> GET /100/response/200/clicks

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

## GET /#account_id/response/#mailing_id/forwards

> GET /100/response/200/forwards

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

## GET /#account_id/response/#mailing_id/optouts

> GET /100/response/200/optouts

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

## GET /#account_id/response/#mailing_id/signups

> GET /100/response/200/signups

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

## GET /#account_id/response/#mailing_id/shares

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

## GET /#account_id/response/#mailing_id/customer_shares

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

## GET /#account_id/response/#mailing_id/customer_share_clicks

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

## GET /#account_id/response/#share_id/customer_share

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

## GET /#account_id/response/#mailing_id/shares/overview

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

# Searches

## GET /#account_id/searches

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

## GET /#account_id/searches/#search_id

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

## POST /#account_id/searches

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

## PUT /#account_id/searches/#search_id

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

## DELETE /#account_id/searches/#search_id

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

## GET /#account_id/searches/#search_id/members

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

# Signup Forms

With this endpoint, you can list all of your signup forms.

## GET /#account_id/signup_forms

> GET /100/signup_forms

```json
[
  {
    "id": 200,
    "name": "Your Default Signup Form"
  },
  {
    "id": 201,
    "name": "My First Signup Form"
  }

]
```

Gets a list of this account’s signup forms.

|   |   |
|---|---|
| **Returns**: | An array of signup forms.


# Triggers

These endpoints provide CRUD operations for our trigger system. Creating a trigger is probably the most widely used of these operations.

## GET /#account_id/triggers

> GET /100/triggers

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

## POST /#account_id/triggers

> POST /100/triggers

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

## GET /#account_id/triggers/#trigger_id

> GET /100/triggers/100

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

## PUT /#account_id/triggers/#trigger_id

> PUT /100/triggers/100

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

## DELETE /#account_id/triggers/#trigger_id

> DELETE /100/triggers/100

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

## GET /#account_id/triggers/#trigger_id/mailings

> GET /100/triggers/101/mailings

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