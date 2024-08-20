[//]: # "title: Three Page Path Challenge - third iteration"
[//]: # "slug: three-page-path-challenge-third-iteration"
[//]: # "pubDate: 20/8/2024 17:00"
[//]: # "lastModified: 20/8/2024 10:22"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

This is the fourth article in a series about a man with a three-page path challenge, an interview question that can come up for programmers.

[Make sure to read the problem scope in the first article in this series.](/a-man-with-a-three-page-path-challenge.html)

Also read about the [first iteration](/three-page-path-challenge-first-iteration.html) and the [second iteration](/three-page-path-challenge-second-iteration.html).

In this article, I believe I have refined myself into a solution I am satisfied with, and I have shown to myself how I have gone back and forth in my iterations for refining my mind and solution for how I can answer all the challenge questions. Can I consider the code finished? No! I believe this code can be optimized and nurtured even further, but the point has also been to positively iterate into a more quality solution, centered around code readability, maintainability, tests, and composition.

- What is the most common three-page pattern for all users?
- What is the frequency (how many times) of the most common pattern for all users?
- How many different three-page patterns are there in total?
- (Bonus) What is the fastest most common three-page pattern for all users?
- (Bonus) What is the slowest three-page pattern for all users?

Before writing this article I came to think about a few different quotes on data and code.

Linus Torvalds once said *"Bad programmers worry about the code. Good programmers worry about data structures and their relationships."*

Fred Brooks says something a little less provocative in his *Mythical Man-Month*:

*Show me your flowchart and conceal your tables, and I shall continue to be mystified. Show me your tables, and I won't usually need your flowchart; it'll be obvious.*

What I am getting at is an internal thought about if I could have either made a data structure from my initial data (LogEntry) or used different data structures for solving this challenge.

I have to pretend that it is needed, the optimization, otherwise it would be an optimization I hardly could justify. But when I run this third iteration on a million log entries and ask for the answer for the first question "What is the most common three-page pattern for all users?" it takes 13 seconds.

Jon Skeet said something along the lines, in his *C# in Depth*, "often performance optimizations hurt code readability." In the case of this iteration, a call to ```All``` on an ```IEnumerable``` takes around 10 seconds, and I could have left it out, but didn't.

That challenge, specifically, is "how do you determine if a property on an entry in a collection has the same value as all other entries in the same collection." There is a logical answer to this, but it exists only because I already have an ordered collection. I can take the first and the last entry, check the value on both. I have not changed the call to ```All```, you can try to do that. And it won't really hurt readability.

In the end of the second iteration, I concluded I was going into a corner and the composition was getting skewed. For many years I have been very aware of when I can actually program and when I need to leave it alone. Some days I am simply too tired to do actual quality work and if I do, I know I have to come back and change it. So I have to step back, relax, read, and focus on something else. You can get invested in something where you try to outsmart yourself. I think some of that happened in the second iteration. In the first iteration, I had also just returned from a 7-week vacation where I had not touched a keyboard or a codebase. These things can play with you. Be aware of that context.

Another note is while the number of iterations go up so does the value of my output. I have used a lot less energy in this iteration and that might be because it has been close to the second iteration but just as much because I have made a note of what was to happen. I knew what was wrong. I could go back and change that. In the first iteration coming into the second iteration I still had to thing about a lot more. 

In this iteration, I ended up with refactoring the type ```PathPatternsAnalyzer``` where one cannot cheat with the partitioning sizes, because the constructor parameter no longer is an ```IOrderedEnumerable<PathPattern>```. Now the full type looks like this.

```
public class PathPatternsAnalyzer
{
    private readonly IEnumerable<LogEntry> logEntries;
    private readonly int partitionSize;

    private IOrderedEnumerable<PathPattern> orderedPathPatterns = Enumerable.Empty<PathPattern>().OrderBy(p => 0);
    private IEnumerable<UserPathPartition> userPathPartitions = new List<UserPathPartition>();

    public PathPatternsAnalyzer(IEnumerable<LogEntry> logEntries, int partitionSize)
    {
        this.partitionSize = partitionSize;
        this.logEntries = logEntries ?? [];

        if (this.partitionSize == 0)
        {
            throw new ArgumentException($"{partitionSize} cannot be zero");
        }

        if (partitionSize > this.logEntries.Count())
        {
            throw new ArgumentException($"{partitionSize} is larger than the total " +
                $"entries in {logEntries} - {this.logEntries.Count()}");
        }

        PartitionUserPaths();
        OrderPathPatterns();
    }

    public PathPattern MostCommonPathPattern()
    {
        // no common paths, extremely slow!
        if (orderedPathPatterns.All(p => p.OccurrenceCount == orderedPathPatterns.First().OccurrenceCount))
        {
            return new PathPattern(0, new List<UserPathPartition>());
        }

        return orderedPathPatterns.First();
    }

    public IOrderedEnumerable<PathPattern> AllPathPatterns()
    {
        return orderedPathPatterns;
    }

    public UserPathPartition? FastestCommonPathPattern()
    {
        var mostCommon = MostCommonPathPattern();

        var fastest = mostCommon.PathPatterns.OrderBy(s => s.TotalLoadTime);

        return fastest.FirstOrDefault();
    }

    public UserPathPartition SlowestPathPattern()
    {
        var slowest = orderedPathPatterns
            .SelectMany(p => p.PathPatterns)
            .OrderByDescending(o => o.TotalLoadTime)
            .First();

        return slowest;
    }

    private void PartitionUserPaths()
    {
        var pathPartitions = new UserPathPartitions(this.logEntries);
        userPathPartitions = pathPartitions.PartitionedByUserId(this.partitionSize);
    }

    private void OrderPathPatterns()
    {
        var pathPatterns = new PathPatterns(this.userPathPartitions);
        orderedPathPatterns = pathPatterns.OrderByOccurrenceDescending();
    }
}
```

See how this type is rather small. It has small methods, it uses the types I made earlier in a simple manner and it answers questions around the data in a few lines. I like it, except for the O(n) time complexity with ```All``` in ```MostCommonPathPattern```.

The tests for this type are quite straightforward as well, due to the separated types which of course are tested separately.

Here are the tests for the ```PathPatternsAnalyzer``` which is also the answering sheet for the questions the challenge put forward.

```
public class PathPatternsAnalyzerTests
{

    [Fact]
    public void What_Is_The_Single_Most_Common_Page_Pattern()
    {
        //Arrange
        var sut = new PathPatternsAnalyzer(Log_Entries_For_4_Users(), 3);

        //Act
        var actual = sut.MostCommonPathPattern();

        //Arrange
        Assert.True(actual.PathPatterns.All(a => a.FlattenedPaths == "/home/profile/results"));
    }

    [Fact]
    public void What_Is_The_Frequency_Of_The_Most_Common_Pattern_For_All_Users()
    {
        //Arrange
        var sut = new PathPatternsAnalyzer(Log_Entries_For_4_Users(), 3);

        //Act
        var actual = sut.MostCommonPathPattern();

        //Arrange
        Assert.True(actual.OccurrenceCount == 2);
    }

    [Fact]
    public void How_Many_Different_Patterns_Exist()
    {
        //Arrange
        var sut = new PathPatternsAnalyzer(Log_Entries_For_4_Users(), 3);

        //Act
        var actual = sut.AllPathPatterns();

        //Arrange
        Assert.True(actual.Count() == 8);
    }

    [Fact]
    public void Fastest_Most_Common_Path_Pattern()
    {
        //Arrange
        var sut = new PathPatternsAnalyzer(Log_Entries_For_4_Users(), 3);

        //Act
        var actual = sut.FastestCommonPathPattern();

        //Arrange
        Assert.True(actual!.TotalLoadTime == 620);
    }

    [Fact]
    public void Slowest_Of_All_Path_Patterns()
    {
        //Arrange
        var sut = new PathPatternsAnalyzer(Log_Entries_For_4_Users(), 3);

        //Act
        var actual = sut.SlowestPathPattern();

        //Arrange
        Assert.True(actual.TotalLoadTime == 900);
    }

    [Fact]
    public void Constructor_Throws_Due_To_Partition_Size_Is_Larger_Than_Collection_Count()
    {
        //Arrange Act Assert
        Assert.Throws<ArgumentException>(() => new PathPatternsAnalyzer(Empty_Log_Entries(), 3));
    }

    [Fact]
    public void Constructor_Throws_Due_To_Partition_Size_Is_0()
    {
        //Arrange Act Assert
        Assert.Throws<ArgumentException>(() => new PathPatternsAnalyzer(Log_Entries_For_4_Users(), 0));
    }

    [Fact]
    public void What_Is_The_Single_Most_Common_Page_Pattern_No_Common_Paths_Exist()
    {
        //Arrange
        var sut = new PathPatternsAnalyzer(Log_Entries_No_Common_Paths(), 3);

        //Act
        var actual = sut.MostCommonPathPattern();

        //Arrange
        Assert.Equal(0, actual.OccurrenceCount);
        Assert.Empty(actual.PathPatterns);
    }

    [Fact]
    public void Fastest_Most_Common_Path_Pattern_No_Common_Paths_Exist()
    {
        //Arrange
        var sut = new PathPatternsAnalyzer(Log_Entries_No_Common_Paths(), 3);

        //Act
        var actual = sut.FastestCommonPathPattern();

        //Arrange
        Assert.Null(actual);
    }

    private IEnumerable<LogEntry> Log_Entries_No_Common_Paths()
    {
        var partitions = new List<LogEntry>
        {
            new LogEntry(1, "/login", 300),
            new LogEntry(1, "/home", 120),
            new LogEntry(1, "/profile", 200),
            new LogEntry(1, "/results", 300),
            new LogEntry(2, "/people", 300),
            new LogEntry(2, "/hello", 120),
            new LogEntry(2, "/contact", 200),
            new LogEntry(2, "/jobs", 300),
        };

        return partitions;
    }

    private IEnumerable<LogEntry> Log_Entries_For_4_Users()
    {
        var partitions = new List<LogEntry>
        {
            new LogEntry(1, "/login", 300),
            new LogEntry(1, "/home", 120),
            new LogEntry(1, "/profile", 200),
            new LogEntry(1, "/results", 300),
            new LogEntry(2, "/home", 300),
            new LogEntry(2, "/profile", 300),
            new LogEntry(2, "/results", 300),
            new LogEntry(3, "/home", 100),
            new LogEntry(3, "/login", 250),
            new LogEntry(3, "/dashboard", 250),
            new LogEntry(3, "/settings", 400),
            new LogEntry(4, "/login", 80),
            new LogEntry(4, "/home", 200),
            new LogEntry(4, "/dashboard", 200),
            new LogEntry(4, "/setting", 200),
            new LogEntry(4, "/search", 200),
            new LogEntry(4, "/people", 200)
        };

        return partitions;
    }

    private IEnumerable<LogEntry> Empty_Log_Entries()
    {
        var partitions = new List<LogEntry>();
        return partitions;
    }
}
```

I might return to this challenge in time, perhaps sooner than later, but for now I consider it at a satisfying state.

You can find all the code for the iterations here.