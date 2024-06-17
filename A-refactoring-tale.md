[//]: # "title: A refactoring tale"
[//]: # "slug: A-refactoring-tale"
[//]: # "pubDate: 17/6/2024 10:22"
[//]: # "lastModified: 17/6/2024 10:22"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

My approach towards refactoring may have changed over time, possibly because my experiences influence my thought patterns. I also like to tell myself that it’s because I see things in a different light. There’s also a difference in my patterns when I code for money versus when I don’t. There’s a time perspective and an implicit speed factor involved.

When I wrote the [software for this blog](https://github.com/Danielovich/RubinStatic), there was some source code that I found irritating to look at. However, I knew it didn't matter much to change it since I would be the only user of the software. One of the perks of writing and using your own software extensively is that you can change it when you feel the need arises.

I had a switch statement that I felt was in the way and saw as a potential candidate for a cleaner implementation. Objectively, there's nothing wrong with it—it’s correct and should be easy to understand even for first-time readers of the code.

It’s a [central part of a module](https://github.com/Danielovich/RubinStatic/blob/da8d759280bbfbca7e52de7bda9732b4b7ac590b/src/Rubin.Markdown/Parsers/MarkdownPostParser.cs#L26) that deals with parsing comments from one format to a stronger type, into memory, for later use.

I must emphasize that I’m not going into implementation details, and there are plenty of possible fingers to point—point away—but it doesn’t matter much how the code looks one way or the other. It’s the path to a better place that’s interesting.

One of the most interesting aspects of writing software you use over time is revisiting certain elements that you later find nuanced, with my mind searching for a recognition of why it ended up that way. Not because the result is incorrect or not accurate, but simply because I can’t remember it.

In that way, refactoring is also a cognitive challenge. It’s an exercise in wanting to improve something despite a lack of resonance somewhere in the brain.

I write software without perfectionism or delving into the complex or fancy. I know that if I write it correctly the first time, the opportunities to understand and change the code will arise more easily over time. But this is also somewhat dangerous because, even though I might be wiser today than I was yesterday when I look at things with fresh or new eyes, it doesn’t mean the same state will apply over time. I risk changing something that was simple enough into something that is no longer simple enough.

Someone once said that simple code is fast code. This had little to do with clock cycles and more to do with the mental burden of understanding and reading something complex.

So, I have this switch statement I want to get rid of. Today, when I try to write some code differently, I first isolate the situation. I do this by branching out or copying the code into another project environment. From there, I write code until I find my approach too complicated for what the baseline offered. This is on purpose.

As a solo programmer for some software, this is an arrogant stance, and I’m fully aware of it. It’s rare to be alone in deciding something, so I must be careful not to become too single-minded in my approach to how I think things should be. But they need to be easy for me to understand. It’s a quality point for me that I can write some code, come back several months later, and immediately understand why I did something in a certain way. But even that tends to change.

So, I extract the code and place it in a space by itself, and from there, I write some code. My first iteration is a rough draft, a sense of warming up to the possibilities for the refactoring, a mind-activating excersise. What do you want here? Try this. Tighten the screw here. Where does it end? 

I write comments for myself to make the history visible for me at a later point. A reference point. It works for me. I like reading comments on thought process during refactoring.

Initially, I thought I would remove the switch statement and make it possible to iterate in a slightly smarter way, so I [wrote something over a few hours and then studied the immediate history of the code](https://github.com/Danielovich/switchonoff/pull/1/files). But the end result was not good.

I continued my journey. [The next experiment I did was with a Dictionary holding an Action](https://github.com/Danielovich/switchonoff/pull/2/files). One of the side effects of this code is that it doesn’t change other parts of the code—tests, for example—except for the original switch statement I wanted to remove. I also introduced the Dictionary. Again, the code is commented so I can come back and look at the experiment later.

I take my time, I think. I wonder sometimes, would someone pay for this in a professional setting? The time taken.

But I’m still not completely satisfied. Even though I actually think it’s a fine approach, I’m not sure now. So, I try again.

Now, I move over to [a pattern that I consider both well-known and suitable for the purpose](https://github.com/Danielovich/switchonoff/pull/3/files), and which I find a possible candidate for improving the code’s condition through refactoring. But there are always trade-offs. I get a bit more code. It’s a bit more complex to read and understand — I might be able to improve that a little. However, I get a better alignment between some structures and new opportunities in the future that might make some things a bit easier. The future is such a fuckery to deal with while programming, so I am by default very hesitant.

I now have three experiments. None of them are really good. None of them are necessary at all to better the correctness of the code. So why would I refator this code ? That has become the real excersise of this thought-process.

I am left with a few experiments that I can study a bit closer and use them to nurture my own arrogance about what I think looks best. If it's more suistainable than the original implementation is of course always a debate. Likeminded and all that. No measurements available.

This is also an excersise and good opportunity to get better at approaching something that seemed like "opionated" and "offensive" code but perhaps wasn’t as bad as it initially appeared. Trying to better something without needing to change it is one the greatest possiblities of programming, becuase unlike in the psysical world, you can throw out all you want without leaving a stain anywhere.

One thing is certain for me. It takes time for me to create what gives, for me, the best results. And I haven’t even touched on what it means to achieve good results, but it’s definitely a rewarding exercise to refactor slowly and in multiple iterations. 

And I am alone in this, not part of team where I know I must be a lot more than a singular-minded, arrogant, opinionated programmer with me own small codebase no one gives any notice. But it still feels great!