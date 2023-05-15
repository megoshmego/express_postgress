can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?


0:08
right here, reading cats with getAll and getById,
0:12
as well as deleting a cat.
0:14
In terms of crud, we're missing the you, updating a cat.
0:18
So to update cats,
0:20
there are many ways that we can update a cat, right?
0:22
We could define a method in here called I dunno,
0:25
static async update.
0:28
And in order to update a cat,
0:30
we need to know which cat we're updating.
0:32
So an id, we need to know a new name,
0:35
let's call that newName and newAge.
0:40
Okay?
0:41
So let's implement this method.
0:44
We know that we're gonna need to make a query.
0:46
So const result equals await db.query,
0:50
and then string template, so I can do multiple lines,
0:53
and we want a UPDATE cats
0:58
SET name equals $1,
1:03
and age equals $2.
1:07
And then we need to specify WHERE,
1:10
so let's do that on its own line, just to clear this up,
1:12
WHERE id equals $3.
1:17
All right, and then we need to pass in,
1:19
the first $1 is the newName.
1:23
Second is newAge, and then third is an id,
1:27
where we're actually updating.
1:29
And let's also add a returning clause,
1:32
RETURNING id, name, and age.
1:37
And then once we get that back,
1:38
we can do exactly what we had up here.
1:42
If there is no returning value,
1:44
nothing is coming back from Postgres,
1:46
we'll throw an error, cat not found.
1:48
Otherwise, let's just return results.rows(0).
1:56
So this update method requires us to pass
1:59
both a new name and a new age.
2:01
It's not gonna work if we only try
2:04
and set a new age or a new name,
2:07
it's a full update to our cat.
2:09
Let's see if it works.
2:11
So I'll just duplicate my route here.
2:13
We'll do a patch route.
2:15
Actually, I guess this would be better as a put,
2:17
because we're updating everything on a cat and :/id.
2:21
We're going to await Cat.update,
2:26
and then we need to pass in the id.
2:29
So request.params.id.
2:33
And we'll also get from request.body.
2:37
We'll destructure name and age,
2:39
and then we'll pass that in, name, age.
2:43
And then that should give us a result.
2:45
Const cat = await, Cat.update,
2:49
and then we'll return res.json cat.
2:54
So this is a put request,
2:55
and we need to include an ID in the URL in the path,
2:59
as well as age and name in the body.
3:02
Let's try it.
3:03
Let's try sending a put request with some data,
3:08
which cat should we update?
3:09
Let's update Pausley.
3:11
So that's an ID of three, as a put request.
3:16
And then in here, we'll specify name is now Pausley.
3:21
It's gonna become Pompomface, and age was two.
3:27
How 'bout age is five?
3:30
All right, moment of truth.
3:32
It looks like it worked.
3:34
We get id of three still,
3:35
name is now Pompomface, age is five.
3:38
Let's take a look at all cats.
3:41
All right, that has been updated.
3:43
If we try and update a cat that doesn't exist,
3:45
like ID of eight as a put request, we do get cat not found.
3:51
So that's not too bad,
3:52
but let's say I also wanted to add in a way
3:54
to update just the age.
3:56
I want to have a route,
3:58
we'll call it a patch route this time,
4:01
it will be /:id as well.
4:03
And I want it to increment the age of a cat.
4:06
So I could also do /:id/makeOlder or something,
4:11
but I'll just do a patch route.
4:12
We have put to update the entire cat patch.
4:15
We'll update the age by one.
4:18
It's just gonna hard-code it,
4:19
it will add one year to a cat's age, the cat with this ID.
4:24
So actually I'll just copy this to save myself some time,
4:28
and we'll change that to a patch request.
4:30
And in here I can't call Cat.update,
4:34
unless I also happen to know a name,
4:36
and I know the age that I want to set, but I don't.
4:39
I would have to first find that cat using
4:43
get, what'd we call it?
4:44
Find by ID, getById.
4:46
And I could do that.
4:48
Or I could just make another method called Cat.makeOlder,
4:54
and that would require an ID.
4:56
And then we'll just write the code
4:57
to increment the cat with this ID, increment its age by one.
5:02
So we don't need age or anything in the body.
5:05
Await Cat.makeOlder, req.params.id.
5:08
We can respond with the cat.
5:10
So then over here, we'll need to duplicate this method.
5:15
We'll call it makeOlder,
5:18
and it requires an ID as an argument.
5:21
Update cats, and then we'll just set age
5:25
to be age + one, WHERE id = $1.
5:31
We'll return everything.
5:32
And then we only have one parameter
5:34
to pass into that array, which is the ID.
5:37
If we get nothing back, throw that cat not found error,
5:40
otherwise return result.rows(0).
5:44
Okay, so this is our makeOlder function.
5:47
It's a method.
5:48
Let's see if that works.
5:51
So it automatically should increment a cat's age by one.
5:54
It's a patch request.
5:55
We don't need to include any data in the body,
5:58
so let's go look at a cat that does exist.
6:01
Do we have cat/3?
6:03
Yeah, Pompomface has age of five.
6:06
Let's send a patch request to cats/3.
6:11
Now age is six, seven, eight, nine, 10.
6:15
Now we're getting up into the range where, you know,
6:19
cat's not really gonna last much longer.
6:22
21, wow, that's about the oldest cat I know.
6:25
Anyway, this is working, but it's kind of annoying.
6:29
If I wanted to also have a method
6:31
just to change a cat's name.
6:33
That's kind of common.
6:34
Maybe not with cats,
6:36
but it's common to have some table and some rows
6:39
where you only want to update one field at a time.
6:42
You may want, let's say we had a blog post,
6:45
right, a blog post table,
6:47
and each blog post has a title, it has an author,
6:52
it has up votes or just total votes.
6:55
And you may want to update title.
6:58
Occasionally you'll let a user edit the title,
7:00
the owner of the post,
7:02
but then you'll be updating the votes all the time.
7:05
If people are using your app, they'll be voting up and down.
7:07
So you'll need a route,
7:09
most likely where you just update votes.
7:12
It's annoying to have to update everything else,
7:15
just to update votes.
7:16
Like technically we could use this update
7:19
in order to update the age,
7:20
but we also would have to pass in the current name,
7:23
and just reset it to the same name.
7:25
So we made a standalone method, makeOlder.
7:28
So you could, if you were using the same approach
7:30
that we've been doing,
7:31
creating a bunch of static methods on a class,
7:34
for a blog post, we could have blog posts.changeVotes,
7:37
or upvote, downvote.
7:39
We could have blog posts.editTitle, blogposts.changeAuthor,
7:44
but that's kind of obnoxious
7:46
to have to set up all those different methods.
7:48
So the second approach
7:49
that we're going to take in the next video
7:53
is to create a slightly more advanced version
7:56
of a class, a model class.
7:59
So instead of a bunch of static methods,
8:02
it will be much more similar to something like SQLAlchemy,
8:05
still lightweight, nowhere near as full featured,
8:08
but we'll actually be instantiating instances
8:11
of a model class, and that will make updating a lot easier.
8:14
So this works, there's nothing wrong with this at all.
8:17
And we will compare and contrast
8:18
the two approaches at the end.
8:20
So this marks the end of our first approach.
8:23
Everything is a static method on a class, cat, dog,
8:27
chicken, pickle, blog posts, user, whatever it is,
8:30
these methods will make a SQL query of some sort,
8:34
and then respond with some data.
8:36
And that allows us to clean up our routes significantly.
8:39
90% of our route logic at this point,
8:42
the actual code inside of a route handler,
8:45
is really just try and catch,
8:46
and then returning something or returning next.
8:49
Otherwise the SQL stuff is pretty much just a line.
8:53
It's a method we're calling.
8:54
A lot nicer, it's still not the final evolution.
8:58
We're gonna level it up in the next video,
9:00
but this is still pretty good.
9:02
(soft music continues)