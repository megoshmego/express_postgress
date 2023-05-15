can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?

Transcript


0:04
- Next up, let's implement a route to create a new dog.
0:08
So that's going to be a router.post.
0:11
And it should just be slash
0:13
for following the same restful convention.
0:15
So a post request not get.
0:17
And in here, we're going to call some method on dog,
0:21
probably something like create.
0:24
And dog.create should insert a new row into the dog's table.
0:28
And assuming that it worked out,
0:30
and it should also return to us a new dog instance,
0:33
just like find did, right?
0:35
Get by ID, or get all.
0:37
Both of those gave us dog objects back
0:40
not just plain old JavaScript objects,
0:41
but actual dog instances.
0:44
So dog.create doesn't exist.
0:46
But if it did, we would want to pass in a name and an age,
0:51
which should be stored in request.body.
0:54
So we could destructure that.
0:57
Name and age equals request.body.
1:01
And then pass that in.
1:03
Name comma, age.
1:04
And then we can respond with that dog
1:07
that hopefully we get back.
1:09
But we need to implement dog.create.
1:11
So let's do that now.
1:13
So another static method, this will be our last one.
1:16
And then deleting
1:18
and updating will both be instance methods.
1:20
Static async, create,
1:23
which should accept a name and an age.
1:26
And then in here,
1:27
we're going to make our query.
1:29
So const results equals await db.query.
1:34
And then our query will be insert into dogs.
1:40
And then we'll specify name first.
1:43
And then age,
1:45
values will be dollar sign one,
1:49
dollar sign one,
1:51
dollar sign two.
1:53
And we can also tell it to return some stuff.
1:56
So returning ID, name, and age.
1:59
Technically, we don't need it to return name and age,
2:01
we already have them.
2:02
But I'll talk about that in just a bit.
2:05
And then after that,
2:06
and then dollar sign one should be name,
2:08
dollar sign two should be age.
2:11
Okay, all right.
2:12
So that should insert a new dog.
2:15
And then we'll take results.rows of zero.
2:20
And we can just destructure ID, name, and age from that.
2:24
So const,
2:26
ID,
2:26
name and age.
2:27
And we can use those new variables to create a new dog.
2:31
So return new dog
2:34
ID,
2:36
name, and age.
2:37
It's the same order we defined in our constructor, great.
2:41
So you definitely could add in additional error checking.
2:43
Throw new express error.
2:45
You know, missing data or name required,
2:47
or whatever the different errors would be.
2:49
But we'll just keep it simple for now.
2:51
Just keep that in mind
2:52
if you're actually writing a more complex app
2:53
that you want to have useful feedback on.
2:56
You want to make sure you're providing that feedback
2:59
and throwing those errors appropriately.
3:01
AlL right, so I mentioned that we already have name and age.
3:04
Do we actually need our query
3:06
to return all of ID, name, and age
3:08
if we already have name and age?
3:10
The answer is kind of no.
3:12
We could just ask for the ID back
3:14
and get that from results.rows of zero
3:17
and then use the existing name and age.
3:19
The reason we may want to do it this way,
3:21
returning ID, name, and age is that there's
3:24
the potential for certain hooks or intermediate operations
3:29
when we insert name and age or any field,
3:32
they may not actually insert exactly
3:35
the way that we've written.
3:36
For example, if we have a limit on the number of characters,
3:39
for a name or whatever, they could be shortened.
3:42
Name could come back as a shortened name
3:45
and not the actual name that a user passed in
3:47
or that a user sent in the request.
3:49
So we may want to return all three.
3:51
The only problem we're going to run into now is
3:53
that pretty sure
3:56
name and age have already been defined.
3:58
They're already identifiers that are being used.
4:01
Let's just see if that's true.
4:03
Let's send a post request to slash dogs
4:07
at our data.
4:08
So name will be Bobo.
4:11
Age will be three.
4:14
Let's send it.
4:16
Couldn't connect to server.
4:18
Yeah, we're getting an error here, right?
4:20
This is a error in my application.
4:22
Where's the message here?
4:23
Identifier name has already been declared.
4:26
So we could modify that by just new name,
4:31
new age,
4:33
and then pass that in here.
4:35
New name
4:37
and new age.
4:41
And that way, I can still destructure without having
4:43
to change name and age.
4:45
It's easiest.
4:46
Okay, let's try that again.
4:48
Servers going.
4:49
Send that post request.
4:51
There we go.
4:52
ID of three.
4:52
Name is Bobo.
4:53
Age is three.
4:54
Let's look at all dogs.
4:57
Here we are,
4:58
our three dogs,
4:59
Whiskey, Woofles, and Bobo.
5:01
So we're now creating or inserting a new dog row
5:04
and then returning from this create method,
5:07
an actual dog instance.
5:09
Next up, let's talk about deleting a dog.
5:13
So to delete a dog,
5:14
what I want to be able to do is find a dog first,
5:18
like we have right here get by ID.
5:21
And then I want to be able to call,
5:24
let's paste that down here,
5:26
dog.remove, or delete or something, erase.
5:29
Kill off, which is very sad, you should not kill dogs.
5:33
But we could name our method that.
5:34
I can't stop you.
5:36
And I want that instance method to actually remove that dog.
5:40
So that's easy enough to implement.
5:42
What I'm gonna do is copy this route,
5:45
paste it down here,
5:47
make it a delete request.
5:48
We need that ID.
5:49
Which dog are we trying to delete.
5:51
Then we will select a dog.
5:53
We're not selecting it, but we're finding.
5:55
Get by ID.
5:56
That returns a dog object.
5:59
Then we should be able to call dog.remove.
6:03
Now dog.remove is something we'll probably need to await.
6:06
Because removing takes time.
6:08
And in that method,
6:09
we'll want to make sure we actually delete the dog.
6:12
So let's go into our model
6:14
and implement that method as a instance method.
6:18
So async, still should be asynchronous.
6:21
Async delete, or remove, I think is what we called it.
6:24
And we don't even need to pass in an ID to remove.
6:28
We're just calling it on dog.
6:30
A dog already has an ID.
6:32
Dog is an instance of this class.
6:35
This dot ID equals ID.
6:37
So my query will look something like this, const results.
6:40
We don't even need the results really do we?
6:44
Maybe we want to add that in
6:47
just in case.
6:49
We could add some error checking
6:52
if the dog was not actually deleted.
6:54
But anyway, const results equals await db.query
6:59
and then delete
7:02
from dogs
7:04
where
7:05
ID equals dollar sign one.
7:09
And then that will be this dot id.
7:13
It's an instance method.
7:15
So we find the dog using the static class method,
7:18
dog.getbyID, we pass in that ID.
7:21
That creates us a new dog instance.
7:24
Right here, we return that new dog.
7:27
It already has an ID.
7:28
So when we call remove,
7:30
we can look into the particular instance this.id.
7:33
This is an instance method.
7:35
So it has access to this.id, this.name, this.age.
7:38
And I don't know what I was thinking,
7:40
we don't even need to save this to a variable.
7:43
I was thinking we do some error checking
7:45
to see if you know we could return ID
7:48
and check if ID is missing.
7:50
If there was nothing deleted
7:51
that means we didn't find the dog.
7:53
However, forgot that we're calling this first, get by ID.
7:57
And if that dog's not found
7:59
then we're not even going to call remove.
8:02
This line runs first.
8:04
So all we need in here is await DB.query.
8:07
And then I don't think
8:09
there's really anything we want to return.
8:11
Just leave it at that.
8:12
So that's remove.
8:14
Over here, we can await dog.remove
8:17
and then respond with something like deleted.
8:24
So now we have this nice little instance method.
8:27
Call it on one individual dog that my mini ORM is returning.
8:31
Dog.getbyID,
8:33
static method takes an ID,
8:35
finds the correct dog row,
8:37
returns that as an actual dog object,
8:39
which has a method remove on it.
8:40
Every dog object has removed.
8:43
I call remove.
8:44
I await that and then we return deleted.
8:46
All right, let's see if it works.
8:49
So look at all my dogs.
8:50
Let's delete Woofles.
8:51
That's an ID of two.
8:54
Delete.
8:56
Okay, it said deleted.
8:58
If I try get request,
9:00
dog not found.
9:01
If I try get requests for all dogs,
9:03
he definitely was deleted, sorry, waffles.
9:07
So that works, we now have remove, right?
9:10
It's pretty easy.
9:11
At this point, if we wanted to remove multiple dogs
9:14
just find them.
9:15
Or if we really wanted to, we could remove all of them
9:18
if we did dog.get all and then looped over those dogs
9:21
and called remove for each one.
9:23
Although you could also just write
9:24
a method called dog.deleteall
9:26
and do it in a single SQL query.
9:28
Here, this is an individual query
9:31
every time we remove a dog,
9:32
but that makes sense.
9:33
Usually we're just removing one thing at a time.
9:36
And then lastly, let's talk about updating a dog.
9:39
So this is my favorite part.
9:41
Well, my favorite part of these basic methods.
9:43
If we wanted to update a portion of the dog
9:46
we could have a route that I don't know
9:49
maybe slash ID slash age,
9:51
which would increment the age of a dog.
9:54
We could find the dog, like we have here.
9:58
And that gives us a dog object.
10:00
And I could change dog.age.
10:02
I could do plus equals one or something like that.
10:05
Then that's not actually saving anything.
10:08
That's on updating SQL.
10:09
But we could write a simple method called save.
10:12
And save will take whatever attributes,
10:14
whatever properties are currently on a dog instance,
10:18
and update them in the database.
10:21
So I could have another route called rename
10:23
where I could change dog's name to be something.
10:27
And then call dog.save.
10:29
And now I just have one save method.
10:32
I don't need a method called change name.
10:33
I don't need one called change age or rename, increase age.
10:37
I can do a single save method.
10:39
And I could have a really complex model
10:42
where we had 10 different columns.
10:44
And I could change them all individually
10:46
just like we did with SQL Alchemy and then called dog.save,
10:50
which is kind of like commit and SQL Alchemy.
10:53
It's really not quite the same, but the same idea.
10:56
So we change the actual object.
10:58
The instance, in JavaScript, it has nothing to do with SQL,
11:01
until we call dog.save.
11:03
So let's start by defining dog.save,
11:06
which will be a asynchronous instance method.
11:10
It doesn't accept any attributes.
11:12
All we want to do is take this.name and this.age.
11:16
We don't expect ID to change, although technically,
11:19
we're not preventing anyone from changing ID.
11:21
But we don't want them to change the ID.
11:23
So we'll just take this.name and this.age
11:26
and update them in the database.
11:29
Let's copy this query here.
11:31
And instead of insert, blah, blah, blah,
11:34
we're going to do const results equals await db.query.
11:38
And here, we'll do updates, dogs,
11:42
Set
11:44
name equals
11:45
dollar sign one.
11:48
And we'll probably do that on the same line,
11:49
age equals dollar sign two.
11:53
And then we want to say where
11:57
ID equals dollar sign three.
12:00
So we only want to update one dog.
12:02
And we need to make sure we're passing in the values.
12:04
So dollar sign one should be this.name,
12:08
dollar sign to this.age,
12:10
dollar sign three this.id.
12:14
Cool.
12:15
So this should work for us.
12:17
We definitely want to return something on an update now.
12:21
And what's extra cool is that if this works out
12:25
we don't really need to do anything.
12:26
We don't need to return anything from here.
12:28
Yeah, we could add some additional logic.
12:30
But those values are already updated on the dog instance.
12:35
If somebody is calling save,
12:36
presumably name has already been changed
12:38
or age has been changed.
12:40
Otherwise, we're just updating with
12:41
the exact same name and age, which doesn't matter.
12:44
The dog instance still has the correct name and age.
12:47
So I don't need to return anything.
12:49
I don't need to make a new dog.
12:50
We're calling this on an instance.
12:52
So let's check it out.
12:53
Let's write a,
12:54
what should we do here?
12:57
Let's do a patch route.
13:00
And we'll do a ID slash, I don't know age,
13:06
which will make older.
13:07
It will increment age by one year.
13:09
So we need to use that ID to find the dog,
13:12
dog.getbyID, same thing.
13:14
But now we'll change dog.age by one.
13:18
Dog.age plus equals one.
13:20
And then we'll call dog.save.
13:22
And we need to await that.
13:24
And then if that works,
13:26
res.JSON,
13:28
just the dog object.
13:29
So let's try it out,
13:30
send a get request to all dogs.
13:32
Let's age Bobo, who has an age of three, an ID of three.
13:37
So dog slash three slash age as a patch request.
13:42
And there we go ages for age is four,
13:44
age is five,
13:44
six, seven, eight.
13:47
Now we could also easily define another route,
13:50
also a patch request or put request,
13:53
it could even be a post request.
13:54
Let's go with slash rename.
13:57
And on this route, instead of just hard coding
13:59
the dog.age to go up by one,
14:02
we'll set dog dot name to be equal to request.body.name.
14:07
We'll just assume that a name has been passed in
14:09
as part of the request body.
14:11
And then we follow the same steps.
14:12
So we find the dog by that ID.
14:15
If we can't find it, our method get by ID throws an error,
14:18
at which point we're already on to the next thing.
14:20
If we do find it, we change the name locally on that object.
14:24
Then we call save.
14:25
And that updates the SQL table that actually updates the row
14:29
and then we respond with JSON.
14:32
So let's do slash three slash rename
14:36
as a patch request
14:37
and we need to pass in the new name,
14:39
which instead of Bobo will be Bilbo, sure.
14:44
Send.
14:46
And now name is Bilbo.
14:48
So at this point,
14:50
you can hopefully see the benefit of this second approach,
14:54
where we're returning an instance from our dog class.
14:58
These different static methods like get all,
15:00
get by ID, create, they return a dog instance.
15:03
And that dog instance has methods like remove and save.
15:07
We could also add fancier methods in.
15:09
We'll talk more in the next video when we wrap things up.
15:12
We'll compare the two approaches.
15:14
But at this point, that's pretty much it Full CRUD.
15:17
You could also just implement a standard update.
15:20
So just patch to slash ID
15:22
and update name and age at the same time.
15:25
But I wanted to show you
15:26
that you could selectively update one piece at a time.
15:29
And you don't need a separate method like we did for cat.
15:32
Right, for cats we had to make a make older method
15:36
and you had to pass in an ID.
15:37
But we'll compare and contrast in the next video.
15:40
(upbeat music)