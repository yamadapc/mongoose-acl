mongoose-parent-acl [![Build Status](https://travis-ci.org/yamadapc/mongoose-parent-acl.png?branch=master)](https://travis-ci.org/yamadapc/mongoose-parent-acl)
===

Forked from https://github.com/scttnlsn/mongoose-acl

Usage
---

```javascript
var mongoose = require('mongoose');
var acl = require('mongoose-parent-acl');

var WidgetSchema = new mongoose.Schema({ … });
WidgetSchema.plugin(acl.object);

var UserSchema = new mongoose.Schema({ … });
UserSchema.plugin(acl.subject);
```
    

Methods
---
The plugin adds accessor methods to the object for getting and setting
permissions of a particular key:

```javascript
var widget = new Widget({ … });

widget.setAccess('foo', ['a', 'b']);
widget.getAccess('foo'); // => ['a', 'b']
```

Or getting all keys with given permissions:

```javascript
widget.keysWithAccess(['a']); // => ['foo']
```
    
There are also convenience methods added to the subject for getting and setting
the permissions for a given object:

```javascript
var user = …;

user.setAccess(widget, ['read', 'write', 'delete']);
user.getAccess(widget); // => ['read', 'write', 'delete']
```
    
We can query for all objects to which a particular subject has access:

```javascript
Widget.withAccess(user, ['read']).exec(function(err, widgets) {
    ...
});
```
    
Options
---

### Object

We can specify the path in which the ACL will be stored (by default it will be
available at `_acl`):

```javascript
WidgetSchema.plugin(acl.object, {
    path: '_acl'
});
```
    
### Subject

Each subject is referred to in an ACL by a unique key (by default it is of the
form `subject:<subject _id>`).  This can be customized by specifying a `key`
option:

```javascript
UserSchema.plugin(acl.subject, {
    key: function() {
        return 'user:' + this._id;
    }
});
```
    
We can also specify additional ACL keys to which a subject has access.  For
example, suppose a user optionally belongs to a number of roles:

```javascript
UserSchema.plugin(acl.subject, {
    additionalKeys: function() {
        return this.additional.map(function(key) {
            return 'additional:' + key;
        });
    }
});
```
    
There is one special key referred to as the public key.  If set, the associated
permissions will apply to all subjects:

```javascript
UserSchema.plugin(acl.subject, {
    public: '*'
});
```

Install
---

    npm install mongoose-parent-acl
    
Tests
---

    npm test

## Donations
Would you like to buy me a beer? Send bitcoin to 3JjxJydvoJjTrhLL86LGMc8cNB16pTAF3y
