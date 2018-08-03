# Device Registry Service

## Usage

All responses will have the form

```json
{
  "data":"Mixed type holding the content",
  "message":"Description of what happened"
}
```

Subsequent response definitions will only detail the expected value of the `data field`

### List all users

**Definition**

`GET /users`

**Response**

- `200 OK` on sucess
```json
[
  {
    "id":"identifier",
    "name":"Name of the person",
    "lastname":"Last Name",
    "gender":"Male, Female"    
  },
  {
    "id":"identifier",
    "name":"Name of the person",
    "lastname":"Last Name",
    "gender":"Male, Female"    
  }
]  
``` 
 
### Registering a new user

**Definition**

`POST /users'

**Arguments**

- `"identifier":string` a unique indentifier for this user 
- `"name":string` a name
- `"lastname":string` a last name
- `"gender":string` sex of the person

If a user already exists with  the given identifier, the existing user will be overwritten.

**Response**

- `201 Created` on success

```json
 {
    "id":"identifier",
    "name":"Name of the person",
    "lastname":"Last Name",
    "gender":"Male Female" 
 }
```
