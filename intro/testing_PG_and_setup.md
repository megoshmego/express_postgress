can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?




0:00
(upbeat music)
0:04
- [Tutor] Next step let's test our post route
0:06
to create a new user.
0:09
So we'll start with our describe
0:12
and we'll go with POST /users.
0:16
Our callback in here,
0:18
our test creates a single user
0:24
and then our async callback
0:26
because we'll be using our request
0:28
from test clients and then we'll make our requests.
0:32
So const response equals await request
0:38
our app variable.post and it should just be /users
0:44
and then to send that data,
0:45
we use dot send or to send data in general,
0:48
not that data we need to actually add the data
0:50
we want to send
0:51
and our route is expecting a name
0:54
so let's go with name of BillyBob,
0:59
no caps and then a type of just regular user
1:04
actually how about staff.
1:05
We don't have very many staff.
1:08
So that should make our post requests with that data
1:11
and then we can write our expectations,
1:13
expect(res.statusCode).toBe
1:20
I think it's 201, is what we went with,
1:22
the created status code instead of 204.
1:25
And then the slightly trickier part
1:27
is expecting the response.body toEqual
1:32
and it's not as simple as just having an equal test user
1:36
or some hard coded entity, some hard coded data
1:40
where, yes we know names should be BillyBob,
1:42
type should be staff,
1:43
but what about id?
1:45
We could technically hard code it
1:47
and just assume if we're deleting everything from users
1:50
there is only one user in there
1:52
so that's id of one.
1:53
So we would assume id is two
1:56
but it's better to make our tests
1:57
a little bit more future-proof,
1:58
if we wanted to let's say,
2:01
create two users before each test
2:04
that would break this test here
2:06
if it is passing,
2:07
which we still don't know if it's passing,
2:09
expect(res.body).toEqual,
2:12
we know it's going to have user as the key
2:14
assuming that's what we did here
2:16
and it's actually not so we should update that.
2:21
We don't have to follow this convention
2:23
but if we're following it for some of our routes,
2:25
it's probably best user: results.rows[0].
2:29
And then here user,
2:31
we expect it to have a name of BillyBob
2:36
and we expect it to have type of staff,
2:40
but then id is a little trickier
2:43
instead of hard coding it
2:44
what we can do is just expect any number
2:48
as we saw early on,
2:49
when we talked about Jest,
2:50
expect.any and then you pass in a constructor like number,
2:55
and this will pass,
2:57
our tests will pass
2:58
assuming id is any number.
3:02
All right, let's see if that test passes
3:05
and it does, okay for passing.
3:08
Next, let's test our patch route to update a user.
3:14
Before we do that, I wanna update my response here,
3:17
just to follow the same pattern we've been doing
3:19
user: results.rows[0]
3:23
and also we're not testing to see
3:26
or we're not checking to see if we found a user
3:29
so we could copy the same logic.
3:32
Where is that?
3:32
Right here,
3:33
if results.rows.length is zero.
3:36
So if that id can't be found,
3:39
let's throw a new ExpressError cannot find user
3:42
or how about cannot update user with id of that id?
3:47
All right, so that route is now configured.
3:50
Let's write our test.
3:51
I'm just gonna copy what we have,
3:54
described slash our PATCH /users/:id
3:59
and we'll go with updates a single user for our test string
4:03
and we'll send a request.post needs to be dot patch
4:08
and instead of /users, let's update our test user,
4:11
so I'm gonna use my string template literal again,
4:14
/users/ and then testUser.id
4:20
and then in terms of what we update,
4:23
let's send name, yeah, sure Billy Bob is fine,
4:28
type is staff, sure that's also fine.
4:31
Maybe we'll spice it up a bit.
4:32
We'll do admin there.
4:35
Our route is not set up right now
4:37
to only accept one of these
4:39
like we can't just update one portion, name.
4:43
If you look at our routes here,
4:45
it is taking exactly name and type
4:49
and trying to set both of them.
4:51
So if one of them is left off,
4:53
we'll have an error
4:54
and we're not even handling,
4:55
well it's being handled
4:56
but we're not providing feedback.
4:58
It will just be a 500 server error.
5:00
So if that's something that you wanted to provide
5:02
you'd need to add some logic in
5:04
to allow for that
5:06
but for now, we'll just test what we have.
5:08
So type will be admin and name is BillyBob
5:11
and then we're going to expect status code to be 200
5:15
not 201 for created but 200
5:18
and then we'll expect(res.body).toEqual,
5:20
user we'll have id, instead of any number
5:23
we know that it's test user
5:26
because we're updating with testUser's id.
5:28
So testUser.id,
5:30
name will be BillyBob
5:31
and type should be admin.
5:35
Okay, moment of truth here.
5:36
We'll try it out.
5:41
Okay.
5:42
Every time I see that red text,
5:43
my heart skips a beat but we're passing.
5:45
That's just from the error
5:46
that we're generating purposefully.
5:49
Let's also write a test case
5:50
for what happens if you update a user
5:53
that doesn't exist.
5:54
Respond with 404 for invalid id.
5:57
So I'm just gonna duplicate my test
5:59
and then just remove most of this.
6:02
So we'll try sending request(app).patch /users/
6:06
we'll do zero again
6:08
and we don't even need to bother sending data
6:11
but if you want to,
6:12
we can and then we'll expect status code to be 404.
6:17
Great so that's passing.
6:19
Finally, we have our delete route.
6:21
So same story here.
6:23
We'll send a delete request.
6:26
Let's just copy this,
6:27
speed things up.
6:30
We'll start with just one test case
6:32
and this is a delete for that describe string,
6:36
deletes a single user
6:39
and then we'll send a request
6:41
as a delete request /users/testUser.id
6:47
and we don't need to send any data along with that.
6:52
And how are we responding here?
6:54
Res.send message deleted
6:57
and status code will just be the default 200.
6:59
So we can just do that,
7:01
expect(rest.statusCode).toBe(200).
7:04
Let's run our test
7:08
and we're getting seven passing.
7:11
And we could also test the response message deleted
7:15
is what we're looking for.
7:17
I don't know why I did all caps
7:18
but whatever expect(res.body).toEqual
7:24
an object where we have message set to DELETED like that.
7:30
Just verify we're matching.
7:32
All right, our final test.
7:34
Did they all run?
7:36
Great, it's working.
7:38
One thing that we are not testing at the moment
7:40
because we don't have logic for it
7:42
is what happens if you try and delete a user
7:43
that doesn't exist.
7:45
So that's something that you could implement.
7:47
Add some logic in
7:49
because right now let's see what happens
7:51
if we try and delete a user.
7:53
Let's do nodemon server.js
7:57
and try and delete a user with an ID that doesn't exist.
7:59
Let's look at all of our users first
8:01
so we can tell what ids don't exist.
8:04
All right, so let's do something like users/23.
8:07
I say, delete request.
8:09
You don't need to send anything.
8:11
We get messaged deleted.
8:13
So if you were to write a test,
8:15
you're going to get a 200 status code
8:17
and a message of deleted
8:19
even if you pass in an invalid id
8:21
and that's because our app right now doesn't care.
8:24
So you may want to add some logic into the delete route
8:26
to prevent a user from getting message deleted
8:30
even if nothing was actually deleted
8:32
and then you could write a test to test for that,
8:34
just like we did for our patch route.
8:37
We were testing to see,
8:38
oh, I never changed this string, did I?
8:41
We're not updating a single user here.
8:43
We are expecting,
8:45
let's go with responds with 404 for invalid id.
8:50
All right, are the other ones fine.
8:53
Responds with 404.
8:55
Now we have a whole bunch of tests
8:57
and you've seen how to test an express app
9:00
where you have a Postgres database
9:02
and the same would be true of really any database.
9:05
Maybe the queries would be different,
9:07
even if you're using a NoSQL database
9:09
something like Mongo,
9:11
you still follow this pattern.
9:13
If you're going to use supertest,
9:15
you're not gonna use the PG package that's for Postgres
9:18
but before each,
9:19
you'll typically make some sort of row in your database
9:22
or multiple rows depending on what your tests look like
9:24
and then after each you'll clear things out
9:27
and then after everything,
9:28
you can sever that connection to the database.
9:31
And that's kind of it
9:32
for the basics of testing and express app with a database.
9:36
(upbeat music)