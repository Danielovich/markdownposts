[//]: # "title: Complexity Costs Thinking"
[//]: # "slug: complexity-costs-thinking"
[//]: # "pubDate: 14/6/2024 12:01"
[//]: # "lastModified: 15/6/2024 12:58"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

I have been pondering on a few context-aware abilities when applying complexity, solving small and larger tasks and what my thought processing is in those situations. I find it an interesting process, balancing complexity while not leaning towards to much cleverness, also catering to a lower common denominator. I'm not sure if my phrasing is entirely accurate, but I'll leave it for now.

My experience tells me that in any field or any task at hand, the expected up-front complexity simultaneously increases the price and costs - monetary, mentally or environmental - either initially or over time. This might seem like a no-brainer, but the question I also want to ask myself is what positive side effects reduced complexity offers.

Some examples which can be taken from any state of your own life.

- It is easier to make a yeast dough than a sourdough
- It is easier to maintain a fiberglass boat than a wooden one
- It is easier to make pasta alla norma than bouillabaisse
- It is easier to get around by bike than by car
- It is easier to set up a wind farm than a nuclear power plant

There are a lot of underlying things here that I'm fully aware exist - for instance, fiberglass doesn't grow on trees as wood does, it is a mixture of different other possible complex compontents and therefore there is an underlying cost involved. Or that nuclear power is a more stable base-load technology because it works even when the sun isn't shining or the wind isn't blowing. Fully aware that nuclear offers more concerns in different aspects. But I think you understand what I'm trying to say.

"It is easier" naturally also applies to software.

I think it's important, at least for myself, not to view increased complexity as a stamp of higher quality. Quite the opposite, in fact. I don't find myself creating a better outcome by using more complex technology, exclusive ingredients, or titanium. Again, I am fully aware I use the word "better" here, which in reality has nothing to do with complexity or any of the words antonyms. The point being sometimes you cannot avoid complexity but I do all that I can, with the boundaries given, to minimize it. So  unless I am 100% aware of why I need to apply complexity I try hard not to fall into its side-effects. I also usually find myself paying a higher price for this applied complexity although it might seem simple and better at first glance.

Therefore, getting the balance right is also important. In other words, you need to be meticulous about the question of why complexity should not be introduced at a given time, in a given state or context. The thing is that I actually find it easier to apply complexity than to remove it, at the same time I am trying to be aware of whether it stems from experience or another internal or external pressure somehow.

- I make yeast dough because my guests can't taste the difference between it and sourdough anyway (sourdough is more complex)
- I sail in a fiberglass boat because I don't want to maintain a wooden one (wooden boats are known for higher cost at maintainablity)
- I make pasta alla norma when the kids are home and I'm tired after a long day (takes less attention than a bouillabaisse)
- I make bouillabaisse when I have plenty of time and I know it will be appreciated by others (i will apply complexity when i believe it's appreciated and needed)
- I bike in the city but drive by car when traveling far (biking in a city is less stressful)

Try using your own statements and try to reflect upon what they "cost" up-front, what their relative price is over time, and also what they give back, also in an up-front and over time manner. And not only on a monetary level but also on the cognitive and environmental level.

Is it really that different from:

- It is easier to write a switch statement than a factory
- It is easier to write text than HTML
- It is easier to read and write in the same method than using commands and queries
- It is easier to deploy via "git push" than with FTP
- It is easier to write code when you know the expectations
- It is easier to use a cloud provider than to host yourself

I find these things also to have a high cohesion with the underlying understanding of an expected outcome. Mow I am approaching something that has to do with knowing, a certainty to why something has to be done this way. And I don't like to guess. Not when I am cooking, sailing and especially not when writing software someone else is paying for.

- I make yeast dough because it's faster than doing a sourdought, and because my guests can't taste the difference
- We write HTML because we know plain text is not presentable enough for our customers needs
- I don't refactor this switch statement to a factory because it only has four branches
- I don't write code before a client has paid for the product
- We created a monolith because the organization is not mature enough for loosening its communication patterns

These things are all about knowing. Not guessing. Knowing why I am doing something or knowing why not to do something. They are about collecting as close to facts as possible before I approach them as tasks. How you collect your facts is up to you but there are plenty of cords to pull in any given situation. Collecting evidense is key.

Another subject which is an offspring of my internal discussion is pace. The software industry seems obsessed with pace - lead time, cycle time, deployments, builds and so forth. That's all dandy. But I find it more interesting being obsessed with the measurement on correctness of an outcome. Am I doing the right things ? On what basis ? Why do I know this ? Most cases of why are perhaps of monetary character, and that is perhaps how the world works but I truly believe quality does not come from a faster pace, but from knowing why.

I might be fast, somewhat and sometimes clever, and I perhaps have applied the right amount of complexity. But at the same time I might have done all of this without knowing for what reasons. I make tons of poor reasoning but they are almost always based on not understanding why. And at least in my book, reversing something is harder than its introduction. So for me it answers the question about knowing why you are applying possible complexities to places where it is not needed. And anything that is not needed, complex or not, is still not needed.

- why did I make a sourdough pizza when I didn't know they couldn't taste the difference ?
- why did I use a sidecar pattern when the monolith already holds logging and configuration ?
- why did we do this feature when no data is provided for its need ?
- why did I introduce a cloud storage when all I needed was a large json file ?
- why did I take the car when it would have better for me to bike ?

You can use these questions as a guiding stick for your own questions of reflection. My point only being that if you don't know why you are doing something, it's hard to justify the time spent. And often in professional programming someone is paying for not knowing why. 