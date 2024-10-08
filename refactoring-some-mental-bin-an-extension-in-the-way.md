[//]: # "title: Refactoring the Mental Bin: An Extension in the way"
[//]: # "slug: refactoring-the-mental-bin-an-extension-in-the-way"
[//]: # "pubDate: 7/10/2024 10:22"
[//]: # "lastModified: 8/10/2024 12:22"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

*This is an article that is a little bit obnoxius really. I draw up some opionions here that I don't initially, when writing code, always comply with. But I would like to comply with at all times. But I guess that's another story.*

I was listening to Jazzmatazz Vol. 1 and reading some really simple code.

I remembered a talk by a former professional friend who had made a statement about arrays and performance. Another professional friend had once said, "stupid code is fast code." I was left pondering these two statements while reading the code.

Code readability is subjective. How someone else perceives code is different from how I do, and therefore, it's of course very important that, when working on a team, such things are handled by using some kind of tooling.

You should, of course, not be "allowed" to use one style of code while your colleague uses a different one; that will most likely lead to confusion and frustration. Program correctness, on the other hand, is something you can objectively measure — ["if your specifications are contradictory, life is very easy, for then you know that no program will satisfy them, so, make "no program"; if your specifications are ambiguous, the greater the ambiguity, the easier the specifications are to satisfy (if the specifications are absolutely ambiguous, every program will satisfy them!)."](https://www.cs.utexas.edu/~EWD/transcriptions/EWD04xx/EWD447.html).

Having consistent, readable code alone should lower the cognitive load while reading that code. It's about context switching, they say. We all know how it feels when we enter a module with neatly organized methods, naming conventions, sizing, and documentation—and then switch to a different module that doesn’t fit into the mental model you were just in. You then need to switch gears and figure out what's going on.

Think about this. Imagine having three programmers. One writes abstract classes more often than the second programmer, who writes more interfaces. And the last programmer writes and uses concrete types. Will this be, or is it even, a readability issue?

How programmers use a language will most likely evolve over time. It will most likely evolve as they learn more things. So a codebase will likely also change over time due to that evolution. How should that be controlled?

Reading code is more difficult than writing it. That’s the same reason programmers should be very much aware of how they present their code to others in written form. If you're working on a team, at least have tooling set up around structural semantics. Personally, I like code that reads into the lowest common denominator; call it naive, stupid, or the opposite of clever. I really don't care about how clever you are, I also need to understand it.

Reading code should be like drinking a good glass of wine while eating a good pasta dish and listening to some jazz tunes — something a little less hip-hop-ish than Guru’s *Jazzmatazz*. It should be comfortable and make you feel safe and welcomed.

So, I was reading some code, and this might seem like a bland example, but it should be easy for someone to read and understand.

#### Return this result:

- You have a list of URIs.
- You want to return only the URIs that have certain path/file extensions.

Go ahead and write a program that does that.

Such a type or method could easily be a utility type/method. Some might even call it a helper. An extension is another name for a higher communicated candidate. There is nothing wrong with any of them, but personally I sometimes forget to think whether such a type is too easy to implement in terms of missing out on cohesion and better encapsulation.

So I can have a difficult time with these sort of types since they tend to grow in the number of files, the number of methods, and the number of things that are just considered an extension, helper or utility.

I often fall into my own traps. This code is part of [my site generator Rubin.Static](https://github.com/Danielovich/RubinStatic), and I have done exactly what I just said I didn’t like to do. I have more extensions in there than I like.

But I can also explain why, since I am quite aware of how my mental juggling works with this. An Extension/Utility/Helper directory, or even (worse) a project of such kind, is such a great mental bin to offload into whenever something does not fit right into the directory you're standing in.

And it underlines and shows to me how I go about programming. It is in iterations, and sometimes I'm simply not good enough to capture or being aware of everything. Then, luckily, I can get back and iterate again.

At the time of writing, I have [five files in the Extensions directory](https://github.com/Danielovich/RubinStatic/tree/main/src/Rubin.Markdown/Extensions). It’s the directory in that small project that holds the most files. It sort of left me with a "there is something off" feeling, but it also tells the story of not believing in my own standards for everything I do or don’t do. Heck, I could have left it alone and just pumped more extensions in there over time. But no.

I can’t remember why I wrote the initial method like I did, but as I just stated, "Extensions/Helper/Utilities" is such a great bin to put things into, right?

I have experienced this many times in my career, in many codebases, and I have come to believe that it can stem from not fully collecting or gathering the code where it belongs. The cohesion and encapsulation of these types should be thought of, I think.  

Take these methods as they are, which is part of the namespace *Rubin.Markdown.Extensions*.

```
public static class UriExtensions
{
    public static IEnumerable<Uri> ByValidFileExtensions(this IEnumerable<Uri> uris, string[] validExtensions)
    {
        foreach (var uri in uris)
        {
            for (var i = 0; i < validExtensions.Length; i++)
            {
                if (validExtensions[i] == uri.GetFileExtension())
                {
                    yield return uri;
                    break;
                }
            }
        }
    }

    public static string GetFileExtension(this Uri uri)
    {
        if (uri == null)
        {
            return string.Empty;
        }
        
        string path = uri.AbsoluteUri;
        string fileName = Path.GetFileName(path);
        
        if (fileName.Contains('.'))
        {
            return Path.GetExtension(fileName);
        }
        
        return string.Empty;
    }
}
```

It works, and there’s nothing particularly wrong with it. We can always argue about the nested loops, but for such a short method, it’s not really an issue. The `GetFileExtension` method can also be narrowed down somewhat, sure.

```
[Fact]
public void When_Uris_Should_Be_Markdown_Return_Only_Markdown_Uris()
{
    var uris = new List<Uri>
    {
        new Uri("https://some.dk/domain/file.md"),
        new Uri("https://some.dk/domain/file.markdown"),
        new Uri("https://some.dk/domain/file1.markdown"),
        new Uri("https://some.dk/domain/file"),
        new Uri("https://some.dk/domain/file.jpg")
    };
    
    var validUris = uris.ByValidFileExtensions(ValidMarkdownFileExtensions.ValidFileExtensions);
    
    Assert.True(validUris.Count() == 3);
}
```

Just as a testament, the test asserts `true`.

Let me restate the opening, rather loose, program specification again:

- You have a list of URIs.
- You want to return only the URIs that have certain path/file extensions.

In the original implementation, I used an extension method, which is arguably a fine way to extend functionality to a type. I am not sure I fully agree. The particular extension methods here add functionality to .NET’s `Uri` type along with `IEnumerable<Uri>`, and I guess that’s a working approach. But that wasn’t my concern.

My main concern is that when I program these extensions (utility or helpers) without having given any explicit thought about whether those are the right fit or if the actual belonging is correct. This relates to what I wrote earlier about using extensions as a mental bin, an offload of functionality that might actually be more closely related to something else. So I need to ask myself why I made extensions rather than something else that might be more closely connected to the code that actually uses those extensions.

Now, if I were to refactor this and decrease the number of extensions in the directory I just mentioned, how could I do that?

I will start by looking at where this extension code belongs. As an experiment, I will conclude for now that the particular extensions do not belong as an extension, helper, or utility, but something else.

I'm backtracking the references from the extension file in Visual Studio, and I see that I have a reference inside the file [DownloadMarkdownFileService](https://github.com/Danielovich/RubinStatic/blob/751329c6c46f6988437c152317e80a742b171b6e/src/Rubin.Markdown/MarkdownDownload/DownloadMarkdownFileService.cs#L25).

```
foreach (var uri in uris.UrisWithValidFileExtensions(ValidMarkdownFileExtensions.ValidFileExtensions))
```

`DownloadMarkdownFileService` contains business rules; its purpose is to only download markdown files and convert their contents into a known type called `MarkdownFile`. So `DownloadMarkdownFileService` is really a fine candidate for the extension rather than my original approach.

The first thing to note is that `DownloadMarkdownFileService` is at the lowest level of abstraction. It's deepest in any call chain I have written myself; there are no calls to other types I have programmed myself. So I can also say that it’s a lower type, a fundamental type. I could also argue that having extensions so low down the chain can be bad since any user of the code could simply alter the extension, e.g., changing the valid file extensions, and then everything else would fail. Not good.

A second thing to consider is that someone else could later alter the extension method, perhaps causing a breaking change at runtime, which could also affect the `DownloadMarkdownFileService`.

I understand that one could mitigate these issues with tests, and one should. But it doesn’t solve the issue around cohesion, I think. The code not only belongs in a closer relationship with the `DownloadMarkdownFileService`, but it also makes that type "stronger" and more solid.

That has resulted in adding some code to the `DownloadMarkdownFileService` and its test pendant, but also deleting three files: two files in the extensions folder and one file in the test project.

The previous `foreach` loop, which looked like this:

```
foreach (var uri in uris.UrisWithValidFileExtensions(ValidMarkdownFileExtensions.ValidFileExtensions))
```

I changed to look like this:

```
foreach (var markdownFile in ValidMarkdownUris(uris))
```

Instead of working on the basis of `Uri`, I made a few changes around the `DownloadMarkdownFileService` type, such as adding a new `Path` property to the [MarkdownFile](https://github.com/Danielovich/RubinStatic/blob/main/src/Rubin.Markdown/Models/MardownFile.cs) type.

Instead of working more than absolutely needed on a `Uri`, I now work on the `MarkdownFile` type. Perhaps this is even more positive encapsulation.

This is what the `DownloadMarkdownFileService` type looked like before I refactored it toward better encapsulation and cohesion. I have left out unimportant code for now.

```
public class DownloadMarkdownFileService : IDownloadMarkdownFile
{
    ...
    ...

    public async Task<List<MardownFile>> DownloadAsync(IEnumerable<Uri> uris)
    {
        var markdownDownloads = new List<MardownFile>();

        foreach (var uri in uris.ByValidFileExtensions(ValidMarkdownFileExtensions.ValidFileExtensions))
        {
            ...
        }

        return markdownDownloads;
    }

    private async Task<MardownFile> DownloadAsync(Uri uri)
    {
        var result = await httpClient.GetAsync(uri);

        ...

        MardownFile

 markdownFile = new();
        markdownFile.Contents = await result.Content.ReadAsStringAsync();

        return markdownFile;
    }
}
```

Now it looks like this:

```
public class DownloadMarkdownFileService : IDownloadMarkdownFile
{
    private readonly string[] ValidFileExtensions = { ".md", ".markdown" };

    ...
    ...

    public async Task<List<MarkdownFile>> DownloadAsync(IEnumerable<Uri> uris)
    {
        var markdownDownloads = new List<MarkdownFile>();

        foreach (var markdownFile in ValidMarkdownUris(uris))
        {
            ...
        }

        return markdownDownloads;
    }

    private async Task<MarkdownFile> DownloadAsync(MarkdownFile markdownFile)
    {
        var result = await httpClient.GetAsync(markdownFile.Path);
        
        ...

        markdownFile.Contents = await result.Content.ReadAsStringAsync();

        return markdownFile;
    }

    private IEnumerable<MarkdownFile> ValidMarkdownUris(IEnumerable<Uri> uris)
    {
        foreach (var uri in uris)
        {
            if (ValidFileExtensions.Contains(UriPathExtension(uri)))
            {
                yield return new MarkdownFile(uri);
            }
        }
    }

    private string UriPathExtension(Uri uri)
    {
        if (uri == null) return string.Empty;

        var fileName = Path.GetFileName(uri.AbsoluteUri);

        if (fileName.Contains('.'))
        {
            return Path.GetExtension(fileName);
        }

        return string.Empty;
    }
}
```

I am happy with the result. I didn’t sacrifice much but I like the approach better than the original. The point is not the code per se, but that I should always apply thought to what is an extensible type (or helper and utilities) and what is supposed to be closer to the actual type I am working on (in this cas it was the `Uri`). 

[You can find the whole commit and changes here](https://github.com/Danielovich/RubinStatic/commit/fa5224e141f0adfa5ed9a04d0e8ecd8841b51044).