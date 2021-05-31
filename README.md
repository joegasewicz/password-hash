# Password Mixin
Mixin thats add some useful functions to ORM objects

# Install
```bash
pip install python-mixin
```

## Setup
first create your objects (of ORM model) and add a `__hash_secret__` meta field.
Assign your application's secret value to `__hash_secret__`.

```python

from password_mixin import PasswordMixin
from sqlalchemy import Model # or Django , Flask-Sqlalchemy... etc.

class UserModel(PasswordMixon, OrmModel):
    
    password = Column(String()) # you must have a `password`.
    
    
    # Now create a meta field to define the secret used to create the salt, for example:
    __hash_secret__ = "your-app-secret"
        
```

## Usage
The password is saved as the following: `"<salt>:<hash>"`

### Password Hashing
```python
from password_mixin import PasswordAttributeError
try:
    user = UserModel()
    user.password = "wizard123"
    user.hash_password() # password is now 31616263313233:7ac5cf88e8c9d262b49af168d9c30e47f2945cc9c207f20af0a39f09aa04595e
    # Now you can save your user to your db etc.
except PasswordAttributeError:
    # handle no password attribute
    

```

### Validating Passwords
```python
from password_mixin import PasswordMatchError

try:
    user.check_password("wizard111")
except PasswordMatchError:
     # handle passwords don't match
```