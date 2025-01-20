[//]: # "title: Files and belongings, why all the indirection ?"
[//]: # "slug: files-and-belongings-why-all-the-indirection"
[//]: # "pubDate: 20/1/2025 16:57"
[//]: # "lastModified: 20/1/2025 16:57"
[//]: # "excerpt: "
[//]: # "categories: programming"
[//]: # "isPublished: true"

This might be addressing dogma, what is occasionally dressed up as good practice. I am not blaming anyone. It seems most part of the world is reading the same information anyway, so I cannot resist. I have come to play with a thought that challenges how some code is probably written.

I am in no position to dictate how your code is written or to what extent your own self-established beliefs have been formed, so this is merely a play of thought. But do take into consideration the "self-established" part of that argument - the why. Why are you writing code that way? Most often, I believe, in a professional context, it's to get things out the door. It would be naive, however, not to accept that we are all, of course, being influenced by whatever is around us, and by what influences those things, and so forth.

### Validating your needs, discarding your guesses

I have talked at length about how I believe validation should occur before writing any code. Others might refer to this as, "Write code that does exactly what you need, make it look good later, and in the end make it fast." The validation of writing code is like the validation of a business idea - if you do not have a single customer, why build the thing? The balance between "does exactly" and "right and fast" is of some importance. Perhaps, if you're the only customer, you don't need to worry too much about the future. Unless, of course, what you're building is meant to portray contextual excellence and self-esteem somehow - then go all in and show the world how great you are. As a programmer, I have been down that road again and again to challenge my own wits (and to sleep at night), but I am fully aware that my code is not of higher importance than how others contribute.

Anyways, the thing about validation is that it could just as easily be applied to writing code. Of course it can. Because why write code that is not needed? Is it because you are predicting the future? Is it because you are arrogant? Is it because you don't want to take the time to contribute to existing code that does nearly the same thing? I am well aware that this argument could almost be considered a religious one, and since context and setting are of high importance, I can only guess why. This has been discussed since the dawn of source code - why are you writing code that solves the future? Sure, you're right, now I am just shoveling more into that bucket, and for that, please excuse me.

And then again, sometimes you do not need to apply validation before you write code; instead, the validation is about something else. Imagine if someone is economically waiting for the outcome of where you are applying your energy - then the code must be written, and the validation might be more about whether that code is sustainable enough for the future. Wait, why? You don't know the future, so apply validation. Like I said, I am in no position to tell you anything that you have already applied your own thorough thoughts to.

### Why all the files? Where is the actual belonging ?

I am going to be a bit provocative and make a guess: most programmers, for a lot of their time, are not working on the higher complexities of things but instead find themselves in a position where abstractions make things easier. You are probably not writing your own security transport layer, your own relational mapper, message bus, and so on. You are most likely using these tools to maintain a productive pace. Your validation will speak to your needs - it's part of preparing work. If you cannot prepare, I believe you diminish your chances of success.

I am stating these things for myself, to underline, for my remembrance, that writing code you do not need to write is not a very good idea. This is also why I have emphasized the validation part of writing code, building software, and perhaps even business ventures. Validation can be difficult, but if you cannot apply the principle of "write code that does exactly the thing you need," chances are that you are already in too deep. Ask yourself: point to the validation you did. Point to the preparation of your work.

Before I wrote this, I initially set out to give myself an example of code that could hide the most important parts, because I have come to realize that when I look at code I have written and that is running in production, it could have been written differently. As it turns out, that code is rarely touched after that point. Rarely is not the same as never, but I have come to realize that grouping things even tighter together could be an easier way to work while figuring out if "writing this code is exactly what I need."

What this has manifested into is sometimes a codebase with fewer files. It is definitely a codebase where seams are tighter but perhaps structured differently for reuse. It has the same number of types and objects but, to me, offers an easier navigational approach to understanding where things belong. It doesn't violate OOP either, which I often find myself applying.

Some code is not touched much after it has been written - so why apply too much indirection to that code? And even if it gets touched often, why the indirection?

It is very important to me, for the reader to understand, that I am not advocating for this as an approach you should adopt - nor even as an approach I would consistently follow. But I do hope this can make you think just a little more about how you approach the code you're writing and, in the end, the software you are responsible for as its caretaker. Do not take what I am writing at face value - I am validating my own chain of thoughts here.

### The codebase of an API integration

I am going to lay out two different styles, one of which I find more commonly used than the other. You can determine for yourself what you think, but don't blame the messenger - add your own validation.

Codebase 1:

```
AirBnbApi
    - Constants
      - FormattingConstants.cs
      - UrlConstants.cs
    - Extensions
      - StringExtensions.cs
      - DateTimeExtensions.cs
    - Helpers
      - DateTimeJsonConverter.cs
      - RequestUrlHelper.cs
    - Interfaces
      - IAirbnbClientService.cs
    - Models
      - Price.cs
  - AirbnbClientService.cs
```

This is a small codebase. From the naming of the files, you can infer somewhat that this is about Airbnb and prices. However, there are a whole lot of other files that the programmer might look at and think, "These are global types used to ease the actual API communication and modeling." How the API works internally is not of importance in this context.

Why have all these files if they are really only serving the purpose of the inner workings of `AirbnbClientService.cs`? The reason I choose to mention `AirbnbClientService` is implicit - I know from looking at codebases like this that this is most likely the type that orchestrates the other types found in the codebase.

Personally, I dislike names such as `Extensions`, `Helpers`, `Utilities`, `Common`, and other similar global identifiers for things that, in my mind, should be kept closer to where those extensions, helpers, utilities, or common items are actually used. When I say "belong," I mean where they are first put to use. That can change over time, of course, but it doesn't change when we are in the state of "write code for exactly what you need."

So, in this codebase, I could argue that if the code inside `FormattingConstants.cs` has formatting methods closely related to `AirbnbClientService.cs`, those methods could just as easily live inside that type.

Let's say that the `AirbnbClientService.cs` file is structured as follows: 

```
public class AirbnbClientService : IAirbnbClientService
```

Now, we want to expand that type so the code inside `FormattingConstants.cs` will be part of it as well. An easy approach is to add a new top-level class name.

Since this code is about an API for prices at Airbnb, we can call the top-level class `PriceApi` and then adjust the code accordingly. I would also add the content of the interface into that file simply because that is where I believe it belongs the most. You could argue that having the interface in its own file makes the reading experience better, but I believe the tradeoff of having it as part of where it is used is worth it. 

The effects of grouping code together are not only technical. They also make it easier for me to understand its possible heritage.

```
public interface IPricesHttpClient
{
    Task<Price> GetPricesForPeriodRangeAsync(DateTime start, DateTime end);
}

public sealed class PriceApi : IPricesHttpClient
{
    ....

    public async Task<Price> GetPricesForPeriodRangeAsync(DateTime start, DateTime end)...

    public sealed class FormattingConstants
    {
        public const string BaseUrl = "https://url.to/airbnb/prices/api";

        public const string PeriodRangeQueryString = "&start={0}?end={1}";
    }

    ...
}
```

So I merged three files into one. And I made a new top level type. I will continue doing this until all code that is specifically tailored to the PriceApi type is in that specific type. In the end that means that all code beloning to PriceApi is in that type.

```
public interface IPricesHttpClient
{
    Task<Price> GetPricesForPeriodRangeAsync(DateTime start, DateTime end);
}

public sealed class PriceApi : IPricesHttpClient
{
    ....

    public async Task<Price> GetPricesForPeriodRangeAsync(DateTime start, DateTime end)...

    public sealed class FormattingConstants
    {
        public const string BaseUrl = "https://url.to/airbnb/prices/api";

        public const string PeriodRangeQueryString = "&start={0}?end={1}";
    }

    public sealed class Price 
    {
        ....
    }

    public sealed class RequestUrlHelpder 
    {
        ...
    }

    ...
}
```

This will leave me with a PriceApi type that encapsulates all its code within that type. Not only that, it will make it easier to know what potential piece of code belongs.

Imagine writing some code, before the change, that would use the Price model inside the Models folder. You would not know, just by writing the code, where that model belongs.

With the change I just made, this is less ambiguous since it is now explicitly part of the PriceApi type. You would need to access the type by specifying PriceApi.Price rather than just Models.Price. Where does Models.Price belong? Now it belongs to PriceApi, and I would argue that is where it should be.

The most obvious counterargument, of course, is: "What if I want to use the Price model in some place other than PriceApi?" Essentially, this is a question about how to reuse Price. I cannot answer that easily because I am not programming for the future - I am programming for what I know at this moment in time.

That decision would depend on future requirements. You might argue to pull Price out to avoid violating DRY, but that would only make sense if the other piece of code needed Price exactly as it is. If it is not exactly as-is, it's not a violation of DRY, and perhaps a lookalike type would be better suited to the code it is really part of.

With the changes completed, the structure and files of the codebase now look like this. I haven't shown you all the changes since that is not the important part.


```
AirBnb
  - PriceApi.cs
  - DateTimeJsonConverter.cs
```

I believe two files are easier to comprehend than nine - not to mention the folders. I have left the DateTimeJsonConverter.cs file to make the reader think about why. Why is that not part of PriceApi.cs? Just as simply, why were there nine files? Why?

Now, what happens if we introduce another API from airbnb we wish to communicate with ? Let's say I want to communicate with the "Search" API from airbnb.

```
public interface ISearchHttpClient
{
    Task<Price> GetSearchForPeriodRangeAsync(string search, DateTime start, DateTime end);
}

public sealed class SearchApi : ISearchHttpClient
{
    ....

    public async Task<Price> GetSearchForPeriodRangeAsync(string search, DateTime start, DateTime end)...

    public sealed class FormattingConstants
    {
        public const string BaseUrl = "https://url.to/airbnb/search/api";

        public const string SearchQueryString = "&q={0}?start={1}?end={2}";
    }

    public sealed class Search 
    {
        ....
    }

    public sealed class RequestUrlHelpder 
    {
        ...
    }

    ...
}
```

Then we can start to ask ourselves what is actually up for reuse between the two API clients, PriceApi and SearchApi.

Since we already have a PriceApi, we can extract the things that are truly duplicates. It might look something like this: there are no folders with odd names, which, to me personally, add more indirection than explicitness. It even seems that most of the code is on the same level, except for the types that belong to their respective APIs, which are placed in the same files.

```
AirBnb
  - PriceApi.cs
  - SearchApi.cs
  - DateTimeJsonConverter.cs
  - AirbnbHttpClientFacade.cs
  - FormattingConstants.cs
```

### Conclusion

This exploration is not about prescribing a one-size-fits-all approach to organizing codebases but rather about challenging the patterns we often adopt without much reflection. By questioning the need for perhaps excessive indirection and fragmentation, I merely aim add to a conversation about code validation and its implications for maintainability and clarity.

The central theme here is to prioritize writing code that does not serve the speculative future-proofing. This approach might foster a balance between explicitness and simplicity, applying a narrower structure of the code.

But ultimately, every choice in a codebase reflects a tradeoff. Whether it's fewer files or more modularity, tighter encapsulation or reusability. My belief is that the answer lies in the context of the validation and the moment of time I am in. This might encourage you to revisit some of your coding habits, validating your decisions against the needs of the present rather than the uncertainties of the future.