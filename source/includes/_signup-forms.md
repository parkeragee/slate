# Signup Forms

With this endpoint, you can list all of your signup forms.

## List signup forms

> GET /:account_id/signup_forms

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