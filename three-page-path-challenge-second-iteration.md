[//]: # "title: Three Page Path Challenge - second iteration"
[//]: # "slug: three-page-path-challenge-second-iteration"
[//]: # "pubDate: 19/8/2024 10:22"
[//]: # "lastModified: 19/8/2024 10:22"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

This is the third article in a series about a man with a three-page path challenge, an interview question that can come up for programmers.

In this article, I will refine and iterate further into the challenge and try answering all the challenge questions.

- What is the most common three-page pattern for all users?
- What is the frequency (how many times) of the most common pattern for all users?
- How many different three-page patterns are there in total?
- (Bonus) What is the fastest most common three-page pattern for all users?
- (Bonus) What is the slowest three-page pattern for all users?

[Make sure to read the problem scope in the first article in this series.](/a-man-with-a-three-page-path-challenge.html)

[You should also read the article on the first iteration of the problem scope of the first question.](/three-page-path-challenge-first-iteration.html)


I left off at the first iteration with a few thoughts:

> "So before I conclude this iteration, I will say that there is some programming left to do, especially around composition and naming. There might be some low-hanging fruits for performance gains, but I need to measure it, and I will not focus on them thoroughly just yet."

So, composition and naming were something I made a mental note of last time around, and it struck me while writing the last article that at least my naming was off. Composition is a different undertaking since it most likely will evolve in future iterations.

In the first iteration, I had used a single type for my implementation, which I had called ```CommonPathDeterminator```. The name has already left my mind since I have not only renamed it but also moved implementation details around to different types.

The name itself, ```CommonPathDeterminator```, was probably something I came up with while thinking, "this type can be used to determine common paths." A few days later, the name gives little sense. I find it too loose, I find the noun *Determinator* to be too vague, and really, the composition of the type is off.

But it was a fine start; it served its purpose for where I was at the time.

Let me outline the type ```CommonPathDeterminator``` from the first iteration before I reveal what I have done in this iteration.

- CommonPathDeterminator (class)
  - ThreePagePattern (public method)
  - CommonPathPartitionsByUserId (public method)
  - NonExistingCommonPathOccurrence (private method)
  - ExistingCommonPathOccurrence (private method)
  - CreateLogPartitions (private method)

Simply looking at the naming of the methods makes one wonder what is going on inside ```CommonPathDeterminator```, which makes the type name even more meaningless when we read the code inside the type.

This makes it all sound like the second iteration is a blessing and everything will be right this time around. Not at all. But it will be more right in some ways, and having given myself the feedback from the first iteration, around naming and composition, I knew where I wanted to focus my attention.

The implementation details and my idea of how to reach a result still stand. As an example of that, the partitioning of log entries is still in play.

I didn't show that piece of code in the first article, but it has changed little since, except I have extracted it out from the then-known ```CommonPathDeterminator``` and put it into its own type called ```PathPartition```.

```
public class PathPartition
{
    private readonly IEnumerable<LogEntry> logEntries;

    public PathPartition(IEnumerable<LogEntry> logEntries)
    {
        this.logEntries = logEntries ?? new List<LogEntry>();
    }

    public IEnumerable<UserPathPartition> PathPartitionsByUserId(int partitionSize)
    {
        var groupedLogEntriesByUser = logEntries
            .GroupBy(x => x.UserId)
            .Select(s => s.ToList());

        List<UserPathPartition> userPathPartitions = new();

        foreach (var userLogEntries in groupedLogEntriesByUser)
        {
            logEntryPartitions.Clear();
            var sizedLogPartitions = CreatePathPartitions(partitionSize, userLogEntries);

            foreach (var logPartition in sizedLogPartitions)
            {
                userPathPartitions.Add(
                    new UserPathPartition(logPartition)
                );
            }
        }

        return userPathPartitions;
    }


    private List<IEnumerable<LogEntry>> logEntryPartitions 
        = new List<IEnumerable<LogEntry>>();

    private List<IEnumerable<LogEntry>> CreatePathPartitions(
        int partitionSize, IEnumerable<LogEntry> logEntries, int skip = 0)
    {
        // no reason to continue if our log entries hold too little data
        if (logEntries.Count() < partitionSize)
        {
            return logEntryPartitions;
        }

        // skip and take because we are in recursion and must diminish the log entry size
        var skippedLogEntries = logEntries.Skip(skip);
        var logEntriesPartition = skippedLogEntries.Take(partitionSize);

        // partition entry must match wished size of partition
        if (logEntriesPartition.Count() == partitionSize)
        {
            logEntryPartitions.Add(logEntriesPartition);

            return CreatePathPartitions(partitionSize, logEntries, ++skip);
        }

        return logEntryPartitions;
    }
}
```

This takes a collection of log entries, partitions them grouped by a user's ID, and converts them into a collection of ```UserPathPartition```.

This still leaves us with a result that looks like the one I had in the first iteration. But the holding type I have changed somewhat.

> userPathPartitions[0] \
&emsp;TotalLoadTime = 60 \
&emsp;FlattenedPaths = 1.html2.html3.html \
    &emsp;Paths = Count(3) \
    &emsp;&emsp;[0] = 1.html \
    &emsp;&emsp;[1] = 2.html \
    &emsp;&emsp;[2] = 3.html \
&emsp;UserId = 1 \
userPathPartitions[1] \
&emsp;LoadTime = 60 \
&emsp;FlattenedPaths = 2.html3.html7.html \
    &emsp;Paths = Count(3) \
    &emsp;&emsp;[0] = 2.html \
    &emsp;&emsp;[1] = 3.html \
    &emsp;&emsp;[2] = 7.html \
&emsp;UserId = 1

The Count is not part of the type; it's merely to visualize the number of entries in the Paths collection.

```
public class UserPathPartition
{
    public int UserId { get; }
    public int TotalLoadTime { get => this.Paths.Sum(x => x.LoadTime); }
    public IEnumerable<LogEntry> Paths { get; }
    public string FlattenedPaths { get => Flatten(); }

    public UserPathPartition(IEnumerable<LogEntry> paths)
    {
        this.Paths = paths ?? new List<LogEntry>();
        this.UserId = this.Paths.Select(u => u.UserId).FirstOrDefault();
    }

    public string Flatten()
    {
        string flattenedPaths = string.Empty;
        foreach (var logEntry in Paths)
        {
            flattenedPaths = flattenedPaths + logEntry.Path;
        }

        return flattenedPaths;
    }
}
```

The flatten method is the only outlier here, and it resides here because I had a small spike during this iteration regarding a small performance optimization when handling larger data sets, and believe I found that when iterating with a ```foreach``` instead of doing ```return string.Join(string.Empty, Paths.Select(logEntry => logEntry.Path));``` it came to the foreach's favor.

You can call it a premature optimization, but I like having these small spikes during the iterations, simply for gaining the most out of what I am trying to do. Why not have the two methods from ```PathPartition``` be part of ```UserPathPartition```? I'm iterating gradually, and I am getting there slowly.

The type from the first iteration, ```CommonPathDeterminator```, has been renamed to ```PathPatterns``` and now looks like this.

```
public class PathPatterns
{
    private readonly IEnumerable<UserPathPartition> userPathPartitions;
    private readonly Dictionary<string, PathPattern> pathPatternOccurrences;

    public PathPatterns(IEnumerable<UserPathPartition> userPathPartitions)
    {
        this.userPathPartitions = userPathPartitions ?? new List<UserPathPartition>();
        this.pathPatternOccurrences = new Dictionary<string, PathPattern>();
    }

    public IOrderedEnumerable<PathPattern> OrderByOccurrenceDescending()
    {
        pathPatternOccurrences.Clear();

        foreach (var pathPartition in userPathPartitions)
        {
            var flattenedPaths = pathPartition.FlattenedPaths;

            ExistingPathPattern(flattenedPaths, pathPartition);
            NonExistingPathPattern(flattenedPaths, pathPartition);
        }

        var orderedPatternOccurrences = pathPatternOccurrences.Values
            .OrderByDescending(p => p.OccurrenceCount);

        return orderedPatternOccurrences;
    }

    private void NonExistingPathPattern(string flattenedPaths, UserPathPartition userPathPartition)
    {
        if (!pathPatternOccurrences.ContainsKey(flattenedPaths))
        {
            var occurrence = new PathPattern(1,
                new List<UserPathPartition>() {
                    new UserPathPartition(userPathPartition.Paths)
                }
            );

            pathPatternOccurrences.Add(flattenedPaths, occurrence);
        }
    }

    private void ExistingPathPattern(string flattenedPaths, UserPathPartition userPathPartition)
    {
        if (pathPatternOccurrences.ContainsKey(flattenedPaths))
        {
            var patternOccurrence = pathPatternOccurrences[flattenedPaths];
            var count = patternOccurrence.OccurrenceCount;

            patternOccurrence.PathPatterns.Add(
                new UserPathPartition(userPathPartition.Paths)
            );

            var occurrence = new PathPattern(++count, patternOccurrence.PathPatterns);

            pathPatternOccurrences.Remove(flattenedPaths);
            pathPatternOccurrences.Add(flattenedPaths, occurrence);
        }
    }
}
```

So to recap, this leaves me with two types instead of the initial bastard type ```CommonPathDeterminator```. Now I have ```PathPatterns``` and ```PathPartition```. I am not saying this will be the final result, but I like it better than the first iteration.

This is how the ```PathPattern``` type looks:

```
public class PathPattern
{
    public List<UserPathPartition> PathPatterns { get; }
    public int OccurrenceCount { get; }

    public PathPattern(int occurrenceCount, List<UserPathPartition> pathPatterns)
    {         
        this.PathPatterns = pathPatterns ?? new List<UserPathPartition>();
        this.OccurrenceCount = occurrenceCount;
    }
}
``` 

All along this iteration, my tests have, of course, also transformed a bit. Like I stated in the first article, they serve as great regression guards, but I must also admit that they have changed drastically because the code they exercise has changed drastically too.

To actually answer the questions for the challenge, I have decided in this iteration to write a type called ```PathPatternsAnalyzer```.

- What is the most common three-page pattern for all users?
- What is the frequency (how many times) of the most common pattern for all users?
- How many different three-page patterns are there in total?
- (Bonus) What is the fastest most common three-page pattern for all users?
- (Bonus) What is the slowest three-page pattern for all users?

While writing the tests for the type ```PathPatternsAnalyzer```, I had made a constructor for the type, looking like this: ```public PathPatternsAnalyzer(IOrderedEnumerable<PathPattern> orderedPathPatterns)```. I soon realized I had cornered myself, and it would mean my composition would be poor.

I have no good way to check the actual partition size of the argument being passed to the constructor. Remember that ```PathPattern``` has no mechanism for this and is simply a holder for data. If using this type as a constructor parameter as I had done, I could easily cheat and put any number of arbitrarily sized partitions into it. We don't want that because the goal is to look for path patterns that are sized into a specific number of partitions.

We would not be able to answer the first question, "What is the most common three-page pattern for all users?" if one partition is of size 6 and another is of size 4. They have to be sized as 3.

That means I have to add some defensiveness to my ```PathPatternAnalyzer``` type. I will take a look at that in iteration 3.