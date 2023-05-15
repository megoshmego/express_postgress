can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?


with updating a user and deleting a user.
0:09
So to update, we'll write a patch request,
0:12
and we'll use the ID to reference a particular user,
0:16
and then we'll take some new name and some new type
0:18
from request.body,
0:20
it's important to note at the moment
0:21
we're not validating any of our data,
0:23
so these could be blank,
0:25
these could be yeah, they could be missing,
0:27
which could be a problem.
0:29
We'll learn later on how to write JSON API as an express
0:31
where we do validate JSON data,
0:34
but at the moment, we're just trusting a user
0:35
which is never a good thing.
0:37
And then we will write a query,
0:39
which is an update query,
0:40
so update users set blah, blah, blah, blah.
0:43
So let's play around with that,
0:45
let's write our route,
0:46
so router.patch/ID, as a variable,
0:52
async, request, response, next,
0:58
and then our try,
1:00
and our catch in case we have any errors,
1:03
and then we want to extract the name,
1:06
so const, name and type equals request.body,
1:11
so those needs to be sent with the body.
1:13
And then we'll make our query,
1:15
const results equals await DB.query.
1:21
and then we'll update users set,
1:25
and then we want name to be equal to $1,
1:30
if that's the order we're going with,
1:31
so the first thing we pass in will be name,
1:33
and then $2 for type,
1:37
$2,
1:38
and then we need to finish the query
1:39
with a where ID equals $3,
1:45
now you can change the order,
1:46
if you want ID to be $1,
1:49
you just need to make sure that the order is correct
1:51
when you pass in your parameters.
1:53
So $1 is name,
1:55
so we'll pass name first,
1:56
$2 is type,
1:58
and then $3 is ID,
2:00
well, how do we get the ID it's not in request.body.
2:03
It's right up there,
2:04
so that's request.params,
2:06
so we could just make a separate variable for that,
2:09
or we could just pass it in here,
2:10
request.params.ID,
2:14
ID like that,
2:17
actually, you know, it's probably a better idea
2:19
to do it up here,
2:20
because then we could, you know, check if there was an ID
2:22
and throw some sort of error,
2:24
so let's do that, const ID equals request.params
2:28
.nothing, de structuring it.
2:31
And then if there is no ID,
2:32
we could add a conditional with throw an error.
2:35
And we're missing one thing
2:37
if we want to also show the return to the updated user,
2:41
we could use returning,
2:44
and then what do we want,
2:45
let's just get ID, name and type,
2:49
or just returning star,
2:51
so that will be our results,
2:53
and we want that returning once again,
2:55
so that we can return it res.send,
2:59
and then results.rows
3:03
of zero because we'll only get one thing back,
3:05
if there's an error we'll return next of error.
3:09
All right,
3:10
so let's just test it out,
3:11
there's always potential that we mess something up,
3:14
let's send a get request first to all users,
3:16
and let's update Jellybean who has an ID of seven.
3:21
So users/7 as a patch request,
3:24
I want to update,
3:26
how about name will be JellyBEAN in all caps,
3:30
and type will be user, sure,
3:33
JellyBean is currently admin,
3:35
let's send it as a patch request,
3:38
Oh, bind message supplies two parameters,
3:40
now I forgot to pass in the ID there.
3:44
So it did warn me,
3:45
it says you were going for three things,
3:47
but you only gave me two,
3:49
that's a problem.
3:50
So let's try it again,
3:53
there we go,
3:54
we now get ID of seven, still the same,
3:57
name is JellyBEAN in all caps,
3:59
and then type is user.
4:01
Let's update one more,
4:03
let's just get, a get request of all users,
4:06
let's update,
4:09
who else do we have?
4:10
How about Jam with an ID of eight,
4:13
so /8 has a patch request,
4:17
we'll set name to now be,
4:20
GrapJame,
4:22
and then type, we could just keep it as admin,
4:25
which is what it is right now,
4:27
send and there we go, Grap,
4:29
let's do Grape,
4:31
All right, and it did update,
4:33
send a get request,
4:34
we can verify all of our users are still there,
4:37
but now we have GrapeJam,
4:39
oh my goodness, I screwed it up again, GrapeJame, yikes.
4:44
it's still our update is working,
4:45
an ID is eight, type is admin.
4:47
Next up, let's add in a delete action,
4:50
so a delete route using an ID where we can delete a user.
4:54
So router.delete,
4:57
we want /ID as a delete request,
5:00
async, request, response, next
5:05
arrow function or just a regular old function,
5:07
and then try catch,
5:11
and then the query that we want to write
5:12
is going to be something like this,
5:15
const results equals DB.query,
5:18
and then delete from users, where ID equals,
5:25
and then $1,
5:27
so, $1 needs to be the ID for deleting by ID,
5:33
so we need to get that ID,
5:34
and it's probably just easiest to do request.params.ID
5:38
just for now.
5:39
Although if you did wanna check if there was an ID,
5:42
if it's a valid ID,
5:44
you could pull this out first and write some logic,
5:46
and then throw an error, if it's not,
5:48
but this is fine.
5:49
So DB.params, or request.params.ID,
5:52
whatever that ID is,
5:53
we will attempt to delete based off of that,
5:56
$1, first parameter there in that array,
6:00
and if it does, we don't really have anything to return,
6:02
so or to respond with from the database,
6:05
we could do a res.send
6:07
something like,
6:09
I don't know, message deleted,
6:13
if you wanted to,
6:14
and then, as always return next error if there is an error,
6:19
let's see if it works,
6:21
so who do I want to delete?
6:23
Let's take a look,
6:25
let's do all users as a get request,
6:28
let's delete sure this GrapeJame with ID of eight,
6:32
so users/8, as a delete request,
6:36
don't need to send any data,
6:38
although that shouldn't really cause any issues,
6:40
and we get message deleted.
6:42
Well, let's verify that,
6:44
let's go with a get request to /users,
6:46
I don't see her or him GrapeJame is no longer here.
6:51
We have one, two, three, four, five, six,
6:53
eight is missing, nine, 10, seven.
6:56
So that's it, we now have a delete route.
6:59
So our SQL is really running the whole thing, right?
7:02
It's just regular old SQL, nothing fancy,
7:04
no methods really other than DB.query,
7:08
we're executing a query it takes time,
7:09
in all of these instances,
7:11
so we use an async function, we await the query,
7:14
hopefully it's resolved, if not,
7:16
whatever error is thrown, it's caught,
7:18
we return next with that error,
7:20
which hits our error handler, very fancy.
7:24
And that's it for the basics of working with PG,
7:27
and writing our queries,
7:28
we talked about returning clauses,
7:30
we talked about SQL sanitization
7:32
to avoid SQL injection attacks.
7:35
One thing that I didn't highlight
7:36
that you've probably noticed by now,
7:38
is that there's no committing involved,
7:40
with SQLAlchemy,
7:41
we would call some methods to hopefully make a change,
7:44
but then we would have to actually commit
7:46
in order for SQLAlchemy to send a true query
7:48
to the database.
7:50
But we're not going through
7:51
any sort of layer of abstraction like SQLAlchemy,
7:54
we're just directly talking to our database,
7:56
we don't need to explicitly commit,
7:58
when we run DB.query we're directly interacting
8:01
with the database right away.
8:03
We're not going through some extra layer,
8:05
that we then have to truly send off.
8:07
But by now, you probably already know that,
8:09
because we haven't committed anything
8:11
we've just been writing DB.query this whole time,
8:13
and those changes are taking effect in our database.
8:17
So this works great,
8:18
but we will see some nice things that we could add on,
8:21
we'll see how we could write our own lightweight ORM.
8:24
But before we do that,
8:25
we're gonna take a break to talk about testing,
8:27
now that we have API set up an express app with a database,
8:31
a real database,
8:32
how do we test this new logic,
8:33
how do we test that things are working
8:35
and that we're actually inserting or deleting?
8:38
So that's next.
8:39
