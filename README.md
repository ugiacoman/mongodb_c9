# mongodb_c9
We'll be testing MongoDB on Cloud9 IDE infrastructure. Cloud9 allows us to avoid the OSX/Windows debate 
and provides an easy VM workstation for testing.

### Let's get started.

#### Create Cloud9 Account

1. Visit [Cloud9](https://c9.io/) and create an account.

2. Once you're logged in create a new workstation from your dashboard.

	![Dashboard](http://i.imgur.com/zJcw3rx.png)

3. Name your workstation and Choose the Python template. Then click `Create Workstation`

4. Let Cloud9 create your workstation.

#### Installing MongoDB

1. Now that we are inside of our workstation we can get started installing MongoDB.

	![Workstation](http://i.imgur.com/lqr0du4.png)

2. In the bash, we'll run `$ sudo pip install pymongo`. Let it install.

3. Now let's enter the python interpretor by running `$ python` and set a client by running the following 
three commands.

	```python 
	>>> from pymongo import MongoClient
	```
	```python
	>>> client = MongoClient()
	```
	```python
	>>> client = MongoClient("mongodb://mongodb0.example.net:27019")
	```

4. Now we have a test client that we can play with. Let's exit the interpretor by running `exit()`

#### Starting up MongoDB

1. Let's start up mongodb first by running the following command in the bash `$ sudo mongod`

2. Note that since we are in Cloud 9's VM we need to keep this terminal running, so let's open a new terminal tab inside the IDE. We'll let the first tab run mongodb and use the second tab for running scripts.

	![Startup](http://i.imgur.com/DQXhLdA.png)

#### Inserting Data

1. Let's create a python script by running the following command in the bash `$ touch insertino.py`.
After running you'll see if pop up in the file manager.

2. Let's enter the empty script and write the following:

	```python
	from pymongo import MongoClient
	from datetime import datetime

	client = MongoClient()
	db = client.test

	result = db.restaurants.insert_one(
	    {
	        "address": {
	            "street": "2 Avenue",
	            "zipcode": "10075",
	            "building": "1480",
	            "coord": [-73.9557413, 40.7720266]
	        },
	        "borough": "Manhattan",
	        "cuisine": "Italian",
	        "grades": [
	            {
	                "date": datetime.strptime("2014-10-01", "%Y-%m-%d"),
	                "grade": "A",
	                "score": 11
	            },
	            {
	                "date": datetime.strptime("2014-01-16", "%Y-%m-%d"),
	                "grade": "B",
	                "score": 17
	            }
	        ],
	        "name": "Vella",
	        "restaurant_id": "41704620"
	    }
	)
	```

3. This script is going to insert a restaurant object into our database.

	![Insertion](http://i.imgur.com/bvr474t.png)

4. Let's run the script by returning to the bash and running `$ python insertino.py`. On success, nothing
 will print out!

#### Reading Data

1. Let's check the entry out by creating a reading script called `readerino.py` and write the following 
inside:

	```python
	from pymongo import MongoClient

	client = MongoClient()
	db = client.test

	cursor = db.restaurants.find()

	for document in cursor:
	    print(document)
	```
2. Let's run this script by using the following command `$ python readerino.py`.

	![Reading](http://i.imgur.com/NOWndVE.png)

3. Since we have only added one entry, you'll only see one entry! Let's add another by running 
`$ python insertino.py` followed by `$ python readerino.py`. Now we have two entries.

	![Readerino](http://i.imgur.com/1WG3uIG.png)

#### Updating Data

1. Let's update Vella's restaurant. Using `restaurant_id`, we can call it and update a 
specific paramater. Let's create another script called `updaterino.py` and fill
it out with the following code. 

	```python
	from pymongo import MongoClient

	client = MongoClient()
	db = client.test

	result = db.restaurants.update_one(
	    {"restaurant_id": "41704620"},
	    {"$set": {"address.street": "1210 New Address"}}
	)	

	cursor = db.restaurants.find()

	for document in cursor:
	    print(document)
	```

2. This will find the first object with `restaurant_id = 41704620` and print out all the results
in the database. To update all objects use `update_many` instead of `update_one`.

#### Deleting Data

1. Let's delete all restaurant objects in Manhattan.

	```python
	from pymongo import MongoClient

	client = MongoClient()
	db = client.test

	result = db.restaurants.delete_many({"borough": "Manhattan"})

	cursor = db.restaurants.find()

	for document in cursor:
	    print(document)
	```

2. Similarly to `update_many`, `delete_many` will delete all objects matching it's criteria. 

#### Sharding and Replicas

1. One of the reasons to use MongoDB is because of how it holds data and the ease of sharding. 
Sharding is a type of database partitioning that separates large databases the into smaller, faster,
parts called data shards. It is difficult to simulate the benefit of sharding on a small test database,
but creating data shards in MongoDB is easy! For more information on how to shard visit 
[here](https://docs.mongodb.org/manual/tutorial/deploy-shard-cluster/).



#### Your Turn

1. Let's create a a restaurant object with the following attributes:

	```python
	"address": {
	    "street": "123 Richmond Rd",
	    "zipcode": "23185",
	    "building": "1",
	    "coord": [99.9999999, 99.9999999]
	},
	"borough": "Burg",
	"cuisine": "The Good Stuff",
	"grades": [
	    {
	        "date": datetime.strptime("2015-09-01", "%Y-%m-%d"),
	        "grade": "A",
	        "score": 11
	    },
	    {
	        "date": datetime.strptime("2013-01-16", "%Y-%m-%d"),
	        "grade": "B",
	        "score": 17
	    }
	],
	"name": "Burgos",
	"restaurant_id": "123456"
	```

2. Now let's find just that new restaurant object. Hint: you'll need to update the find() method in `readerino.py`. Find() takes in the following argument: `{"key":"value"}`.

3. Please submit a screenshot to ugiacoman@email.wm.edu of both the terminal output and the `readerino.py script.


