# Django Cornflakes
An implementation (not yet a package) to help creating shardable object IDs for django 
models, inspired by Instagram's sharding algorithm. 

#### How to integrate?
Currently since this project is not a package, you would have to place these files
somewhere in your django project, and reference from it.

Currently, I have only tested it with Postgres DB.

You need to set the `SHARD_EPOCH` time in your settings.py, like as follows -
```python
SHARD_EPOCH = 1314220021721
```

Here is an example of using ShardedIDField
```python
from yourprojectname.utils.cornflakes.decorators import model_config
from yourprojectname.utils.cornflakes.fields import ShardedIDField

@model_config()
class User(AbstractUser):
    id = ShardedIDField(primary_key=True, null=False)
```

#### Composition of an ID
Each of IDs consists of:
- 41 bits for time in milliseconds (gives us 41 years of IDs with a custom epoch)
- 13 bits that represent the logical shard ID
- 10 bits that represent an auto-incrementing sequence, modulus 1024. 

This means we can generate 1024 IDs, per shard, per millisecond

#### Learn More
Learn more about Instagram's sharding technique (and why it is required) -
https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c

