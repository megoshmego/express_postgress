can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?


(upbeat music)
0:04
- [Instructor] Next up, let's keep building out
0:06
our cat model class.
0:08
Let's add in some functionality for creating a new cat.
0:12
So if we're following RESTful conventions,
0:14
we'll add a route as a post request to slash
0:19
so /cats as a post,
0:21
which should make a new cat.
0:24
So add in our standard handler here,
0:26
request response next
0:30
and I'm just gonna copy this to save some time
0:32
and what we would like to do, what would be nice,
0:35
is if we could just call something like Cat.create
0:40
and then pass in some age and a name or name and age
0:44
and name and age are supposed to be
0:46
in request.body.
0:47
We do have express.json being used in our app right there.
0:53
So request.body should contain both name and age,
0:56
assuming they've been passed in as part of the request.
0:59
So we can extract those const
1:01
name, age equals request.body.
1:04
And then I could pass that in to name, to name.
1:08
I could pass name in to create and pass age into create,
1:11
which doesn't exist, and that could return
1:15
some data, right?
1:16
Hopefully will make me a new cat and we'll await it
1:20
because we know instead of Cat.create,
1:22
we're going to be using db.query, which takes time.
1:26
It will be an async function that returns a promise.
1:28
So let's head over to cat.js and implement a create method.
1:33
So static async create, which accepts a name and an age
1:40
and then we're going to do a query
1:42
that will be an insert query
1:44
so const, we'll go with result again,
1:46
equals await db.query,
1:50
and then insert into cats
1:57
and then we'll do name and then age values
2:02
and then parametrize things so $1, $2,
2:06
and then we'll add in our parameters,
2:08
which will be named first.
2:10
That's what we listed first, name and then age.
2:14
And if we want to return something from this function,
2:17
if we wanna return the newly created cat information,
2:21
we should probably add on our returning clause.
2:24
And I think I'd like to just break this up
2:27
into a couple of lines,
2:29
Um meh, maybe like that for now,
2:33
and then add in our returning,
2:35
returning ID name and age.
2:39
Let's see if that works for us.
2:41
Let's close that there.
2:44
And then that should give us our result
2:46
and then after that we'll return
2:50
result.rows zero.
2:56
Okay, let's see what happens.
2:59
So over here, we are awaiting cat.create.
3:03
Make sure this is an async function here.
3:05
We're awaiting db.query to insert that new cats.
3:09
Hopefully we get some ID, name and age back,
3:11
and then we can return that result.rows zero.
3:15
and then here, we should just make sure
3:16
we're sending that cat back.
3:18
So return res.json cat.
3:23
Let's try making a new cat.
3:24
So post request to /cats.
3:27
We'll need to add in name,
3:29
which will be Jello Mittens,
3:34
and an age,
3:37
which will be four.
3:40
Send.
3:42
It looks like it worked.
3:43
We got an ID, we've got our age and our name.
3:46
Let's send a get request to /cats
3:49
and there's Jello Mittens.
3:51
So we created a cat.
3:52
Now, what if one of these were missing?
3:55
For example, if we left name off.
3:57
Right now, I don't even remember what the database
4:00
is set up as.
4:01
If it's going to validate any of this or not validate,
4:04
but if there are any requirements, if name is required,
4:08
yeah, it is required.
4:09
No value in column name.
4:11
Violates not-null constraint.
4:13
What we could do is just add some validation in,
4:16
in our method inside of cat.
4:18
Instead of doing it here in our route,
4:21
we can move that logic, that validation logic, into cat.
4:25
So we could just do something like if not name
4:30
or not age,
4:35
we can throw a new express error
4:38
and just say missing required data.
4:43
And we'll probably just do a 400 status code
4:45
for bad request.
4:47
Make sure we have our try-catch over here, and we do.
4:51
All right, so try that again.
4:53
Now we get missing required data status, 400.
4:56
Same thing if I had named, but left off age.
4:59
Actually, I don't know if we want to require age.
5:04
Is there a default for age?
5:06
Hmm, maybe I shouldn't have done that.
5:08
Anyway, regardless of what our database supports,
5:11
our create method requires both name and age.
5:15
Now in a bit, a couple more sections here,
5:17
we'll learn how to validate json data
5:19
so we can create APIs.
5:21
At the moment we're just looking for name and age
5:24
and we would have to do some manual checking
5:26
of the format of our data from request.body.
5:29
We'll see a better solution later on.
5:31
But for now we have our create method.
5:33
No SQL right here.
5:35
It's all moved into our cat class.
5:38
So let's also do a delete
5:40
so that we can delete a cat by ID.
5:42
So router.delete with an ID,
5:47
async, request response next,
5:52
and then our try catch,
5:57
return next error.
5:59
All right, I know it's a bit boring to watch me type that.
6:02
So it would be nice if we could just do cat.delete
6:05
or delete by ID and then pass in
6:08
request.params.id.
6:12
So that ID there, and we'll try deleting.
6:15
We want to await that, it doesn't exist,
6:17
but we know we want to.
6:20
Do we really need a response from that?
6:23
I don't think we really care what we get back.
6:25
It's up to us how we implement this method, cat.delete.
6:29
Are we going to return the deleted cat?
6:31
Do we return nothing?
6:33
What do we return?
6:35
I think we'll just await it and then return res.json,
6:40
something like message deleted.
6:46
So let's implement cat.delete, which accepts an ID.
6:50
So over here, another static async method
6:54
Call it delete and it accepts an ID.
6:58
And our query is going to be delete from cat
7:01
so we'll do a const result equals await
7:06
and then db.query.
7:10
Let's do delete from cats
7:15
where ID equals and then $1
7:20
pass in our parameters here, which is just ID.
7:24
And oh, I'm still inside my string aren't I?
7:27
I have got an extra quote there.
7:29
So that should do it for us.
7:31
Maybe move this down to its own line.
7:34
Out dent that, great.
7:36
And then we don't really need to return anything, do we?
7:39
We just await that.
7:41
And then we can await this promise over here
7:45
and then res.json message deleted.
7:48
Let's test it out.
7:50
So I'm gonna send a delete request.
7:52
Which cat do I want to delete?
7:53
Let's survey our cats.
7:56
We'll get rid of Jello Mittens.
7:57
I'm sorry Jello Mittens.
7:58
ID of four.
7:59
So cats/4 as a delete request.
8:03
We don't need any json data.
8:06
Message deleted.
8:08
Let's go back to all cats as the get request.
8:11
Yep, he's gone or she's gone.
8:13
Let's try deleting 56, which doesn't exist.
8:18
We still get that same message, message deleted.
8:21
There is no error involved because
8:24
we can try and delete something like we are here
8:26
and that query is going to succeed.
8:29
It's going to be sent and there's not gonna be an error
8:31
on the SQL side of things.
8:33
It's just not going to delete anything.
8:35
If you tried to delete directly in your PostgreSQL shell,
8:38
delete something with an ID of 1000, which doesn't exist,
8:42
it doesn't freak out at you,
8:43
it just doesn't delete anything.
8:45
So if we wanted to check if something was deleted,
8:48
we could use a returning and just return one thing.
8:51
We're not gonna actually send it back
8:53
out of this function,
8:54
but we'll in returning from this one query
8:57
and then we can check if result.rows.length equals zero,
9:04
we can throw a new express error, cat not found, 404.
9:12
So now if you try and delete something that doesn't exist,
9:15
we still try that, we do delete it, it's just doesn't exist.
9:19
And we have proof that it didn't exist because
9:21
nothing was returned.
9:23
We didn't get this returning clause kicking in.
9:25
And so we throw that new express error.
9:27
We've still got our try-catch over here.
9:29
Return next error.
9:31
All right, same thing.
9:32
Cats 56, cat not found, 404.
9:37
I don't think I wanna delete another cat, do I?
9:40
I guess I can delete Fluffy.
9:43
/1, delete request, goodbye, Fluffy.
9:47
And she's gone.
9:50
Yup, totally gone.
9:51
I'm sorry, I hope it was painless.
9:53
(upbeat music)