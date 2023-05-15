can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?

0:00
(upbeat music)
0:04
- [Instructor] Alright, so next up
0:05
we'll see a different approach
0:06
for constructing a model class.
0:08
This will be much closer to what we've already seen
0:11
with actual ORMs like SQLAlchemy.
0:14
So rather than just defining a static
0:16
or defining a class with a bunch of static methods
0:19
like we did for cat, our cat model is really just
0:22
a bunch of methods grouped together.
0:24
They happen to be in a cat class.
0:26
They could just be in some standard,
0:29
plain or JavaScript object.
0:31
They could be separate functions or methods
0:33
in something else, but it makes sense to group them in cat.
0:37
It's just easier and when we run our code,
0:39
it's clear that we're getting all cats
0:41
or we're getting one cat by ID.
0:44
But with this smarter object oriented model
0:46
we are actually going to be instantiating
0:49
new instances of our class.
0:51
We still will have static methods,
0:53
but this will be more traditional.
0:55
More traditional in the sense of an actual class
0:58
where we create new instances.
1:00
It acts as a blueprint
1:01
and we have different properties
1:03
and different instance methods
1:05
as well as some static methods mixed in,
1:07
but also it will be more like a traditional ORM.
1:10
So here's a little flashback to flask SQLAlchemy.
1:14
If we had a user model
1:16
and we ran user.query.filter by
1:19
or user.query.filter, or order by,
1:24
notice and hopefully you remember this
1:26
what we get back is a list
1:28
where each object it's not just some hash
1:32
or it's not a string.
1:34
It's an actual user object.
1:36
It's an instance of the user class.
1:39
So we get back an object that has
1:42
I don't know an email, a password an ID
1:45
and we can access those as properties.
1:47
Right now we don't have that,
1:49
with cat, we are responding with data
1:52
but our cat class methods like create
1:57
is just responding with a plain old JavaScript object
2:00
that has an age and a name and an ID.
2:03
And that works, but what if we wanted to be able
2:05
to call certain methods on a cat?
2:08
Like if I wanted to call,
2:10
speak or meow on a particular cat.
2:13
Or if cats had a relationship with toys,
2:18
there is an association between the two,
2:20
I want it to be able to get one cat and then call cat
2:25
about prissy.add new toy, or get toys.
2:32
Those are methods I would call an a particular
2:34
instance of a cat,
2:36
but we don't have that ability right now.
2:38
We don't have that method
2:39
but we'd also don't have anything close to that
2:42
because we are not instantiating instances of cat.
2:45
So what we're going to do now is create a model class.
2:49
We're gonna work with dogs instead.
2:52
The SQL file that I gave you also creates a dog's table
2:55
and it inserts like two or three dogs.
2:58
So we're gonna write some logic for dog's
3:00
where we will actually get back an object,
3:03
a dog object when we make our queries.
3:06
When we call dog dot find all
3:09
or get all or get by ID
3:11
or create, we'll get back a dog like D
3:15
if we just name it D and I'll be able to get d.id.
3:19
D.age and I could add methods like, d.speak,
3:23
d.fetch and you could extrapolate this
3:26
to something more realistic instead of a dog,
3:28
we could have a user that we get back like Timmy,
3:32
and we could call Timmy.reset password.
3:36
That could be a method, an instance method on our model.
3:40
We could have Timmy.change profile picture.
3:45
So let's see how we're gonna go about this.
3:48
So overall, this approach is more traditional,
3:51
as we've already discussed, we will instantiate our model.
3:54
It won't just consist of a group,
3:56
a collection of static methods.
3:58
We'll actually have a constructor,
4:00
we'll create new dog instances
4:03
and we're doing dogs of dog instance
4:05
but whatever your model is,
4:07
if you follow this approach you'll be instantiating
4:09
new instances of that model class.
4:11
And this class, each instance will hold specific data.
4:15
That is particular to one dog.
4:18
We will have static methods
4:19
so we can still do things like get all dogs
4:22
or find one dog that doesn't really make sense
4:25
to be a instance method on a particular dog.
4:29
Things like updating one dog or deleting a dog makes sense.
4:33
Those need to be instance methods
4:35
on the particular dog object,
4:37
but get all dogs, get one dog
4:41
you don't really call those on a dog.
4:43
Otherwise, I mean you already have the dog,
4:45
why are you trying to find a particular dog using that dog?
4:48
Anyway, you'll see how it all works.
4:50
Well, have regular methods, static methods,
4:53
and it's like a mini ORM.
4:55
So in the next video,
4:55
we'll start actually defining our model
4:58
but I'll just give you an overview
4:59
of what we have right now.
5:01
So at the moment I have a dogs.JS route file
5:05
and it has a single route.
5:06
There's no model involved.
5:08
This is our old way of doing it.
5:10
We're just awaiting DB.query, select star from dogs.
5:14
So it's a get request to slash in my app JS
5:18
I'm requiring the dog routes./routes/dogs this file here,
5:22
and then I'm telling my app to use dog routes
5:25
prefixed by slash dogs.
5:28
So this is actually slash dogs as a get request.
5:32
We get all dogs in the database
5:34
and we send them back as Jason.
5:36
We don't have try and catch cause I'm being lazy
5:38
but we're gonna update this anyway.
5:40
And if I send a request, get request to slash dogs
5:43
I currently have two dogs in there.
5:45
Okay, so that's our starting point.
5:48
In the next video we'll define our dog model
5:51
and write our first couple of methods.