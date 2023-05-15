can you please evaluate the following transcript for important terms, ideas , and give a simple demonstration of the concepts for review?


let's focus more on completing our full CRUD
0:08
set of actions set of routes here,
0:10
and we'll begin with creating a new user.
0:13
So the route to create a user,
0:15
if we're following res full conventions
0:18
which we're kind of not because we have slash search,
0:21
but anyway if we are,
0:23
it would be a POST request to slash users.
0:25
So router dot post slash nothing,
0:29
and then we'll have our async function,
0:31
request response next, our try and our catche.
0:38
And then in here, what we're going to do
0:40
is take the request body and we're expecting a user
0:44
or somebody who's making the request
0:46
to include a name and a type.
0:49
I don't need that autocomplete
0:50
but we need a name and a type,
0:52
and the ID will be automatically generated.
0:54
So we need to extract name and type.
0:56
So let's just use destructuring const name,
0:59
common type equals request dot body,
1:02
and we have our body being parsed as JSON
1:06
thanks to this line in our app,
1:07
app dot use Express dot JSON.
1:10
Then we want to run our query.
1:12
So const results equals await DB dot query,
1:17
but our query this time will be a insert into users
1:23
and then we need to specify what
1:25
we're actually inserting in the order
1:27
so we'll do name comma type and then we have our values.
1:32
So this is regular old SQL,
1:34
if we went into our data dot SQL file, just like this.
1:38
Insert into users name comma type values,
1:41
and then the actual values.
1:43
But of course the difference is that
1:44
we're going to parameterize here.
1:46
So we'll do dollar sign one followed by dollar sign two,
1:49
and then we pass in our array
1:51
with the two values in that order.
1:53
So if we're inserting name first,
1:55
then dollar sign one needs to be name
1:58
meaning the first element here is name,
2:00
and then the second element will be type.
2:04
All right, so we have that done.
2:06
Now, we'll just return res dot JSON results dot rows,
2:13
and let's see what happens.
2:15
And if there's an error, we'll catch it
2:17
and then call return next with that error.
2:22
So let's try sending a POST request
2:24
to slash users with some new data.
2:27
So POST, get rid of this query string,
2:30
slash users without the search
2:32
and then we'll add some JSON in here,
2:34
we need name will be set to Jellybean
2:40
and type will be just what should we do?
2:45
I guess admin, we kind of have a lot of admins
2:48
for this website, but whatever.
2:50
Alright, let's send it.
2:53
Okay, did it work or did it not work?
2:56
Do we have an error?
2:57
No, so it looks like it worked,
2:59
but we're not getting anything back, why is that?
3:02
Well, when we insert something if you think about it,
3:04
just running a regular SQL query in our POSTgres shell,
3:08
inserting something is not gonna return any information
3:11
to us it's just inserting it,
3:13
it will give us an error if it doesn't work,
3:15
but we're not selecting any data.
3:17
So results dot rows it's not really what we want to return
3:20
at least, we're not getting any rows
3:22
like if we just print out results,
3:28
and maybe I should add a debugger.
3:30
Aah whatever, we'll just do console dot log results.
3:33
Just trust me, rows is not gonna contain what we want,
3:36
it's just an empty array.
3:37
So if I go to slash users as a GET request,
3:42
Jellybean was created but we're not actually
3:45
getting that data back and using it.
3:47
We can't use it cause we're not getting anything back,
3:50
it's just giving us back an empty results.
3:52
The rows is empty so if I make one more request,
3:56
let's do Jelly, I don't know Jam,
4:00
who is an admin send a POST request,
4:03
what do we get printed out?
4:04
Here's our result, it says command is insert
4:06
row count is one rows that we got back is zero.
4:10
So we didn't get anything back.
4:12
So to fix that, we could make another select,
4:16
we could make a select statement,
4:18
but rather than doing that there's an easier option,
4:20
which is to use a returning clause.
4:23
So when we insert or update or delete in SQL,
4:26
we can add on a returning clause,
4:28
this is just a SQL thing,
4:29
it's not a Node thing or PG thing,
4:32
and returning is how we specify what we want back.
4:36
So based off of what we just inserted,
4:38
I want the ID and the name,
4:40
or I want ID and name and type or just returning star.
4:44
Probably just gonna do returning star here,
4:46
so let's do that.
4:48
Right here, before the end of our query,
4:51
returning star or we could be explicit
4:56
we want ID name and then type.
5:02
Totally up to you how you want to structure that.
5:04
So let's make one more user and see if it works.
5:08
So POST request, instead of Jam,
5:10
what's another J name?
5:12
Jasmine, I guess Jared,
5:15
and Jared will be a regular user.
5:18
Send that as POST request and now we're getting back
5:22
that information that was just created,
5:24
ID is nine name is Jared type is user.
5:27
If we had any other information in there,
5:29
we would be getting it back.
5:30
Well actually, with what we've written,
5:32
we'd only get ID name and type back,
5:34
but if we had returning star,
5:36
or if we added on some other thing
5:38
that we're hoping to get back,
5:39
we now get that information back
5:41
and it's contained in results dot rows
5:43
which is what we're sending back.
5:45
One minor change I would make is change
5:47
the status code here to be two zero one for created,
5:52
instead of a default 200.
5:54
But otherwise, we're now creating a new user,
5:56
and we're responding with data back
5:58
that we're getting from that one query,
6:00
we don't have to write two queries.
6:02
So we're getting the insert to work
6:03
and that returning clause is going to send us back data
6:07
that is automatically stored in results dot rows.
6:11
One tweak we could make is that
6:13
we are returning all rows, results dot rows,
6:16
even though we only have one row in there
6:18
and there should only be one because
6:19
we're only inserting one thing,
6:21
so we could do send back res dot status is two zero one
6:24
dot JSON results dot rows square brackets zero,
6:28
we just want that first row, that first object.
6:31
So we'll send one more Jeremy I guess,
6:36
who's also a regular user.
6:37
Now we get two zero one first of all,
6:39
that's our new status code created,
6:41
and we just get the object the JSON data back
6:44
without the extra array braces.
6:46
And now we are successfully and safely creating a new user.
6:50
We are using our parameterization that we just learned,
6:52
we are using the returning clause we just learned,
6:55
good stuff.
6:56
(upbeat music)