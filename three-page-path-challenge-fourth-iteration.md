[//]: # "title: Three Page Path Challenge - fourth iteration"
[//]: # "slug: three-page-path-challenge-fourth-iteration"
[//]: # "pubDate: 26/8/2024 17:00"
[//]: # "lastModified: 26/8/2024 10:22"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

This is the fifth article in a series about a man with a three-page path challenge, an interview question that can come up for programmers.

[Make sure to read the problem scope in the first article in this series.](/a-man-with-a-three-page-path-challenge.html)

Also, read about the [first iteration](/three-page-path-challenge-first-iteration.html), the [second iteration](/three-page-path-challenge-second-iteration.html), and the [third iteration](/three-page-path-challenge-third-iteration.html).

There is always more, right? The balance between solving something at a satisfactory level and wanting to twist and turn something for answers that you started to ask yourself questions to. Programmers are usually creative beings, often questioning in silence and navigating mazes that can change patterns as they go. One needs to exercise one's mind, not questioning everything to a level of obscurity. Just as often, programmers are filled with honor by their craft, which in turn comes from their mind and hopefully thousands of hours of training. Question it, and you question their intelligence. Question a human's intelligence, and chances are that the human will react. Unless that human being is cool, calm, and knows everything is going to be okay.

We do it again and again, with a hopefulness of getting into that state of complete joy of floating. What a feeling. Crystal clear right there and then.

In this fifth article, I have turned my mind into asking it questions around defensiveness, immutability, and perhaps a little performance.

Visual Studio has a profiler I like. It's easy to use. For example, it lets you look at percentages of where CPU is used in your program, and it gave me some insights. But before I went there, I had some thoughts about how I could "tighten" the types of my program. Defensive code has grown a bit on me, and that has led me to pursue immutability and sealed types.

Both Eric Lippert and Jon Skeet have great comments about sealed types, and I have come to a conclusion for part of this iteration, at least, that sealed types should be the default. It resonates with me that opening something is easier than closing something that was open from the get-go. And in programming, this should really resonate with you quite easily.

So I sealed almost everything off. No problem, no tests failed, nothing couldn't compile. Pure win. The next thing I wanted to try was to turn some of my types into immutables since it makes sense that data cannot be edited during execution. Reading logs is a read-only mechanism, and it shouldn't be possible to add or change data during those measurements. That's one thing, even though I probably cannot guarantee anything. The other thing is that it's more defensive and leaves fewer surprises.

With an ```IEnumerable<T>``` you can edit it during execution. I wanted to seal that option off because of obvious reasons when talking immutability, so I settled with ```IImmutableList<T>``` instead. You can edit that type as well, but it will return a copy/new instance if you do.

Looking at the [code for this iteration](https://github.com/Danielovich/LogParsingKata/tree/fourthiteration), it is obvious to me that I have not followed this all the way through since there are IEnumerables still in the code. I have let myself side-track because of the interest in what possible performance—if any at all—this has left on the solution. The code I have been timing and benchmarking has turned out to be faster in iteration three. So that was a side-track that didn't do me any good in that capacity, and I even looked into the profiler for things that took up most time.

But did it leave the code in a more readable state then? I fell into a different trap. I didn't do one thing at a time. Don't optimize code for the sake of refactoring. And don't refactor solely for optimization unless you are absolutely clear about why. Measure, measure, measure. Even though I fell right into it—the joy of floating, right?—I would be very wary of doing optimizations on code without being absolutely specific, and as it often turns out, side effects of performance optimization can creep into the surrounding types of your software. I think I have done that in this iteration. I wanted to be too clever perhaps, and now I will have to go back and clean up with only one thing in mind.

So the code is left in a state now where it is not as readable, I think, as in iteration three. I will have to address that. I am back at it. [You can follow the path from this unfinished test](https://github.com/Danielovich/LogParsingKata/blob/fourthiteration/logparserkata/PathPatternsLogEntryFileTests.cs) and go about it as you wish.