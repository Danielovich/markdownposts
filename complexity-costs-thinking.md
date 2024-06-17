[//]: # "title: Complexity Costs Thinking"
[//]: # "slug: complexity-costs-thinking"
[//]: # "pubDate: 14/6/2024 12:01"
[//]: # "lastModified: 17/6/2024 10:20"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

I have been pondering on a few context-aware abilities when applying complexity, solving small and larger tasks and what my thought processing is in those situations. I find it an interesting process, balancing complexity while not leaning towards to much cleverness, also catering to a lower common denominator. I'm not sure if my phrasing is entirely accurate, but I'll leave it for now.

My experience tells me that in any field or any task at hand, the expected up-front complexity simultaneously increases the price and costs - monetary, mentally or environmental - either initially or over time. This might seem like a no-brainer, but the question I also want to ask myself is what positive side effects reduced complexity offers.

These are just examples from my own mind, based on where some of my energy has been channeled the last couple of weeks. Take some from your own life.

- It is easier to make a yeast dough than a sourdough
- It is easier to maintain a fiberglass boat than a wooden one
- It is easier to make pasta alla norma than bouillabaisse
- It is easier to get around by bike than by car
- It is easier to set up a wind farm than a nuclear power plant

Since this is about the thought process around complexity the reason for reiterating "easier" should be self-documenting for the reader.

Of course there are a lot of underlying things here that I'm fully aware exist - for instance, fiberglass doesn't grow on trees as wood does, it is a mixture of different other possible complex compontents and therefore there is some kind of cost involved. Or that nuclear power is a more stable base-load technology because it works even when the sun isn't shining or the wind isn't blowing. Fully aware that nuclear offers more concerns in different aspects. But I think you understand what I'm trying to say.

"It is easier" naturally also applies to software. Just as its stubborn sibling "it's better" does.

I find it important, at least for myself, not to view increased complexity as a stamp of higher quality. Quite the opposite, in fact. Just as I don't find myself creating a better outcome by using more complex technology, exclusive ingredients for cooking, or titanium as the boats lanterns.

Again, I am fully aware I use the word "better" here, which in reality has nothing to do with complexity or any of the words antonyms. The point being sometimes you cannot avoid complexity but I do all that I can, with the boundaries given, to minimize it. So unless I am 100% aware of why I need to apply complexity I try hard not to be pressured by its side-effects. I also usually find myself paying a higher price for this applied complexity although it might seem easier and better at first glance.

Everything comes with trade-offs. Is the relative price right ?

Therefore, getting the balance right is also important. In other words, I need to be meticulous about the question of why complexity should not be introduced at a given time, in a given state or context. The thing is that I  find it easier to introduce complexity, more so than removing it. I am very conscious of this and try to be aware of whether it stems from experience or another internal or external pressure somehow.

Why do you add complexity ? To introduce simplicity ? Do you know the cost of its abstractions ?

- I make yeast dough because my guests can't taste the difference between it and sourdough anyway (sourdough is more complex)
- I sail in a fiberglass boat because I don't want to maintain a wooden one (wooden boats are known for higher cost around maintainablity)
- I make pasta alla norma when the kids are home and I'm tired after a long day (takes less attention than a bouillabaisse)
- I make bouillabaisse when I have plenty of time and I know it will be appreciated by others (apply complexity when i believe it's appreciated and needed)
- I bike in the city, because it's infrastructure makes it better than driving.

Try using your own statements and try to reflect upon what their "costs" are up-front, and what their relative price is over time. Also what they give back to you. Also in an up-front and over time manner. And don't measure it  not only on a monetary level but also on the cognitive, organizational, environmental and social level, etc.

Is it really that different from:

- It is easier to write a switch statement than a factory
- It is easier to write text than HTML
- It is easier to read and write in the same method than using commands and queries
- It is easier to deploy via "git push" than with FTP
- It is easier to write code when you know the expectations
- It is easier to use a cloud provider than to host yourself

What is the cohesion ?

I find these things also to have a high cohesion with the underlying understanding of an expected outcome. Now I might be approaching something that has to do with knowing, a certainty to why something has to be, or can be, done a certain way. Personally I don't like to guess. I like to be prepared. That goes for when I am cooking, sailing and especially when writing software someone else is paying for.

I can apply my preparation by knowing why I am doing something.

- I make yeast dough because it's faster than doing a sourdought, and because my guests can't taste the difference
- We write HTML because we know plain text is not presentable enough for our customers needs
- I don't refactor this switch statement to a factory because it only has four top level branches
- I don't create a product before a client has paid for it
- We created a monolith because the organization is not mature enough for loosening its communication patterns

These things are all about knowing. Not guessing. Knowing why I am doing something or knowing why not to do something. They are about collecting as close to facts as possible before I approach them as tasks. How you collect your facts is up to you but there are plenty of cords to pull in any given situation. Collecting evidence and data points, documentating them, is key.

It will slow me down, but I would rather spent my time doing things I know why I am doing than guessing why I am doing. 

Another subject which is an offspring of my internal discussion is pace. The software industry seems obsessed with pace - lead time, cycle time, deployments, builds and so forth. That's all dandy. But I find it more interesting being obsessed with the measurement on correctness of an outcome. 

Am I doing the right things ? On what basis ? Why do I know this ? Most cases of why in a professional setting are likely of monetary and competing character, and that is perhaps how the world works. But I truly believe quality does not come from a faster pace, but from knowing why.

Pace can help us in many different ways, also as supporting pillar in validating the why. I need to be fast at validating the why, but I cannot be  faster than the constraints. Being fast is not a quality in software. 

I might be fast, somewhat and sometimes clever, and I perhaps have applied the right amount of complexity. But at the same time I might have done all of this without knowing for what reasons. I make lots of poor reasoning but they are almost always based on not understanding why. And understanding the why is often time consuming and will take you places you didn't think of initially.

At least in my book, reversing something is harder than its introduction. So for me it answers the question about knowing why you are applying possible complexities to places where it is not needed. And anything that is not needed, complex or not, is still not needed.

- why did I make a sourdough pizza when I knew my guests couldn't taste the difference ?
- why did I use a sidecar pattern when the monolith already holds logging and configuration ?
- why did we do this feature when no data is provided for its need ?
- why did I introduce idempotency patterns when we have no eventual consistency challenges in this domain ?
- why did I take the car when it would have better for me to bike ?

You can use these questions as a guiding stick for your own questions of reflection. My point only being, with this piece of text, that if you don't know why you are doing something, it's hard to justify the time spent, the money used, the relative cost, the human efforts etc. Don't just do things, understand why they are done. 