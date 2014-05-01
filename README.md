# RedditSharp

A partial implementation of the [Reddit](http://reddit.com) API. Includes support for many API endpoints, as well as
LINQ-style paging of results.

```csharp
var reddit = new Reddit();
var user = reddit.LogIn("username", "password");
var subreddit = reddit.GetSubreddit("/r/example");
subreddit.Subscribe();
foreach (var post in subreddit.New.Take(25))
{
    if (post.Title == "What is my karma?")
    {
        // Note: This is an example. Bots are not permitted to cast votes automatically.
        post.Upvote();
        var comment = post.Comment(string.Format("You have {0} link karma!", post.Author.LinkKarma));
        comment.Distinguish(DistinguishType.Moderator);
    }
}
```

**Important note**: Make sure you use `.Take(int)` when working with pagable content. For example, don't do this:

```csharp
var all = reddit.RSlashAll;
foreach (var post in all) // BAD
{
    // ...
}
```

This will cause you to page through everything that has ever been posted on Reddit. Better:

```csharp
var all = reddit.RSlashAll;
foreach (var post in all.Take(25))
{
    // ...
}
```

## Development

See [RedditSharp](https://github.com/SirCmpwn/RedditSharp#redditsharp)