# Server API

**Page Content**

- [Common API](#common-api)
    1. [get\_doc](#frappeget_docdoctype-name)
    2. [get\_all](#frappeget_alldoctype-filters-fields)
    3. [get\_list](#frappeget_listdoctype-filters-fields)
- [Database API](#database-api)
    1. [set\_value](#set_value)
    2. [get\_value](#set_value)
    3. [get\_values](#set_value)
    4. [get\_values\_from\_single](#set_value)
    4. [get\_single\_value](#set_value)
    5. [exists](#exists)
    6. [count](#count)



## Common API


### frappe.get_doc(doctype, name)
Loads a document from the database with given doctype (table) and name.
It returns a document object which contains fields as properties and controller methods.

**Example**
```python
>>> doc = frappe.get_doc('Project', 'My Project')

>>> doc.name
My Project

>>> doc.doctype
Project

```

### frappe.get_all(doctype, filters, fields)
Returns a list of dict objects from the database


**Example**
```python
frappe.get_all('User',
    filters=[['email', '=', 'richard@piedpiper.com']]
    fields=['fullname', 'age'],
)

# Output
[{
    'fullname': 'Richard Hendricks',
    'age': 28
}]

```

### frappe.get_list(doctype, filters, fields)
Sames as frappe.get_all, but will only show records that are permitted for the session user


## Database API

### set_value
Sets value of the property

`frappe.db.set_value(doctype, docname, fieldname, value)`

**Example**
```python
frappe.db.set_value('User', 'test@gmail.com', 'age', '29')
```

### get_value
Returns a document property or list of properties.

`frappe.db.get_value(doctype, docname, fieldname)`

**Example**
```python
# return first customer starting with a
frappe.db.get_value("Customer", {"name": ("like a%")})

# return last login of User `test@example.com`
frappe.db.get_value("User", "test@example.com", "last_login")

last_login, last_ip = frappe.db.get_value("User", "test@example.com", ["last_login", "last_ip"])

# returns default date_format
frappe.db.get_value("System Settings", None, "date_format")
```


### get_values
Returns multiple document properties.

**Example**
```python
# return first customer starting with a
customers = frappe.db.get_values("Customer", {"name": ("like a%")})

# return last login of User test@example.com
user = frappe.db.get_values("User", "test@example.com", "*")[0]
```


### get\_values\_from\_single
Get values from `tabSingles` (Single DocTypes)

`frappe.db.get_values_from_single(fields, filters, doctype)`

**Example**
```python
user
```


### get\_single\_value
Get property of Single DocType.

`get_single_value(doctype, fieldname)`

**Example**
```python
# Get the default value of the company from the Global Defaults doctype.
company = frappe.db.get_single_value('Global Defaults', 'default_company')
```

### touch
Update the modified timestamp of the document.

`frappe.db.touch(doctype, docname)`


### exists
Returns true if document exists.

`frappe.db.exists(doctype, docname)`
**Example**
```python
# Check if document exists.
company = frappe.db.exists('Global Defaults', 'default_company')
```

### count
Returns `COUNT(*)` for given DocType and filters.

`frappe.db.count(doctype, filters)`
**Example**
```python
enabled_user_count = frappe.db.count('User', filters=[['enabled', '=', '1']])
```

```
frappe.db.sql()
frappe.db.sql_list()
frappe.db.get_singles_dict()
```


