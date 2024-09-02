[//]: # "title: Three Page Path Challenge - fifth iteration"
[//]: # "slug: three-page-path-challenge-fifth-iteration"
[//]: # "pubDate: 2/9/2024 08:22"
[//]: # "lastModified: 2/9/2024 12:03"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"


This is the sixth article in a series about a man with a three-page path challenge, an interview question that can come up for programmers.

[Make sure to read the problem scope in the first article in this series.](/a-man-with-a-three-page-path-challenge.html)

Also, read about the [first iteration](/three-page-path-challenge-first-iteration.html), the [second iteration](/three-page-path-challenge-second-iteration.html), the [third iteration](/three-page-path-challenge-third-iteration.html), and the [fourth iteration](/three-page-path-challenge-fourth-iteration.html).

I had to question myself around the matter of explicitness and defensiveness in the code I had written. Arguments should be clear, but I had found some of what I had written to be too implicit. This is in relation to correctness but also somewhat about the future and how we can prepare for it. I used to say I am not concerned with the future when writing code because it has an impossibility that is self-explanatory. Live requirements are most often a sign to be read as "stop."

If we guess about exact needs, the correctness could turn into incorrectness. It's a guess. I still fall into the trap, guessing my own ability, sometimes taking it for something higher than it really is. That will be a strain on any human mind—guessing, hoping to be right, but not with a high possibility of certainty.

Anyways, to boil it all the way down, it should be as obvious as possible to a programmer how an API works when one uses it. So when you allow your fellow programmer to pass in a type as an argument which is not explicitly telling, you are not making it obvious. And you might be guessing, even though it might be unconscious.

### Here, I am training my abilities

For a constructor to accept an ```IEnumerable<T>``` but then change that type inside the constructor, e.g., to an ```IImmutableList<T>```, closes off the possible updating of an exact type. It will hide the fact—e.g., if the exact type of the ```IEnumerable<T>``` is a ```List<T>```—that one could manipulate that collection after initializing. That would not be possible in the same capacity with an ```IImmutableList<T>```.

Think about it for a bit. Should my ```LogEntry``` collection be left open for manipulation later in the execution chain? Why not? Why yes? Am I the one to determine that? Is it a business rule? Do I know the future? Which type is more open than the other? Will your code be easier to close or to open?

If I did this ```public PathPatternsAnalyzer(IEnumerable<LogEntry> logEntries, int partitionSize)``` and wrote a test like this, it would fail.

```
[Fact]
public void Constructor_Parameter_Can_Update_During_Execution()
{
    //Arrange
    var listOfLogEntries = Log_Entries_No_Common_Paths().ToList();
    var count = listOfLogEntries.Count;
    var sut = new PathPatternsAnalyzer(listOfLogEntries, 3);

    //Act
    // will add to the collection and affect the sut
    listOfLogEntries.Add(new LogEntry(1, "path", 100));
    
    //Assert
    Assert.True(count == sut.logEntries.Count());
}
``` 

Yet, changing the constructor argument to an ```IImmutableList<T>``` closes off that possibility to change the collection during execution. What is expected, and how do I decide?

```public PathPatternsAnalyzer(IImmutableList<LogEntry> logEntries, int partitionSize)```

While this test would then succeed:

```
[Fact]
public void Constructor_Parameter_Cannot_Update_During_Execution()
{
    //Arrange
    var listOfLogEntries = Log_Entries_No_Common_Paths().ToList();
    var count = listOfLogEntries.Count;
    var sut = new PathPatternsAnalyzer(listOfLogEntries.ToImmutableList(), 3);
    
    //Act
    // collection in sut will not be affected
    listOfLogEntries.Add(new LogEntry(1, "path", 100));
    
    //Assert
    Assert.True(count == sut.logEntries.Count());
}
```

I know it sounds trite, and you might read this and think, "why is this idiot not just using whatever type? Why all this?" That's a valid point, but you might be forgetting that this is a challenge in programming, and asking yourself these questions is exactly how you become a better programmer. The reflections made can be useful. The exploration of types, patterns, and options. 

I want to be explicit now, and therefore I will accept only ```IImmutableList<T>``` because it first and foremost signals to the programmer what to expect: that this constructor argument is immutable. I also believe that it's easier to understand since it is more predictable.

Bear in mind that it was never the goal that I wanted to create something immutable. It happened during the challenge, and it happened because I could take liberty to try it out while also finding it more correct at that moment.

You can find the [code for this fifth iteration here](https://github.com/Danielovich/LogParsingKata/blob/fifthiteration), and I have made a small [Runner program](https://github.com/Danielovich/LogParsingKata/blob/fifthiteration/runner/Program.cs) where you can follow the execution chain from.
