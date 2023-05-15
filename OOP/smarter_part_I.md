can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?


(soft guitar music)
0:04
- All right
0:05
- So let's start with this slash dog's route
0:07
As it get requests.
0:09
The end goal is for us to be able to call a method,
0:11
that will still be a static method.
0:13
At least this one will.
0:14
Dog, If that's the name of our model class,
0:17
Oh, auto-correct,
0:18
you're killing me.
0:19
There we go,
0:20
dog with a capital D dot,
0:22
get all or find all or something.
0:24
But this method that's not defined yet,
0:27
should return an array of dogs.
0:30
So an actual dog object,
0:32
which it won't look like this.
0:34
This is sort of the Python syntax,
0:35
but a dog that has an ID and an age and a name,
0:39
and maybe some methods like speak, or, you know,
0:43
if this was a user log out or something actually useful,
0:46
not speak.
0:48
But anyway,
0:49
it's a blueprint that we can follow
0:50
for other more useful models.
0:53
But dogs are useful, right?
0:55
So what we're going to do is start by defining
0:57
that Basic Dog Blueprint.
0:59
just a dog that has an ID, a name and an age,
1:03
just like we did way back.
1:04
When we started Object Oriented Java script,
1:06
we didn't have any real world applications at that point,
1:10
we just defined a constructor.
1:11
We put, you know,
1:12
this dot age equals age,
1:14
this dot id equals ID.
1:15
So let's do that to start.
1:18
I'm going to make a new model,
1:19
dog dot J S
1:21
in my model's directory.
1:23
And I'll define a class dog.
1:25
We'll have a constructor,
1:28
and this constructor will be past ID, name, and age.
1:32
And then we'll set this dot id to ID,
1:35
this dot name equals name.
1:38
Now we're not going to be calling this constructor ourselves
1:40
very often.
1:41
Probably not at all,
1:43
but we still need this here.
1:45
We're going to then define a method,
1:46
like get all and get all will end up...
1:50
That's not an arrow function.
1:51
That's just supposed to be an arrow.
1:53
It will end up getting some data,
1:55
from Postgres like we've already seen.
1:58
And then we'll loop over that data
2:00
and instantiate a new dog,
2:02
like new dog with that data we get back from sequel.
2:07
So we need this in place.
2:10
And then we'll do a module that exports equals dog.
2:15
And that is our basic class.
2:18
No Sequel, nothing involved yet.
2:21
Now to create our method that we're going to use dog dot,
2:26
get all or find all,
2:27
whatever will do for cats
2:29
Get all, let's be consistent.
2:31
So dog get all,
2:33
I'd like to be able to call that in here.
2:35
So I'm going to need to import or require dog,
2:39
require and from here, I'm in the routes directory.
2:43
So dot dot slash models slash dog,
2:46
need to back out one and then go into models,
2:49
dog dot JS.
2:51
I'm not going to need DB anymore in here,
2:54
because I'm not going to be making the queries here anymore.
2:57
I do need DB over here though.
2:59
And I'm probably going to want express error.
3:02
So let's just copy that.
3:04
Although these paths are incorrect, aren't they?
3:07
Actually no, they are correct.
3:10
Nevermind. Ignore that.
3:12
So let's implement our, get all.
3:15
I'd like to be able to call dog dot, get all.
3:17
And then that should return to me an array of dog objects.
3:20
So over here in my model class,
3:22
we'll define our static async method.
3:25
Get all it's still static.
3:27
It doesn't make sense to have an instance method called,
3:29
"get all". I mean, you could make it work,
3:32
but you would first have to instantiate a dog.
3:34
We don't need that.
3:35
We'll just call dog dot get all.
3:37
And that should return instances of dog.
3:40
So in here, we'll do DB,
3:42
which we've already required dot query dot,
3:45
not dot
3:47
and then we'll pass in a string
3:49
and we'll select all dogs
3:51
and we'll be specific. We want ID name and age.
3:54
Those are the only three columns from dogs.
3:59
And we'll await that,
4:02
save that to a variable constant.
4:05
Let's just go with results, results.
4:08
And that will give us a results object,
4:10
which has rows in it.
4:13
So we have results dot rows.
4:15
And what I want to do is for each row,
4:17
I want to map that in to a new dog object.
4:21
And then I want to return an array of those dog objects.
4:25
So if we just do a console dot log results to start,
4:30
maybe results,
4:31
just results that should work.
4:32
We're not actually returning anything here.
4:35
Why don't we start by just return results dot rows.
4:40
And in dogs JS,
4:43
we'll just return Rez dot JSON of dogs.
4:46
Let's see if this works now.
4:48
All right, so we are getting that data back.
4:50
And if we look in our console, as we've seen,
4:53
we have a raws property.
4:55
It's an array.
4:56
These are not dog instances.
4:58
So we want to map over this array.
5:00
This is one way of doing it map through this array
5:03
and for each object here,
5:05
turn it into an actual dog or make a new dog.
5:08
So in our model right here,
5:12
we'll make a new variable can call this dogs equals results,
5:16
dot raws dot map,
5:18
and then we'll call each.
5:20
We could go with row or D,
5:23
I guess R it represents a single row.
5:26
We'll create a new dog
5:28
and we'll pass in our dot id row dot id,
5:32
our dot name our dot age.
5:35
matching this order here,
5:36
ID, name and age.
5:38
Okay.
5:40
And then,
5:40
and that is what we're going to return,
5:42
return dogs.
5:44
And we'll counsel dot log dogs there,
5:47
and result dot raws here.
5:50
So I'm going to send another request,
5:52
and scroll down in my console.
5:54
This is what we had before.
5:56
Now, we've created an array of dog objects.
6:00
So each dog has an ID, a name and an age.
6:03
And it doesn't really seem like much of an improvement,
6:06
aside from the fact that we have the class dog.
6:09
It's a little text that says,
6:11
"this thing is a dog,"
6:12
which is nice enough, I guess,
6:15
but now we have the option to add in different functionality
6:18
to dogs.
6:19
We could add an instance method like,
6:22
well, how about bark, Speak,
6:27
kind of stupid, but we'll do that.
6:29
And console dot log Woof.
6:33
Or how about this dot dog,
6:35
this dot name says woof?
6:37
So this,
6:39
dot name
6:41
says,
6:42
woof.
6:44
Now we have a method.
6:45
And remember,
6:46
this would typically be something actually useful,
6:49
like logging out a user.
6:50
User dot log out,
6:51
Timmy dot log out,
6:52
current user dot log out.
6:54
But those are all instance methods on some particular
6:57
instance of a model, user model in that case.
7:01
speak is just a very simple one.
7:03
So now I could call, you know, speak on each dog over here.
7:08
So we're getting all of our dogs back, right?
7:09
We've returned dogs.
7:11
I'll get rid of my console dot logs
7:13
and make my query get all dogs.
7:15
Turn that into an array of dog instances.
7:17
I'm calling new dog, so this constructor runs.
7:21
Then we have a dog instance, a dog object for each row.
7:25
We returned that array of dogs.
7:27
And then here,
7:28
we're awaiting that array of dogs
7:31
and notice that I'm just rez dot JSONing,
7:34
dogs, the entire array.
7:36
What do we get back?
7:38
We get back the same JSON.
7:40
So in express,
7:41
it's smart enough to take an object like our dog class,
7:46
a dog instance rather, and turn it into JSON.
7:49
So it just gives us the different properties,
7:51
ID, name, and age.
7:53
If I were to add something onto each dog,
7:56
like this dot species
8:00
equals dog like that,
8:04
now it automatically includes that,
8:06
as part of the JSON response.
8:09
So it's taking all the properties,
8:11
all the fields on a given instance of dog
8:14
and turning it into JSON.
8:15
We don't have to serialize it ourself.
8:17
It's ignoring methods like speak, which is really nice.
8:21
So we don't have to really do anything fancy at all.
8:23
Just rest out JSON, that array of dog objects.
8:27
But now if I wanted to, I could call speak on each dog,
8:30
which I probably don't want to,
8:32
but let's do dogs dot for each,
8:35
for each dog, we'll call dog dot speak.
8:39
And now if I send that same request,
8:41
look in my console.
8:43
Whisky says Woof,
8:44
waffles says woof.
8:46
So that's our first little example,
8:48
of adding some functionality to each particular dog.
8:51
fortunately in a bit.
8:52
And we'll see something more useful,
8:54
especially around deleting a dog and updating a dog.
8:57
Those will be Instance methods,
8:59
but for now we are getting all dogs and then returning,
9:03
get rid of that.
9:04
An array of dog Instances.
9:07
Now let's add our other get route,
9:09
to get a particular dog by ID.
9:13
So the route should be a get request is slash colon ID,
9:16
dogs slash ID.
9:18
And we'll call a method that doesn't exist.
9:22
Dog dot get by ID or find by ID,
9:24
which should be consistent with cat, get by ID.
9:28
Okay.
9:30
So get by ID and then we need to pass in this ID.
9:33
Again, this method doesn't exist.
9:35
request that params dot ID.
9:37
So now let's implement that,
9:40
get rid of that.
9:41
And we'll just respond with the dog itself,
9:44
assuming that this works
9:45
and gives us the dog that we found
9:47
and in our dog dot JS file,
9:49
this will be another static method.
9:52
It's not something we call on a particular dog.
9:54
We're trying to find a dog.
9:56
So static async get by ID.
9:59
It will accept an ID as its single parameter there.
10:02
And then we'll make our query
10:04
const results equals await,
10:07
DB dot query.
10:09
And I'll probably do this one on multiple lines.
10:12
So select,
10:13
ID, name come at age from dogs,
10:17
and then where ID equals dollar sign one.
10:22
And then we'll pass in after that string,
10:27
ID.
10:28
This query should get one dog based off of that ID.
10:32
And then we'll just return a new dog,
10:36
created based off of that result.
10:39
So what will be easiest is to isolate that row.
10:42
Let's just go with constant D equals,
10:44
results dot rows of zero.
10:48
Now I have this D variable,
10:50
I can pass into new dog.
10:51
The dog constructor will be called,
10:53
D dot name and D dot age.
10:57
So I'm making a new dog and then I'm returning that.
11:00
And at the moment we have no error handling,
11:03
but let's just try requesting a dog that we know exists,
11:06
like idea of one or two.
11:09
Not Found,
11:11
dog slash one as a get request.
11:13
I think I forgot to save this file. Didn't I?
11:15
Yeah, I did, try again.
11:18
There we go.
11:19
We're getting idea of one,
11:20
name: whiskey,
11:21
age: six
11:22
dog slash two
11:24
ID of two name is waffles age is three.
11:26
Now we don't have our standard error handling,
11:29
which we definitely should add in,
11:31
in our dog here.
11:33
There's definitely an option
11:34
that we might not find any dogs, right?
11:38
We might not find a dog with that ID.
11:40
So since we're already looking at results dot rows of zero,
11:43
probably just do, if not D,
11:47
we'll throw a new express error,
11:50
which I can't remember if I imported, did I?
11:52
Yes.
11:54
Dog not found,
11:57
four Oh four.
11:59
And up here in my route.
12:01
I need to make sure I have try and catch.
12:05
So wrap that and try,
12:07
catch any error return next of that error.
12:12
Okay. So now if I try and look for dog slash five,
12:16
dog not found four Oh four.
12:18
And there's not really much that could go wrong here,
12:21
but we still should wrap this in a try-catch
12:24
because there definitely are things that could go wrong.
12:27
Just not really based off of the data
12:29
that a user is providing.
12:31
This is always the same.
12:32
It's just slash we're getting all Dogs,
12:34
return next error.
12:39
All right, so at this point,
12:41
we haven't really noticed much of an improvement
12:43
from this strategy or this new approach.
12:46
Yes we do have a dog object that we're getting back
12:49
or multiple dog objects here,
12:50
a single one here based off of a Sequel query
12:54
that is sent off.
12:55
But those dog objects are just turned into JSON
12:58
and we don't really do anything with them.
13:00
We could, we could add methods as we saw with speak.
13:04
So technically that is an improvement.
13:06
It's just not very useful here,
13:08
but this pattern is going to pay off,
13:10
in the next video or two,
13:11
once we talk about things like updating a dog
13:13
or deleting a dog.
13:15
