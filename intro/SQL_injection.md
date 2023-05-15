can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?

0:00
(upbeat music)
0:04
- [Narrator] All right.
0:04
So we just saw how to make our first query
0:07
to a database using the PG package.
0:09
And as we learned,
0:10
it is asynchronous, very important to be aware of.
0:13
Fortunately, with async await, the syntax impact
0:16
is pretty minimal.
0:18
It's really easy
0:19
to understand what happens.
0:20
Then we have this,
0:21
we don't have to use callbacks, thankfully.
0:23
And we also don't even need to use promises explicitly.
0:27
Even though there are promises involved,
0:29
as we've learned with async and await.
0:31
This is just nice syntax on top of promises that
0:35
are already there behind the scenes.
0:37
But we haven't talked about errors,
0:39
And what happens if we have an error.
0:41
At the moment, my code works fine.
0:42
If I send a get request to slash users,
0:45
but let's suppose that something were to go wrong.
0:48
And I'll just force it to start,
0:50
I could have a syntax error in here,
0:52
or I could be looking for a table
0:53
that doesn't exist, like that.
0:56
What do you suppose will happen,
0:57
when I send a request to slash users,
1:00
and I hit this callback?
1:02
let's try it.
1:05
Never a good sign.
1:06
We're not getting a response.
1:08
What happened?
1:09
Well,
1:10
our server crashed.
1:12
We've got a problem here.
1:13
It says, "Unhandled promise,
1:14
rejection, warning, error relation, blah, blah,
1:16
blah does not exist."
1:18
So that's the error that we got in here.
1:20
Relation does not exist,
1:21
but because this is an asynchronous function
1:24
and async function,
1:25
that's actually kind
1:26
of a distinction,
1:27
but it's an async function.
1:29
Remember that, that returns a promise,
1:31
and we have an unhandled promise, rejection warning.
1:35
And that's a problem.
1:37
If we had any issue in an async function,
1:40
even if I was just trying to return,
1:42
something that doesn't exist.
1:44
Blah, blah, blah, dot log,
1:46
my server restarts.
1:47
Okay.
1:48
I send a request,
1:49
same thing.
1:51
Now that's a really silly example,
1:53
but we have the same problem,
1:55
even though we hard-coded some syntax,
1:57
it's not a syntax error,
1:58
but an error,
1:59
that we knew it had definition
2:01
or a reference error.
2:03
But at the end of the day,
2:03
it's the same problem.
2:04
Unhandled promise rejection warning.
2:07
Now, if this were not an async function,
2:09
if it was just a regular callback here.
2:11
What do you think will happen here?
2:13
Well, let's try it.
2:14
Now We just get a regular old response.
2:17
It is an error, it status code of 500,
2:19
here's the message.
2:21
So that's simple enough, right?
2:22
We're using that default error handler that we set up.
2:25
So it's not the default,
2:26
but we're falling back on that error handler
2:28
from app js.
2:29
But that's not going to work,
2:30
with what we have at the moment, for an async function here.
2:34
So in order to make that work,
2:37
we have to use what we've already been doing.
2:39
We need to use, try and catch,
2:41
which is how we handle errors anyway,
2:43
in an async function.
2:44
Outside of express,
2:45
if we're making an Axios request
2:47
in the browser.
2:48
We still use,
2:49
try and catch.
2:50
Inside of an async function.
2:52
Just like with a promise,
2:53
we're going to have a dot then
2:54
and a dot catch.
2:56
So that's actually what's happening.
2:58
There is a promise here,
2:59
and it's been rejected
3:01
or it's not been resolved.
3:03
So let's add try,
3:06
and then wrap that around that
3:08
at a catch.
3:11
Like that,
3:12
and then we'll call next of error,
3:14
which is what we've already seen.
3:16
And earlier,
3:17
when we first introduced this pattern of try,
3:19
catch,
3:20
next error.
3:21
To return that, by the way.
3:23
I mentioned early on,
3:25
this may not make a difference at the moment,
3:26
but when we get to asynchronous operations,
3:28
like making a query from our database.
3:31
Which has to be asynchronous,
3:33
we could use a dot then I guess,
3:34
but anyways. It's an asynchronous operation,
3:37
so we just an async function.
3:38
Having errors in here,
3:40
is problematic unless we try and catch them.
3:43
So let's try it out now.
3:46
Send my request,
3:47
and now we are getting an error message.
3:50
We may not want this error message
3:51
to make it to a user
3:53
relation, blah, blah, blah does not exist.
3:55
This is coming from the PG package.
3:57
So you could add your own error on top of that
4:00
or your own error message, On top of that rather
4:02
you could wrap that,
4:03
you could do something
4:05
other than what we have here.
4:06
This would be a pretty uncommon error anyway.
4:08
You're not gonna to hard-code the incorrect table.
4:11
But now at least our errors,
4:13
aren't breaking our server.
4:14
We're still responding with something.
4:16
So this is a very important pattern to keep in mind.
4:18
When we're writing handlers, route handler callbacks,
4:21
that are async functions.
4:23
We need to make sure that we try
4:24
and catch any errors,
4:26
and then call next with
4:27
that error, or some other error that we use,
4:29
to create based off of the originating error.
4:32
So we could handle it differently.
4:34
We could pass in some new error here instead,
4:36
new express error.
4:38
Do I have (indistinct) no. I haven't included it here,
4:40
or required it.
4:41
But we could make a new express error.
4:43
And write a custom message and accustomed status code.
4:46
All right. So next we're going to continue using db.query,
4:49
but we'll try some other queries
4:51
that aren't hard-coded.
4:52
So that's coming up.
4:53
(Upbeat music)