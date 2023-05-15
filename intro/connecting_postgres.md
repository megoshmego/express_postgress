can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?

[Instructor] Now that we have talked about
0:05
the approachful use using the pg SQL driver,
0:08
let's actually do it.
0:10
Let's install it and write some basic queries.
0:13
Now, before I do that I wanna show you the basic skeleton
0:15
of the app that I'm gonna use.
0:17
I don't wanna make you watch me set up an Express app
0:19
from scratch and deal with the Express Error and starting
0:23
the server and everything.
0:24
So what I have here is a very basic skeleton
0:26
for an Express app.
0:28
Require express I've already npm installed it,
0:31
and package.json there it is express.
0:34
And I have my app variable, express error
0:38
the same express error class we've been using.
0:41
I am telling my app to use express.json that middleware
0:44
to parse the body as json of every request.
0:47
I have a routes file called users in the routes directory.
0:51
It doesn't actually have any routes defined yet.
0:53
What we'll be doing here is writing a route let's say /all,
0:57
which should select all users.
1:00
It's a user's routes we'll be working with users,
1:02
a users table, select all users from the database
1:05
and render them or respond with them as json.
1:09
But at the moment I've just set up a router I export
1:11
the router, we now need to fill in the gaps
1:13
and define our routes.
1:15
And then I have my error handling so 404 and then
1:18
the general error handler and then I'm starting my app,
1:21
app.listen on port 3000.
1:23
So that's all there, nothing crazy nothing new,
1:26
but now let's introduce a database.
1:28
So we'll be using Postgres as we've already discussed.
1:31
And the first thing I wanna do is set up a database to use.
1:34
So I have a file called data.sql you can download this
1:38
and it just makes a very simple table called users.
1:42
Each user has an ID, it's auto incrementing, a primary key,
1:46
it has a name which is text it can't be null and a type
1:49
which is also text and it can't be null.
1:51
So name like Jenny or Juanita, and then type of user would
1:56
be admin, staff or just user user, generic user.
2:00
So then I'm inserting six or so different users
2:03
and this has nothing to do with node.
2:05
At this point it's just a SQL file.
2:08
I'm going to run this file and if you're following along
2:11
and you're trying to code with me, I would run it as well.
2:14
So in my terminal, that file is data.sql.
2:18
Make sure psql is still working, alright?
2:21
And then easiest way to run it is psql and then I want
2:24
to run data.sql, alright.
2:29
So it made me my database, it created a table, it inserted
2:32
some stuff let's just connect to psql.
2:36
Connect to that database which is usersdb
2:39
and then select star from users.
2:43
And we do have some users, alright.
2:45
So that's a starting point.
2:46
Now we need to connect this to our node app.
2:49
So I'm gonna get out of here and clear.
2:53
How do we do that?
2:54
Well, this is where the pg package comes in.
2:57
It's similar to that psycopg2 package that
3:00
we used in Python.
3:02
We needed that as a dependency for SQLAlchemy work
3:05
with Postgres that was the adapter to take Python and make
3:09
it work with Postgres.
3:11
PG and node is the same thing it's a similar concept.
3:15
It establishes a connection to a database we tell
3:17
it what database and then it gives us a way
3:19
of actually executing a query.
3:22
So to install it, npm install pg.
3:25
I already have my package.json it has express in there,
3:28
npm i or install pg, alright?
3:31
So we now have that package hopefully we have
3:34
our database created we have some stuff in there.
3:37
Let's look at package.json, there's pg in node modules.
3:41
You'll see pg in here somewhere as well as a whole bunch
3:44
of dependencies, pg-int8, pg-connection string, pg-pool,
3:48
pg-types, pg-pass.
3:50
All that stuff has to do with Postgres and we needed it when
3:53
we installed pg or it needed it.
3:56
So to use this package there's a couple of things
3:58
we need to do.
3:59
One, is require it of course, two, we need to tell
4:02
it what database to work with and how to connect
4:04
to that database.
4:05
And then the third step is to actually query our database
4:09
and get some data back.
4:10
So typically we'll have some logic to connect to
4:13
the database and to establish that connection and we'll put
4:16
that in a file called db.js.
4:19
And I'll step through each line here.
4:21
I'm gonna start by just copying it over
4:23
into a new file db.js.
4:26
So close down node modules don't need you.
4:30
And then I'll just make that file in the root
4:33
of my project db.js, alrighty.
4:37
So there's a couple of things in here.
4:39
One, first of all, we require pg specifically we're after
4:43
this thing called Client so we're gonna destructure it.
4:47
You could do this right, pg and then just reference,
4:51
pg.Client everywhere but if we're only after clients this is
4:56
the easiest option.
4:57
Then what we're doing here is setting up
5:00
two different databases that we could possibly use
5:03
two different database URLs.
5:04
And actually the database I just created is usersdb.
5:08
I called it usersdb so we'll do usersdb test.
5:11
This will allow us to have two different databases one
5:14
for testing and then one for our main application
5:17
and development and then we have this conditional here,
5:20
if process.env.node environment NODE_ENV is equal to test,
5:25
this will be the URI that we're using.
5:27
Now, at this point we're not involving pg at all.
5:30
We're just making a variable DB_URI and
5:32
we're conditionally setting it to this or to this URL.
5:36
Two different databases, at the moment I don't even have
5:38
this database created but when we actually test
5:41
if you remember, we set up at the top of our testing file
5:46
or a test file process.env.NODE_ENV equals test.
5:51
And I said that when we wrote test with a database this
5:53
would be important so that's what we're doing here.
5:56
When we run a test and we set this environment, NODE_ENV
5:59
to be test we'll be using this database instead
6:02
of our regular database.
6:03
We don't wanna write test against our real information
6:05
our real data, we'll set up a test db.
6:08
But for now, this is the only database I've created.
6:10
Then onto this line here new client, which is what
6:13
we imported from pg.
6:15
We pass in an object to configure it and the one thing
6:18
we need to give it is the database URI.
6:21
Where is our Postgres database that we're trying
6:23
to connect to?
6:24
What is the URL or URI technically?
6:26
And it's this, it must we're running in test but we're not
6:30
at least not right now.
6:31
So that is how we pass it in, in an object.
6:34
ConnectionString is the property name and then DB_URI.
6:38
And then we call db.connect, db is a name of that variable
6:42
that starts up our connection.
6:44
Then module.exports equals db.
6:47
Now, I can import this db from some other file like in
6:51
our users file where I'm defining the routes.
6:54
I'll now be able to require db and have access to it
6:58
and on db I'll have different methods like db.query,
7:02
which I can call to make a SQL query, alright.
7:05
So this is our setup, I'm gonna save this file.
7:08
Now, what do we do?
7:09
Well, the one method that we are most interested in is
7:13
this db.query method.
7:15
So let's import db will require it in our users.js.
7:20
And remember the structure here, I'm in a routes directory.
7:23
So to require db I'll just call it db.
7:27
I can't do ./db, I need to do ../ to go back one directory
7:34
into this folder, into the db.js file.
7:38
So that gives me db.
7:40
Now, let's set up a route we'll call it slash how
7:42
about just slash.
7:44
In our app js, we are using all the user routes under
7:47
the prefix /users.
7:49
So I'll define a get route on my router to get all users so
7:55
that would be router.get/ if we're being restful
7:58
which is actually /users when we use these routes
8:01
and then we have our callback request and response.
8:06
And what I wanna do is get all the users from the database.
8:10
We just set up that connection hopefully it works.
8:13
And then I wanna select all users and
8:15
then respond with them.
8:16
And the syntax for that is db.query and then we pass
8:21
in a SQL string.
8:22
Select star from users just like we ran in
8:25
the Postgres console just a moment ago.
8:27
So let's save that to a variable const results equals db
8:32
which we have required .query and then select star from
8:38
the users table, okay.
8:41
So now what?
8:42
Well, if everything goes well that will give
8:46
us a value inside of that results variable
8:48
that has a property called rows.
8:51
So let's just try responding with that,
8:55
return res.json results.rows, alright.
9:00
So that is our first route, let's see what we get back.
9:04
So I'm gonna start up my server.
9:06
Am I in the right directory, yep?
9:08
Nodemon app.js local host 3000, I'll use insomnia
9:14
and I'll send a get request to local host 3000 /users.
9:20
Nobody returned for response why is that?
9:24
Right here we did respond with something, right?
9:26
Res.json results.rows.
9:29
But the fact that there was nobody there it wasn't like
9:32
it returned something else it just responded with nothing.
9:36
It was like results didn't exist.
9:38
And that's exactly the problem.
9:41
What's going on?
9:42
Well, these queries are asynchronous.
9:44
They take some time and unlike Python, JavaScript
9:48
is not just gonna sit here and wait for this to finish
9:50
before it tries to respond with this json.
9:53
So this might take just a split second but this code
9:56
is gonna run before this finishes.
9:58
Just like we've seen with making an Axios API call,
10:01
if you have some code right after you make your request,
10:04
you don't know and in all likelihood you're not going
10:07
to be able to have the data back before
10:09
that second line runs, which is why we use promises
10:12
or callbacks and async functions and that's exactly what
10:15
we're going to do here.
10:16
We're going to wait for that query to finish
10:19
by making an async function and awaiting db.query.
10:23
So db.query actually returns a promise and we can await it
10:26
as long as we have an async function.
10:29
Await, so now this line will not run until this promise
10:33
has been resolved.
10:34
When it's hopefully resolved, we have results.
10:36
It has a value in it and then we can return
10:39
res.json results.rows, okay.
10:42
Moment of truth here same request.
10:45
Hey, take a look.
10:46
What did we get back?
10:48
While results.rows is gonna give us a JavaScript array
10:51
and in that array we have objects.
10:53
And in each object we have an ID, a name and a type.
10:57
So that's our data everything is working so far,
11:01
db.query we pass in our SQL query it will connect
11:04
to that database and run this query that gets data back
11:08
and then we get this object called results here.
11:10
And if you wanted to, we could put a debugger right here
11:15
and then run this.
11:17
So nodemon --inspect app.js and then we hit the debugger.
11:27
We'll go into our Chrome console, open that up
11:32
and let's take a look at what results looks like.
11:35
It's not defined, really?
11:39
What do they call it?
11:41
What?
12:00
And then if we hit that end point again because right now
12:04
our debugger has not actually been started.
12:07
Oh my gosh, so many windows here, right?
12:09
We only run debugger, it will pause.
12:12
It's adding a break point when we hit this route.
12:14
So I still need to send that request.
12:17
We send it and here we go.
12:19
So let's go into our console look at request.
12:21
We have response and we have this results object that comes
12:26
from that pg package we've been using
12:29
that database .query call and inside of it, it tells us
12:33
the command that we ran, it shows us the number
12:35
of rows we got back and then rows contains our data.
12:39
It's an array containing objects.
12:42
So I'm gonna get rid of our debugger now but we've seen
12:45
that our db.query call returns a promise and eventually
12:49
the resolved value from that promise contains
12:51
a .rows property which will be an array of the matching rows
12:55
for each row as an object.
12:57
So it makes it very easy to send back as json.
13:00
It's already just a JavaScript array which is valid json
13:03
and we respond with it.
13:05
And that concludes our very first query using pg.
13:08
So next we will expand upon this and add more routes,
13:11
not just selecting but what about selecting based off
13:13
of some variable, an ID or something which can get tricky
13:17
when we talk about SQL injection.