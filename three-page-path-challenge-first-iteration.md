[//]: # "title: Three Page Path Challenge - Thoughts and first iteration"
[//]: # "slug: three-page-path-challenge-first-iteration"
[//]: # "pubDate: 6/8/2024 10:22"
[//]: # "lastModified: 13/8/2024 10:22"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

[Make sure to read the problem scope in first article in this series.]('https://danielfrost.dk/a-man-with-a-three-page-path-challenge.html')

When looking at a programming challenge, I usually spend some time processing and mentally mapping some of the issues at hand. I doubt this is a unique trait for me, but perhaps I spend more time than others thinking before writing any code. I like to come prepared, and I don't really like surprises. I am not a particularly fast programmer, but when I come prepared, it usually helps. I put pressure elsewhere in the beginning. But I still iterate during my implementation.

Looking at this challenge, I thought the most interesting part of the program would be how to partition the log entries into some kind of pattern which could be used later.

If the log entries look like this and our first task is to resolve the question, "What is the most common three-page pattern for all users?" my first thought was to somehow group the log entries by each user. But since the question I need to programmatically answer states "what is the most common 3-page pattern..." I also thought that I needed to partition the paths in groups of 3.

Right off, I started to think about how to make this perform decently with a million rows, which irritated me because that thought would lay there in the back of my mind while I wrote code, and I needed to put that aside.

```
public struct LogEntry
{
    public LogEntry(int userId, string path, int loadTime)
    {
        this.UserId = userId;
        this.LoadTime = loadTime;
        this.Path = path;
    }

    public int UserId { get; }
    public int LoadTime { get; }
    public string Path { get; }
}
```

I start by writing a test, of course, since TDD will help both me design the code for the better as we go along and later guard me from regressions, which is what tests, to me, are all about.

```
private IEnumerable<LogEntry> FakeEntries_For_Three_Users()
{
    var entries = new List<LogEntry>
    {
        new LogEntry(1, "1.html", 20),
        new LogEntry(1, "2.html", 10),
        new LogEntry(1, "3.html", 30),
        new LogEntry(1, "7.html", 20),
        new LogEntry(1, "5.html", 10),
        new LogEntry(1, "8.html", 30),
        new LogEntry(2, "1.html", 20),
        new LogEntry(2, "2.html", 10),
        new LogEntry(2, "3.html", 40),
        new LogEntry(2, "9.html", 20),
        new LogEntry(2, "12.html", 10),
        new LogEntry(2, "36.html", 30),
        new LogEntry(3, "6.html", 20),
        new LogEntry(3, "3.html", 10),
        new LogEntry(3, "4.html", 30),
        new LogEntry(3, "1.html", 20),
        new LogEntry(3, "2.html", 22),
        new LogEntry(3, "3.html", 30)
    };
    return entries;
}
```

This should be easy to comprehend mentally because the log entry list at hand is short and will make my code execute faster and it will be easier for me to debug because I can calculate results by hand.

There are 6 entries with the userId=1. If we want to pair six entries into groups of 3, we should have 4 pairs:

> 1.html \
2.html \
3.html \
 \
2.html \
3.html \
7.html \
 \
3.html \
7.html \
5.html \
 \
7.html \
5.html \
8.html

I thought about this for a short while, while writing a bit of code, and circled around some different approaches I could take to overcome this.

- UserId is the key since it's the user's different paths that are important.
- I need to iterate paths for every user and pair them in 3's.
- If I need to iterate the paths in pairs of 3's, what should I then return?

Since this is the first iteration, I didn't think about returning one collection only, which I actually haven't tried, but I have made a note that it would be worthwhile pursuing.

I want to make a note of how I perceive quality in code, which is not something I believe you can ever obtain in a first iteration. So I am not so concerned with naming, structure, and to some extent design. I am always trying my best at the moment I am trying it, knowing all too well as I go I will become wiser. When you program for money, the time factor from external pressure for pace is never in favor.

I went back to my tests and wanted to assert my pairing of the user's paths, using TDD; test, refactor, test, refactor, etc.

```
[Fact]
public void Common_Path_Partitions_For_User_Is_Partitioned_Correctly()
{
    //Arrange
    var sut = new CommonPathDeterminator(FakeEntries_For_Three_Users());
    
    //Act
    var partitions = sut.CommonPathPartitionsByUserId();
    var user1PartitionSize = partitions.Where(p => p.UserId == 1).Select(x => x.Paths).Count();
    var user2PartitionSize = partitions.Where(p => p.UserId == 2).Select(x => x.Paths).Count();
    var user3PartitionSize = partitions.Where(p => p.UserId == 3).Select(x => x.Paths).Count();
    
    //Assert
    Assert.True(user1PartitionSize == 4);
    Assert.True(user2PartitionSize == 4);
    Assert.True(user3PartitionSize == 4);
}
```

We exercise the CommonPathPartitionsByUserId as you might have spotted. I have a few more tests that exercise the same method, with different input, but I saw no reason to add them here.

```
public IEnumerable<CommonPath> CommonPathPartitionsByUserId()
{
    var groupByUser = logEntries
        .GroupBy(x => x.UserId)
        .Select(s => s.ToList());
        
    List<CommonPath> commonPaths = new();

    foreach (var entry in groupByUser)
    {
        logPartitions.Clear();

        var partitioned = CreateLogPartitions(3, entry);

        foreach (var partition in partitioned)
        {
            var paths = partition.Select(p => p.Path);
            var loadTimeSum = partition.Sum(s => s.LoadTime);
            var userId = partition.Select(u => u.UserId).FirstOrDefault();
            commonPaths.Add(
                new CommonPath(loadTimeSum, userId, paths)
            );
        }
    }
    return commonPaths;
}
```

Looking at this code while I write this, I have a thought about the nested loop. I cannot make an exact point to what I am trying to infer, I need to think and look at it more thoroughly. That's how my programming mind also works.

In the inner loop, I call ```CreateLogPartitions(3, entry);``` where ```entry``` is the list of log entries for a user.

The responsibility of the method ```CreateLogPartitions``` is to pair the paths for each user. During a TDD iteration, I ended up creating a new type; ```CommonPath```, which I use for holding the log entry pairs and a summed-up load time for each pair.

```
public class CommonPath
{
    public int UserId { get; }
    public int LoadTimeSum { get; }
    public IEnumerable<string> Paths { get; } = new List<string>();
    public CommonPath(int loadTimeSum, int userId, IEnumerable<string> paths)
    {
        this.LoadTimeSum = loadTimeSum;
        this.UserId = userId;
        this.Paths = paths;
    }
}
```

You might wonder what the method ```CreateLogPartitions``` looks like, but I am not going to show it to you just yet. Think about how you would do it yourself.

I made the return value of the method a ```List<IEnumerable<LogEntry>>``` and I made the method recursive. It didn't take me long to note this as something I wanted to address in the near future. Not that I do not find recursion useful, but I am not sure it helps the reader of the code in this case, as it might be *too much* for what is needed. I believe a simpler loop could do the same, and I want to try it in another iteration. The signature of the method looks like this, see if you can implement a recursive method with the same signature ```List<IEnumerable<LogEntry>> CreateLogPartitions(int partitionSize, IEnumerable<LogEntry> logEntries, int skip = 0)```

What we are left with, which is also exposed by looking at the test above, is a collection ```IEnumerable<CommonPath>```.

The first two entries in the collection:

> commonPaths[0] \
&emsp;LoadTime = 60 \
    &emsp;Paths = Count(3) \
    &emsp;&emsp;[0] = 1.html \
    &emsp;&emsp;[1] = 2.html \
    &emsp;&emsp;[2] = 3.html \
&emsp;UserId = 1 \
 commonPaths[1] \
&emsp;LoadTime = 60 \
    &emsp;Paths = Count(3) \
    &emsp;&emsp;[0] = 2.html \
    &emsp;&emsp;[1] = 3.html \
    &emsp;&emsp;[2] = 7.html \
&emsp;UserId = 1

So now I have a collection with all the possible 3-pair path patterns for each user. I have a few tests that I use for regressions, and I am at a point where I need to think about how I will count the occurrences of each pattern.

At this point, I thought the "worst part" was over and I could start laying the groundwork for somehow counting the occurrences in the ```IEnumerable<CommonPath>```. For that, I believed I needed a faster collection than a List and I also wanted to compose a different type than the ```CommonPath``` for holding the result.

Back to the tests. As I started writing a test, I composed a new type, which I circled around for exposing the result as a collection. It manifested into an immutable type like this. If I could get a collection like this as a result, I would be grateful.

```
public class CommonPathOccurrence
{
    public List<CommonPath> CommonPaths { get; } = new List<CommonPath>();
    public int OccurrenceCount { get; }
    public CommonPathOccurrence(int occurrenceCount, List<CommonPath> commonPaths)
    {         
        this.CommonPaths = commonPaths;
        this.OccurrenceCount = occurrenceCount;
    }
}
```

I know there are a few things that stick out here, but it's the first iteration, and as previously stated, I will re-iterate.

But I still needed to do some internal juggling for adding and increasing to this type, specifically since the type I made immutable, I would have to do a little more than if it were mutable.

```
[Fact]
public void Find_Most_Common_Three_Page_Path_Pattern_For_Three_Users()
{
    //Arrange
    var sut = new CommonPathDeterminator(FakeEntries_For_Three_Users());
    
    //Act
    var pattern = sut.ThreePagePattern(sut.CommonPathPartitionsByUserId());
    
    //Assert
    var commonPaths = pattern
        .Where(w => w.OccurrenceCount == 3)
        .SelectMany(s => s.CommonPaths);
    
    var allPathsEqual = commonPaths
        .All(a => a.Paths.SequenceEqual(commonPaths.First().Paths));
    
    Assert.True(allPathsEqual);
}
``` 

Remember the list of entries we are testing against. It has some users that each share the same path pattern of 1.html, 2.html, 3.html. That is the most common pattern; hence, it should occur 3 times in the collection variable "pattern".

```
public IEnumerable<CommonPathOccurrence> ThreePagePattern(IEnumerable<CommonPath> commonPaths)
{
    foreach (var commonPath in commonPaths)
    {
        var flatPaths = string.Join(string.Empty, commonPath.Paths);

        ExistingCommonPathOccurrence(flatPaths, commonPath);
        NonExistingCommonPathOccurrence(flatPaths, commonPath);
    }

    var orderedMostCommonThreePagePattern = commonPathOccurrences
        .OrderBy(o => o.Key)
        .Select(o => o.Value)
        .ToList();

    return orderedMostCommonThreePagePattern;
}
```

The two methods ```ExistingCommonPathOccurrence``` and ```NonExistingCommonPathOccurrence``` are private and make use of the ```commonPathOccurrences```, which is a ```Dictionary<string, CommonPathOccurrence>```.

Here are those two methods:

```
private Dictionary<string, CommonPathOccurrence> commonPathOccurrences = new();

private void NonExistingCommonPathOccurrence(string flatPaths, CommonPath commonPath)
{
    if (!commonPathOccurrences.ContainsKey(flatPaths))
    {
        var occurrence = new CommonPathOccurrence(1,
            new List<CommonPath>() {
                new CommonPath(commonPath.LoadTimeSum, 0, commonPath.Paths)
            }
        );
        commonPathOccurrences.Add(flatPaths, occurrence);
    }
}

private void ExistingCommonPathOccurrence(string flatPaths, CommonPath commonPath)
{
    if (commonPathOccurrences.ContainsKey(flatPaths))
    {
        var existing = commonPathOccurrences[flatPaths];
        var newCount = existing.OccurrenceCount;

        var newCommonPaths = new List<CommonPath>(existing.CommonPaths)
        {
            new CommonPath(commonPath.LoadTimeSum, 0, commonPath.Paths)
        };

        var occurrence = new CommonPathOccurrence(++newCount, newCommonPaths);
        
        commonPathOccurrences.Remove(flatPaths);
        commonPathOccurrences.Add(flatPaths, occurrence);
    }
}
```

I chose to flatten the paths and use that as my dictionary key. Since the ```CommonPathOccurrence``` is immutable, I have to remove the existing instance and add a new one with the old values and an increased count.

I use the ```CommonPath``` as a collection type within ```CommonPathOccurrence``` acting as a container, but looking at the composition and especially how I use that specific collection with the ```ExistingCommonPathOccurrence``` and ```NonExistingCommonPathOccurrence``` I think I can do a better design. This is still the first iteration, though, so I will come to it.

Let's look at the calling method again. I call the internal workings, which hand me back a dictionary, and then I order it by its key and convert it into a List. This seems expensive, so I will have to address that too.

```
public IEnumerable<CommonPathOccurrence> ThreePagePattern(IEnumerable<CommonPath> commonPaths)
{
    foreach (var commonPath in commonPaths)
    {
        var flatPaths = string.Join(string.Empty, commonPath.Paths);

        ExistingCommonPathOccurrence(flatPaths, commonPath);
        NonExistingCommonPathOccurrence(flatPaths, commonPath);
    }

    var orderedMostCommonThreePagePattern = commonPathOccurrences
        .OrderBy(o => o.Key)
        .Select(o => o.Value)
        .ToList();

    return orderedMostCommonThreePagePattern;
}
```

This leaves me with a result which the test above ```Find_Most_Common_Three_Page_Path_Pattern_For_Three_Users``` asserts to true. It's not ordered by the ```OccurrenceCount``` but my method ```ThreePagePattern``` is not yet named as such that it infers that it should come in some kind of order. Next iteration.

I am well aware that I have not answered all the questions for the challenge, but we are getting there.

So before I conclude this iteration, I will say that there are some programming left to do, especially around composition and naming. There might be some low-hanging fruits for performance gains, but I need to measure it, and I will focus on them thouroughly just yet.

When I run the code with a million rows, it times right now to around 7 seconds, which I believe is way too long. I will hopefully iterate my way through to better performance too.