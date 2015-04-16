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

**Parameters**:

`deleted` (boolean) – Accepts True or 1. Optional flag to include deleted fields.

**Returns**: An array of fields.

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

**Parameters**: 

`deleted` (boolean) – Accepts True or 1. Optionally show a field even if it has been deleted.

**Returns**: A field.

**Raises**:  Http404 if the field does not exist.

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

**Parameters**:

`shortcut_name` (string) – The internal name for this field.

`display_name` (string) – Display name, used for forms and reports.

`field_type` (string) – The type of value this field will contain. Accepts one of text, text[], numeric, boolean, date, timestamp.

`widget_type` (string) – The type of widget this field will display as. Accepts one of text, long, checkbox, select multiple, check_multiple, radio, date, select one, number.

`column_order` (integer) – Order of this column in lists.

**Returns**: A reference to the new field.

## DELETE /#account_id/fields/#field_id

> DELETE /100/fields/200

```json
true
```

Deletes a field.

**Returns**: True if the field is deleted, False otherwise.

## POST /#account_id/fields/#field_id/clear

> POST /100/fields/200/clear

```json
{}

true
```

Clear the member data for the specified field.

**Returns**: True if all of the member field data is deleted

## PUT /#account_id/fields/#field_id

> PUT /100/fields/202

```json
{
  "display_name": "Your Birthday"
}

202
```

Updates an existing field.

**Returns**: A reference to the updated field.

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

**Returns**: An array of groups.

**Parameters**: 

`group_types` (string) – Accepts a comma-separated string with one or more of the following group_types: ‘g’ (group), ‘t’ (test), ‘h’ (hidden), ‘all’ (all). Defaults to ‘g’.

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

**Returns**: An array of the new group ids and group names.

**Parameters**: 

`groups` (array) – An array of group objects. Each object must contain a group_name parameter.

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

**Returns**: A group.

**Raises**: Http404 if the group does not exist.

## PUT /#account_id/groups/#member_group_id

> PUT /100/groups/150

```json
{
  "group_name": "New Group Name"
}

true
```

Update information for a single member group.

**Parameters**:

`group_name` (string) – Updated group name.

**Returns**: True if the update was successful

**Raises**: Http404 if the group does not exist.

## DELETE /#account_id/groups/#member_group_id

> DELETE /100/groups/150

```json
true
```

Delete a single member group.

Returns : True if the group is deleted.

Raises :  Http404 if the group does not exist.

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

**Returns**: An array of members.

**Parameters**:

`deleted` (boolean) – include deleted members. Optional, defaults to false.

**Raises**: Http404 if the group does not exist.

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

**Parameters**: 

`member_ids` (array) – An array of member ids.

**Returns**: An array of references to the members added to the group. If a member already exists in the group or is not a valid member, that reference will not be returned.

**Raises**: Http404 if the group does not exist.

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

**Parameters**:

`member_ids` (array) – An array of member ids.

**Returns**: An array of references to the removed members.

**Raises**: Http404 if the group does not exist.

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

**Parameters**:

`member_status_id` (string) – Optional. This is ‘a’ (active), ‘o’ (optout), or ‘e’ (error).

**Returns**: Returns the number of members removed from the group.

**Raises**: Http404 if the group does not exist.

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

**Parameters**:

`member_status_id` (string) – This is ‘a’ (active), ‘o’ (optout), or ‘e’ (error).

**Returns**: Returns true.

**Raises**: Http404 if the group does not exist.

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

**Parameters**: 

`member_status_id` (array) – This is ‘a’ (active), ‘o’ (optout), or ‘e’ (error).

**Returns**: True

**Raises**: Http404 if the group does not exist.

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

**Parameters**:

`include_archived` (boolean) – Optional flag to include archived mailings in the list.

`mailing_types` (string) – Accepts a comma-separated string with one or more of the following mailing types: ‘m’ (standard), ‘t’ (test), ‘r’ (trigger), ‘s’ (split). Defaults to ‘m,t’, standard and test mailings, when none are specified.

`mailing_statuses` (string) – Accepts a comma-separated string with one or more of the following mailing statuses: ‘p’ (pending), ‘a’ (paused), ‘s’ (sending), ‘x’ (canceled), ‘c’ (complete), ‘f’ (failed). Defaults to ‘p,a,s,x,c,f’, all statuses, when none are specified.

`is_scheduled` (boolean) – Mailings that have a scheduled timestamp.

`with_html_body` (boolean) – Include the html_body content.

`with_plaintext` (boolean) – Include the plaintext content.

**Raises**: Http400 if invalid mailing types or statuses are specified.

**Returns**: An array of mailings.

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

**Returns**: A mailing.

**Raises**: Http404 if no mailing is found.

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

**Returns**: An array of members including status and member fields.

**Raises**: Http404 if no mailing is found.

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

**Parameters**: 

`type` (string) – Accepts: ‘all’, ‘html’, ‘plaintext’, ‘subject’. Defaults to ‘all’, if not provided.

**Returns**: Message content from a mailing, personalized for a member. The response will contain all parts of the mailing content by default, or just the type of content specified by type.

**Raises**: Http404 if no message is found.

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

**Returns**: An array of groups.

**Raises**: Http404 if no mailing is found.

## GET /#account_id/mailings/#mailing_id/searches

> GET /100/mailings/200/searches

```json
[]
```

Get all searches associated with a sent mailing.

**Returns**: An array of searches.

**Raises**: Http404 if no mailing is found.

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

**Returns**: True if the mailing is successfully archived.

## DELETE /#account_id/mailings/cancel/#mailing_id

> DELETE /100/mailings/cancel/201

```json
true
```

Cancels a mailing that has a current status of pending or paused. All other statuses will result in a 404.

**Returns**: True if mailing marked as cancelled.

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

**Parameters**:

`recipient_emails` (array) – An array of email addresses to which to forward the specified message.

`note` (string) – A note to include in the forward. This note will be HTML encoded and is limited to 500 characters.

**Returns**: A reference to the new mailing.

**Raises**: Http404 if no message is found.

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

**Parameters**: 

`sender` (string) – The message sender. If this is not supplied, the sender of the original mailing will be used.

`heads_up_emails` (array) – A list of email addresses that heads up notification emails will be sent to.

`recipient_emails` (array) – An array of email addresses to which the new mailing should be sent.

`recipient_groups` (array) – An array of member groups to which the new mailing should be sent.

`recipient_searches` (array) – A list of searches that this mailing should be sent to.

**Returns**: A reference to the new mailing.

**Raises**: Http404 if no mailing is found.

## GET /#account_id/mailings/#mailing_id/headsup

> GET /100/mailings/200/headsup

```json
[]
```

Get heads up email address(es) related to a mailing.

**Returns**: An array of heads up email addresses.

**Raises**: Http404 if no mailing is found.

## POST /#account_id/mailings/validate

> POST /100/mailings/validate

```json
{
  "subject": "Another Fine Test"
}

true
```

Validate that a mailing has valid personalization-tag syntax. Checks tag syntax in three params:

**Parameters**: 

`html_body` (string) – The html contents of the mailing

`plaintext` (string) – The plaintext contents of the mailing. Unlike in create_mailing, this param is not required.

`subject` (string) – The subject of the mailing

**Returns**: true

**Raises**: Http400 if any tags are invalid. The response body will have information about the invalid tags.

## POST /#account_id/mailings/#mailing_id/winner/#winner_id

> POST /100/mailings/202/winner/203

```json
{}

true
```

Declare the winner of a split test manually. In the event that the test duration has not elapsed, the current stats for each test will be frozen and the content defined in the user declared winner will sent to the remaining members for the mailing. Please note, any messages that are pending for each of the test variations will receive the content assigned to them when the test was initially constructed.

**Returns**: True on success.

**Raises**: Http403 if the winner cannot be manually declared.

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

**Parameters**: 

`deleted` (boolean) – Accepts True or 1. Optional flag to include deleted members.

**Returns**: A list of members in the given account.

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

**Parameters**: 

`deleted` (boolean) – Accepts True or 1. Optional flag to include deleted members.

**Returns**: A single member if one exists.

**Raises**: Http404 if no member is found.

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

**Parameters**: 

`deleted` (boolean) – Accepts True or 1. Optional flag to include deleted members.

**Returns**: A single member if one exists.

**Raises**: Http404 if no member is found.

## GET /#account_id/members/#member_id/optout

> GET /100/members/201/optout

```json
[]
```

If a member has been opted out, returns the details of their optout, specifically date and `mailing_id`.

**Returns**: Member opt out date and mailing if member is opted out.

**Raises**: Http404 if no member is found.

## PUT /#account_id/members/email/optout/:email

> PUT /100/members/email/optout/tony@myemma.com

```json
{}

true
```

Update a member’s status to optout keyed on email address instead of an ID.

**Returns**: True if member status change was successful or member was already opted out.
**Raises**: Http404 if no member is found.

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

**Parameters**: 

`members` (array) – An array of members to update

A member is a dictionary of member emails and field values to import. The only required field is "email". All other fields are treated as the name of a member field.

**Parameters**: 

`source_filename` (string) – An arbitrary string to associate with this import.

This should generally be set to the filename that the user uploaded.

**Parameters**:

`add_only` (boolean) – Optional. Only add new members, ignore existing members.

`group_ids` (array of integers) – Optional. Add imported members to this list of groups.

**Returns**: An import id

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

**Parameters**: 

`email` (string) – Email address of member to add or update

`fields` (dictionary) – Names and values of user-defined fields to update

`group_ids` (array of integers) – Optional. Add imported members to this list of groups.

`field_triggers` (boolean) – Optional. Fires related field change autoresponders when set to true.

**Returns**: The `member_id` of the new or updated member, whether the member was added or an existing member was updated, and the status of the member. The status will be reported as ‘a’ (active), ‘e’ (error), or ‘o’ (optout).

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

**Parameters**: 

`email` (string) – Email address of the member to sign-up.

`group_ids` (Array of strings (group_ids)) – An array of group ids to associate sign-up with.

`fields` (dictionary) – Optional. Names and values of user-defined fields to update.

`signup_form_id` (integer) – Optional. Indicate that this member used a particular signup form. This is important if you have custom mailings for a particular signup form and so that signup-based triggers will be fired.

`opt_in_subject` (string) – Optional. Override the confirmation message subject with your own copy.

`opt_in_message` (string) – Optional. Override the confirmation message body with your own copy. Must include the following tags: [rsvp_name], [rsvp_email], [opt_in_url], [opt_out_url].

`field_triggers` (boolean) – Optional. Fires related field change autoresponders when set to true.

**Return**: The `member_id` of the member, and their status. The status will be reported as ‘a’ (active), ‘e’ (error), or ‘o’ (optout).

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

**Parameters**: 

`member_ids` (array) – An array of member ids to delete.

**Returns**: True if all members are successfully deleted, otherwise False.


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

**Parameters**: 

`member_ids` (array) – The array of member ids to change.

`status_to` (string) – The new status for the given members. Accepts one of ‘a’ (active), ‘e’ (error), ‘o’ (optout).

**Returns**: True if the members are successfully updated, otherwise False.

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

**Parameters**: 

`email` (string) – A new email address for the member.

`status_to` (string) – A new status for the member. Accepts one of ‘a’ (active), ‘e’ (error), ‘o’ (opt-out).

`fields` (array) – An array of fields with associated values for this member.

`field_triggers` (boolean) – Optional. Fires related field change autoresponders when set to true.

**Returns**: True if the member was updated successfully

**Raises**: Http404 if no member is found.

## DELETE /#account_id/members/#member_id

> DELETE /100/members/202

```json
true
```

Delete the specified member.

The member, along with any associated response and history information, will be completely removed from the database.

**Returns**: True if the member is deleted.

**Raises**: Http404 if no member is found.