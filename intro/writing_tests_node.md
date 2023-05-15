can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?





0:04
Next up, let's start writing tests for our CRUD Actions.
0:08
Remember, these slides are for cats.
0:10
But the idea is the same.
0:11
We have a RESTful API for users,
0:14
or cats if you're using the cats code
0:16
that you can download along with my code.
0:18
And
0:19
in either case,
0:20
we have full RESTful routes to test.
0:23
We have the ability to get all cats,
0:25
a list of cats,
0:26
to get a specific cat,
0:28
to Post a new cat,
0:30
to Create a new cat,
0:31
to Update a cat,
0:32
to Delete a cat.
0:33
So let's test those various actions.
0:35
And we'll start by testing reading.
0:38
So if we want to test the GET slash cats
0:40
or slash users route,
0:42
what we'll do is basically the same thing we did before.
0:45
So we await request dot get,
0:48
and then some path like slash cats
0:50
or slash users.
0:51
And then we'll check the status code.
0:53
And then we'll check the response dot body.
0:55
And then when we actually look at the body,
0:57
we'll use toEqual,
0:59
and we'll pass in
1:00
or we'll compare it against our testCat
1:02
or a test user,
1:03
or whatever we made in our before each.
1:08
So at least in this case, this makes it easy.
1:10
We just have one cat or one user in the database.
1:13
So we just put the array braces around it
1:15
to indicate that we expect
1:16
that we'll get a list,
1:17
we will get a list it just will be one thing long.
1:20
Alright.
1:21
So now we'll make our request.
1:23
So const response equals await.
1:26
And then we have our request function,
1:28
which is coming from Supertest.
1:29
And then we pass in our app.
1:32
And then dot get where did you go
1:35
dot get
1:36
slash users.
1:39
And after that, we can write our expectations.
1:43
So expect
1:44
res dot status code
1:47
dot
1:48
toBe
1:50
200.
1:52
Let's just start with that.
1:53
Run our tests over here,
1:54
jest
1:58
and what is it complaining about?
2:00
Oh, I didn't make an async function.
2:03
That was dumb.
2:04
Try that again,
2:05
I can't use a weight without async.
2:08
Alright, so now we're passing.
2:09
So now let's write our tests for the body.
2:11
So expect
2:14
res dot
2:15
body
2:17
dot toBe
2:18
not toBe toEqual
2:20
toBe is going to be a comparison
2:22
for the exact reference in memory.
2:25
We want to compare the contents of the arrays
2:28
or in this case, an object.
2:30
So we want it toEqual an object.
2:32
And if we look at our routes,
2:34
we're returning where are you?
2:37
Oh, it looks like we're just returning an array
2:40
as of the users, we're not actually following
2:43
this exact pattern.
2:45
So our JSON is just going to be the array in there.
2:50
So if we want it to, I think this will work,
2:53
we'll have to verify test user.
2:58
Let's see is that working?
3:01
Yeah, it does work.
3:02
I would probably update though,
3:04
and change my route here.
3:06
Instead of res dot JSON results dot rows,
3:08
I would res dot JSON and object
3:10
and then have a key of users set to results dot rows.
3:16
It's just a better way
3:17
of returning that JSON in my opinion.
3:19
So we'll expect it to be users as a key.
3:22
And that needs to be an object.
3:24
There we go.
3:25
And then an array
3:26
containing whatever users are in the database,
3:29
but there's only one so we can write our test,
3:31
saying there's exactly one which is test user.
3:37
And we're still passing great news.
3:39
Alright, next up,
3:40
let's duplicate this
3:42
and write a test for our next route,
3:45
which is reading a single cat or a single user,
3:49
let's actually add a route in.
3:50
Right now we have this search route,
3:53
which is slightly different.
3:54
It's expecting a query string.
3:56
And it's using that query string
3:58
to figure out multiple matching users.
4:01
Let's define a single route.
4:03
Just take this one here,
4:05
the router dot patch,
4:06
and this route will be a get request.
4:09
So I'm gonna update that to be get slash id.
4:13
And we'll take that id,
4:15
we don't need anything from the body.
4:17
We're going to update this query to be a select.
4:22
So SELECT
4:23
star
4:25
FROM
4:26
users
4:28
WHERE
4:29
and then we want id equals and then dollar sign one,
4:34
which will be this id here.
4:36
And we'll pass in, id like that.
4:39
And then we'll return res dot send
4:43
user
4:46
and then results dot rows zero.
4:50
Let's verify that this route is working on its own.
4:53
So I'm gonna start up my app nodemon.
4:55
Now I use that server js file
4:57
because I moved the server logic into its own file.
5:00
So we send our get request,
5:01
let's look at users slash two,
5:03
which should be Jenny,
5:06
and it is,
5:07
we get user
5:08
id: two
5:09
name: Jenny
5:09
type: staff.
5:10
Alright, so I'm gonna stop my server,
5:12
notice that our test database
5:14
is completely separate, right,
5:15
we're not inserting
5:16
or deleting from this database in our tests,
5:19
that's just a sign things are working well.
5:21
So now we'll write a test for this route slash with an id.
5:26
So GET slash users slash colon id,
5:29
and we'll go with Gets a single user.
5:33
So we're going to do the same thing
5:34
except we know we have one user in there
5:38
with the idea of one,
5:39
which we if you want it to double, double, double, triple,
5:42
quadruple check and be certain,
5:44
we could use a string template literal,
5:46
rather than hard coding that id.
5:48
And we could do testUser dot id right there.
5:53
And then we'll expect the status code to still be 200
5:56
we'll expect the response to be user
5:59
and then
6:00
not an array,
6:01
but just testUser like that.
6:04
Let's try running our tests.
6:08
And
6:09
it looks like it worked.
6:10
Two passing great.
6:11
Next up, let's write a test for the same route
6:14
that I know is going to fail.
6:16
And then let's make it pass.
6:18
So right now,
6:19
in our users, js routes file
6:22
for this get route with the id.
6:24
If you pass in an id that isn't found
6:26
to like 77, or zero,
6:29
that query still runs,
6:30
and it doesn't give us an error,
6:32
it's just not gonna give us any rows.
6:34
If you write a SELECT in Postgres,
6:36
and you don't find any matching rows,
6:39
sequel doesn't freak out at you,
6:40
it doesn't you know make a big fuss,
6:42
it just doesn't give you any results.
6:44
So same thing here,
6:45
when we're using our pg package,
6:47
pg or db dot query,
6:50
we're not gonna get an error.
6:51
So this try and catch is not going to catch an error.
6:54
If the id isn't found,
6:57
there are other things that could go wrong of course,
6:59
like the database not being found,
7:01
or the connection being screwed up somehow.
7:03
But in order to make sure
7:05
that we tell the user,
7:06
let's save status code 404,
7:08
in a message saying invalid id or id not found,
7:11
we actually have to implement that logic ourself.
7:14
So if we write a test,
7:16
just gonna duplicate our current test,
7:18
and update the string here to be
7:22
Responds
7:23
with
7:24
404 for
7:25
invalid id,
7:26
something like that.
7:27
And then we will hard code an id we know will be invalid.
7:31
Even if there's like 20 users in our database,
7:34
or 1000, or 10,000 in our test database,
7:37
there won't be a user with the id of zero.
7:39
And we'll expect the status code to be 404.
7:44
But at the moment, that won't happen,
7:46
we'll still get a 200.
7:48
It just won't include any data.
7:51
It won't find a user to respond with.
7:53
But it still tries to respond,
7:54
still gives us a 200.
7:56
So what we can do in here is add some logic
7:59
before we respond with this res dot send,
8:02
we'll check instead of results dot rows
8:05
is there anything.
8:06
So if
8:07
results dot rows
8:09
dot length
8:10
equals zero,
8:12
meaning nothing was found.
8:14
Why don't we make a new ExpressError?
8:16
So new ExpressError, and we'll throw it.
8:20
Let's go with a string template literal.
8:23
And then we'll just fill in the status code of 404.
8:26
And we'll say
8:27
can't
8:28
find user
8:30
with id of
8:31
and then we can add in id right there.
8:35
And save that to a variable
8:36
or we can just throw it right there.
8:39
That's fine.
8:40
I don't think we have ExpressError imported.
8:44
So let's import that.
8:45
ExpressError equals require
8:49
dot slash ExpressError,
8:51
lowercase.
8:53
And actually I think it's dot dot slash, isn't it?
8:56
We're in the routes folder.
8:59
Yep.
8:59
And it's over here.
9:01
So dot dot slash.
9:03
Alright,
9:04
let's see if this just works on its own first,
9:07
nodemon
9:09
server
9:10
js.
9:11
And we'll send a request to users slash zero.
9:15
And there we go,
9:16
we're getting our error,
9:17
can't find user with id of zero status is 404.
9:20
That's working.
9:21
So now if we run jest,
9:24
is it gonna pass?
9:26
And it does.
9:28
There we go three passing,
9:29
we see the console dot error,
9:31
which is just coming from our ExpressError.
9:33
We're printing out the stack,
9:35
but we are passing the test.
9:37
Okay,
9:38
so in the next video,
9:39
we'll flash out our tests a little bit more,
9:41
and we'll cover our patch routes, our post route
9:43
and the delete route as well.
