[//]: # "title: Balancing Readability, Testability, and Structure: Refactoring a small type with John Carmack’s Email in mind"
[//]: # "slug: refactoring-a-small-type-with-carmacks-email-in-mind"
[//]: # "pubDate: 28/10/2024 10:22"
[//]: # "lastModified: 28/10/2024 19:38"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

This is an article about refactoring the characteristics and behaviors of a type, based on some interesting arguments written in an email by programmer John Carmack in 2007.

[The email](http://number-none.com/blow/blog/programming/2014/09/26/carmack-on-inlined-code.html). 

He introduces an interesting idea that makes me reflect on when and why it might be necessary to disregard conventional wisdom surrounding the downsizing and decomposition of methods/functions, often aimed at improving readability, code navigation, and mental cognition. While I definitely believe that smaller pieces of code can enhance readability, there is also a cost associated with it, particularly around maintaining the same navigational context and minimizing context switching.

It's a worthwhile question whether breaking down methods/functions into smaller ones is the right approach from the outset.

John's email from 2007, at least as I interpret it, suggests something contrary to what I have been taught over the years—that is, not to lump everything into one method or function. Larger methods, of course, can blur readability, become brittle in testing, and violate the Single Responsibility Principle (SRP). The exercise of "3 strikes, then add the first abstraction" comes to mind. I no longer apply this upfront because my sense of when to decompose a method is somewhat ingrained in how I approach programming in the first place. So, I could say that I decompose right away, but that wouldn't be entirely accurate because I frequently revisit and refactor, either following the Boy Scout Rule or based on my cognitive state at that particular time. I write a method, decompose it when it becomes too long, and continuously refine it without giving it much thought. I do, however, approach decomposition with more consideration when it involves a public API.

I mention "first-time abstraction" because I don't believe the initial abstraction attempt is always correct, even after reviewing something three times and determining it should be abstracted. I might need the abstraction, but does that mean it will be perfect the first time? In my experience, no.

This has become part of my own practice: applying a hypothesis around a potential abstraction but also keeping it as narrow as possible due to the likelihood of revisiting the same piece of code in the future. This isn't universally applicable—magic numbers and strings, for example, are clear candidates for abstraction early on.

Anyway, as I reflect on this while writing and reviewing some code, I see no reason to split up this function or module/class as a means of improving readability. Readability is a crucial aspect of programming, but I also worry about applying too much abstraction since it can extend beyond readability concerns. And honestly, I'm not sure I entirely believe that.

When I navigate through code—going to references, implementations, scrolling up and down—I often lose context. But maybe if the decomposition isn't scattered across different types and files, it could be effective. This is how I've programmed for years.

It has been a long time since I've used a different approach in any object-oriented programming language, though earlier I wrote a lot of interpreted code. Robert C. Martin's advice, “Gather together the things that change for the same reasons,” still resonates. It's solid guidance.

John Carmack's perspective adds another layer, one that I believe significantly impacts a programmer's cognitive load when navigating and reading source code: “Step into every function to try and walk through the complete code coverage.” Next time you're working in a codebase, try following all the paths your code takes during execution and debugging.

We can debate readability and composition all year. I'm not here to impose any dogma or dictate the best approach—you can decide that for yourself. I'm simply experimenting with styles. Some, as John suggests, might be less prone to introducing defects.

In his email, John outlines three types of compositions for a function, or a module if you prefer to extend the concept.

He concludes with these thoughts:

“Inlining code quickly runs into conflict with modularity and object-oriented programming (OOP) protections, and good judgment must be applied. The whole point of modularity is to hide details, while I advocate for increased awareness of those details. Practical factors like increased multiple checkouts of source files and including more local data in the master precompiled header, forcing more full rebuilds, must also be weighed. Currently, I lean towards using heavyweight objects as the reasonable breakpoint for combining code and reducing the use of medium-sized helper objects while keeping any very lightweight objects purely functional if they must exist at all.”

The key points I take away are:

"Good judgment must be applied"; you need experience and professionalism for this.
"Practical factors..."; juggling multiple files while reading or writing code can be challenging.
"Heavyweight objects...very lightweight objects...purely functional"; this appears to be the crux of his argument, suggesting a preference for larger objects while avoiding decomposition unless a clear breakpoint exists. Pure functions are beneficial but tricky when dealing with I/O and networking.
Finally, John gives this advice:

“If a function is only called from a single place, consider inlining it.”

I don't intend to diminish John's insights—he's a far more experienced programmer than I am—but I find his thoughts and arguments compelling enough to explore further. Since it's a style I haven't used in a while, I'm curious to give it a try.

The first thing that comes to mind when I read, “If a function is only called from a single place, consider inlining it,” is that every function starts by being called from just one place. If not, you might already be ahead of yourself.

I'll begin with an inlined version of an originally non-inlined method and present the full type. One could write a type like this with significant inlining involved.

```
public class DownloadMarkdownFileService : IDownloadMarkdownFile
{
	private readonly string[] ValidFileExtensions = [".md", ".markdown"];
	private readonly List<DownloadMarkdownExecption> downloadExceptions = [];
	private readonly List<MarkdownFile> markdownDownloads = [];
	private readonly DownloadMarkdownFileServiceResult downloadAsyncResult = new();

	private readonly HttpClient httpClient;

	public DownloadMarkdownFileService(HttpClient httpClient)
	{
		this.httpClient = httpClient;
	}

	public async Task<DownloadMarkdownFileServiceResult> DownloadAsync
		(IEnumerable<Uri> uris, CancellationToken cancellationToken = default)
	{
		foreach (var uri in uris)
		{
			if (uri == null) continue;

			var fileName = Path.GetFileName(uri.AbsoluteUri);
			var extension = fileName.Contains('.') ? Path.GetExtension(fileName) : string.Empty;

			if (!ValidFileExtensions.Contains(extension)) continue;

			var markdownFile = new MarkdownFile(uri);

			try
			{
				var result = await httpClient.GetAsync
					(markdownFile.Path, cancellationToken);

				if (!result.IsSuccessStatusCode)
				{
					throw new HttpRequestException
						($"Could not download file at {markdownFile.Path}", 
						null, 
						result.StatusCode);
				}

				markdownFile.Contents =
					await result.Content.ReadAsStringAsync();

				markdownDownloads.Add(markdownFile);
			}
			catch (HttpRequestException hre)
			{
				downloadExceptions.Add
					(new DownloadMarkdownExecption($"{hre.Message}", hre));
			}
			catch (Exception e)
			{
				downloadExceptions.Add
					(new DownloadMarkdownExecption($"{e.Message}", e));
			}
		}

		downloadAsyncResult.DownloadExceptions = downloadExceptions;
		downloadAsyncResult.MarkdownFiles = markdownDownloads;

		return downloadAsyncResult;
	}
}
```

Anyone can program in their own preferred style; I'm not here to critique but rather to experiment with different approaches and explore their potential.

When I look at this code, I feel a bit mixed. It accomplishes its purpose, and its structure and length are reasonable.

However, I find it difficult to fully accept this method because it goes against some principles I consider important, and I suspect it may pose challenges when testing. Although readability isn't particularly problematic here, I feel the method does more than is ideal for a single function to handle.

The inlined elements within this method could also be seen as problematic. For instance, the method performs multiple tasks, limits extension options, and might even conflict with the Dependency Inversion Principle, as it returns a concrete type rather than an interface.

Here's what the method currently does:

- Validates file extensions (ValidFileExtensions)
- Fetches content via HTTP (using HttpClient)
- Processes downloaded files (markdownDownloads)
- Returns the result in DownloadMarkdownFileServiceResult

While I don't find the method's readability poor—it narrates a clear story, has some error-handling decisions, and a logical structure—testing it could be challenging. This is mainly due to the intertwined implementation details. For instance, the URI extension validation mechanism is tightly integrated within the method, making it less flexible and more difficult to modify or test independently. This isn't about rigid adherence to any single approach; rather, it's just an observation. Our goal here is to explore if and when inlining code in this way is effective for certain parts of our codebase.

In a more structured OOP environment, the URI extension might make an excellent candidate for a pure function. And this aligns with what I understand John is suggesting in his email, where he states: "If a function is only called from a single place, consider inlining it." He also notes, "If the work is close to purely functional...try to make it completely functional."

Since this function is called only once, inlining seems appropriate. So, let's look for other potential candidates where this might also apply.

One could write a pure function as shown below and encapsulate it in a lightweight type. While this approach might counter the purpose of inlining, it's worth noting. So far, I've identified one candidate for inlining, but there could be more.

```
public string UriPathExtension(Uri uri) =>
	uri is null ? string.Empty : Path.GetExtension(uri.AbsoluteUri);
```

The next point John makes in his list is: "If a function is called from multiple places, see if it is possible to arrange for the work to be done in a single place, perhaps with flags, and inline that."

This is an interesting suggestion. Suppose I have two types of tasks I need to carry out. I want to download markdown files, but I also need to download image files, like JPGs and PNGs. I might initially have two distinct services, each calling the pure function I outlined in ```UriPathExtension```. John's advice here is to consolidate that work in a single place and then inline the function again.

Since my original method returns a ```DownloadMarkdownFileServiceResult```, I would likely need to refactor it to a more general ```DownloadFileServiceResult```. From there, the adjustments could start to cascade. With this type, I could handle both tasks while keeping potential extractions inlined. (Please note, I haven't fully implemented this code, so it's not build-ready due to the incomplete refactoring.)

In this experiment, I'm focusing on readability, testability, and the shifts in mindset required when programming in an OOP context.

```
public class DownloadMarkdownFileService : IDownloadFiles
{
	private readonly string[] ValidMarkdownFileExtensions = [".md", ".markdown"];
	private readonly string[] ValidPictureFileExtensions = [".jpg", ".png"];

	private readonly List<DownloadMarkdownExecption> downloadExceptions = [];
	private readonly List<DownloadableFile> downloadedFiles = [];
	private readonly DownloadMarkdownFileServiceResult downloadAsyncResult = new();

	private readonly HttpClient httpClient;

	public DownloadMarkdownFileService(HttpClient httpClient)
	{
		this.httpClient = httpClient;
	}

	public async Task<DownloadMarkdownFileServiceResult> DownloadAsync
		(IEnumerable<Uri> uris, CancellationToken cancellationToken = default)
	{
		foreach (var uri in uris)
		{
			if (uri == null) continue;

			var fileName = Path.GetFileName(uri.AbsoluteUri);
			var extension = fileName.Contains('.') ? Path.GetExtension(fileName) : string.Empty;

			if (!ValidMarkdownFileExtensions.Contains(extension)) continue;
			if (!ValidPictureFileExtensions.Contains(extension)) continue;

			try
			{
				var result = await httpClient.GetAsync
					(file.Path, cancellationToken);

				if (!result.IsSuccessStatusCode)
				{
					throw new HttpRequestException
						($"Could not download file at {file.Path}", 
						null, 
						result.StatusCode);
				}

				//Markdown
				if (ValidMarkdownFileExtensions.Contains(extension))
				{
					//markdown 
					file.Contents =
						await result.Content.ReadAsStringAsync();
				}

				//Pictures
				if (ValidPictureFileExtensions.Contains(extension))
				{
					//markdown 
					file.FileStream =
						await result.Content.ReadAsStreamAsync();
				}

				downloadedFiles.Add(file);
			}
			catch (HttpRequestException hre)
			{
				downloadExceptions.Add
					(new DownloadResultExecption($"{hre.Message}", hre));
			}
			catch (Exception e)
			{
				downloadExceptions.Add
					(new DownloadResultdownExecption($"{e.Message}", e));
			}
		}

		downloadAsyncResult.DownloadExceptions = downloadExceptions;
		downloadAsyncResult.DownloadableFiles = downloadedFiles;

		return downloadAsyncResult;
	}
}
```

How does it read? How does it test? I could reiterate what I mentioned with the first method described earlier: the more varied tasks I incorporate around the same domain concept, the more this method seems to expand.

I realize I'm bringing up testing considerations here, and perhaps I shouldn't, but each time I add more inlined code, I have an instinctive hesitation. This latest method won't necessarily become easier to test or to use. One small challenge among many is that now I need to assess the file type in the result as well. But having all the code in one place certainly has its benefits. The trade-offs, however, are evident.

### "good judgment must be applied" - John Carmack

For readability, this setup isn't quite working for me. While I appreciate the reduced cognitive load from having all the code in one place, the trade-offs seem too significant. One might argue that this type is too small to be a meaningful experiment, which is fair — but is any type truly too small? Either way, the bottom line is that I don't find this approach appealing, so I'll go back, keeping in mind that "good judgment must be applied," and refactor the first method posted to explore a few alternative styles I might actually use.

My initial refactoring approach still leans towards the principles outlined in Carmack's Email which is perfectly fine. I'll decompose the type as I proceed.

```
public class DownloadMarkdownFileService : IDownloadMarkdownFile
{
    private readonly HttpClient httpClient;
    private readonly string[] validFileExtensions = { ".md", ".markdown" };

    public DownloadMarkdownFileService(HttpClient httpClient)
    {
        this.httpClient = httpClient;
    }

    public async Task<DownloadMarkdownFileServiceResult> DownloadAsync(IEnumerable<Uri> uris, CancellationToken cancellationToken = default)
    {
        var downloadExceptions = new List<DownloadMarkdownExecption>();
        var markdownFiles = new List<MarkdownFile>();

        foreach (var uri in uris)
        {
            if (uri == null) continue;

            var fileName = Path.GetFileName(uri.AbsoluteUri);
            var extension = fileName.Contains('.') ? Path.GetExtension(fileName) : string.Empty;

            if (!validFileExtensions.Contains(extension)) continue;

            try
            {
                var response = await httpClient.GetAsync(uri, cancellationToken);

                if (!response.IsSuccessStatusCode)
                {
                    throw new HttpRequestException($"Could not download file at {uri}", null, response.StatusCode);
                }

                var contents = await response.Content.ReadAsStringAsync();
                markdownFiles.Add(new MarkdownFile(uri) { Contents = contents });
            }
            catch (HttpRequestException hre)
            {
                downloadExceptions.Add(new DownloadMarkdownExecption(hre.Message, hre));
            }
            catch (Exception e)
            {
                downloadExceptions.Add(new DownloadMarkdownExecption(e.Message, e));
            }
        }

        return new DownloadMarkdownFileServiceResult
        {
            DownloadExceptions = downloadExceptions,
            MarkdownFiles = markdownFiles
        };
    }
}
```
The second approach is leaning a little more into something around SOLID, not much though, since I have only really extracted a new interface in ```IFileValidator```.

```
public interface IFileValidator
{
    bool IsValid(string fileName);
}

public class FileValidator : IFileValidator
{
    private readonly string[] _validFileExtensions = { ".md", ".markdown" };

    public bool IsValid(string fileName)
    {
        var extension = Path.GetExtension(fileName);
        return !string.IsNullOrEmpty(extension) && _validFileExtensions.Contains(extension);
    }
}

public class DownloadMarkdownFileService : IDownloadMarkdownFile
{
    private readonly IFileValidator fileValidator;
    private readonly HttpClient httpClient;

	public DownloadMarkdownFileService(IFileValidator fileValidator, HttpClient httpClient)
	{
		this.httpClient = httpClient;
		this.fileValidator = fileValidator;
	}

    public async Task<DownloadMarkdownFileServiceResult> DownloadAsync(IEnumerable<Uri> uris, CancellationToken cancellationToken = default)
    {
        var downloadExceptions = new List<DownloadMarkdownException>();
        var markdownDownloads = new List<MarkdownFile>();

        foreach (var uri in uris)
        {
            if (uri == null) continue;

            var fileName = Path.GetFileName(uri.AbsoluteUri);
            if (!fileValidator.IsValid(fileName)) continue;

            var markdownFile = new MarkdownFile(uri);

            try
            {
                var result = await httpClient.GetAsync(markdownFile.Path, cancellationToken);

                if (!result.IsSuccessStatusCode)
                {
                    throw new HttpRequestException($"Could not download file at {markdownFile.Path}", null, result.StatusCode);
                }

                markdownFile.Contents = await result.Content.ReadAsStringAsync();
                markdownDownloads.Add(markdownFile);
            }
            catch (HttpRequestException hre)
            {
                downloadExceptions.Add(new DownloadMarkdownException(hre.Message, hre));
            }
            catch (Exception e)
            {
                downloadExceptions.Add(new DownloadMarkdownException(e.Message, e));
            }
        }

        return new DownloadMarkdownFileServiceResult
        {
            DownloadExceptions = downloadExceptions,
            MarkdownFiles = markdownDownloads
        };
    }
}
```

As I go along, please remember that I am still looking for readability and secondly, testability.

The next refactoring I present is towards even more SOLID, and even more decomposing. Does it read well ? Does it test well ?

```
public interface IFileValidator
{
	bool IsValid(string fileName);
}

public class FileValidator : IFileValidator
{
	private readonly string[] _validFileExtensions = { ".md", ".markdown" };

	public bool IsValid(string fileName)
	{
		var extension = Path.GetExtension(fileName);
		return !string.IsNullOrEmpty(extension) && _validFileExtensions.Contains(extension);
	}
}

public class DownloadMarkdownFileService : IDownloadMarkdownFile
{
	private readonly IFileValidator fileValidator;
	private readonly HttpClient httpClient;

	public DownloadMarkdownFileService(IFileValidator fileValidator, HttpClient httpClient)
	{
		this.httpClient = httpClient;
		this.fileValidator = fileValidator;
	}

	public async Task<DownloadMarkdownFileServiceResult> DownloadAsync(
		IEnumerable<Uri> uris, 
		CancellationToken cancellationToken = default)
	{
		var downloadExceptions = new List<DownloadMarkdownException>();
		var markdownFiles = new List<MarkdownFile>();

		foreach (var uri in uris)
		{
			if (uri == null) continue;

			var (success, markdownFile, exception) = await TryDownloadFileAsync(uri, cancellationToken);

			if (success && markdownFile != null)
			{
				markdownFiles.Add(markdownFile);
			}
			
			if (exception != null)
			{
				downloadExceptions.Add(exception);
			}
		}

		return CreateResult(markdownFiles, downloadExceptions);
	}

	private async Task<(bool Success, MarkdownFile? MarkdownFile, DownloadMarkdownException? Exception)> 
		TryDownloadFileAsync(Uri uri, CancellationToken cancellationToken)
	{
		if (!IsValidFile(uri))
		{
			return (false, null, null);
		}

		var markdownFile = new MarkdownFile(uri);

		try
		{
			var content = await DownloadFileContentAsync(markdownFile, cancellationToken);
			markdownFile.Contents = content;
			return (true, markdownFile, null);
		}
		catch (HttpRequestException hre)
		{
			return (false, null, new DownloadMarkdownException(hre.Message, hre));
		}
		catch (Exception e)
		{
			return (false, null, new DownloadMarkdownException(e.Message, e));
		}
	}

	private bool IsValidFile(Uri uri)
	{
		var fileName = Path.GetFileName(uri.AbsoluteUri);
		return fileValidator.IsValid(fileName);
	}

	private async Task<string> DownloadFileContentAsync(
		MarkdownFile markdownFile, 
		CancellationToken cancellationToken)
	{
		var response = await httpClient.GetAsync(markdownFile.Path, cancellationToken);

		if (!response.IsSuccessStatusCode)
		{
			throw new HttpRequestException($"Could not download file at " +
				$"{markdownFile.Path}", null, response.StatusCode);
		}

		return await response.Content.ReadAsStringAsync();
	}

	private DownloadMarkdownFileServiceResult CreateResult(
		IEnumerable<MarkdownFile> markdownFiles, 
		IEnumerable<DownloadMarkdownException> exceptions)
	{
		return new DownloadMarkdownFileServiceResult
		{
			MarkdownFiles = markdownFiles.ToList(),
			DownloadExceptions = exceptions.ToList()
		};
	}
}
```

Now I'm approaching something that's highly decomposed compared to the first type I presented above. For me, it's not as readable, though that may be personal preference.

The key point here is that the benefits from this decomposition are less about readability and more about testability. How code reads is often more a matter of context. We could agree that, in isolation, each of these smaller methods is easier to read than the earlier types, but without the context around them, they only convey a single aspect of the process.

When methods do only one thing, they're either meant to be used by other types or are perhaps too small and should be consolidated into the types where they naturally fit. I believe that's in line with what Carmack mentioned. So, in a way, we're back to square one: writing code that balances readability, testability, and structure is challenging.

I could continue with a few more examples of what this type might look like. One example, and a language feature I rarely see used, is the use of local functions. I find them particularly appealing when working with functions needed only within a single type. This will be the last refactoring I present. It's been enjoyable to explore and demonstrate John Carmack's ideas, and exercises like this are always insightful.

```
public class DownloadMarkdownFileService : IDownloadMarkdownFile
{
	private readonly string[] validFileExtensions = { ".md", ".markdown" };
	private readonly List<DownloadMarkdownExecption> downloadExceptions = new();
	private readonly List<MarkdownFile> markdownDownloads = new();
	private readonly DownloadMarkdownFileServiceResult downloadAsyncResult = new();

	private readonly HttpClient httpClient;

	public DownloadMarkdownFileService(HttpClient httpClient)
	{
		this.httpClient = httpClient;
	}

	public async Task<DownloadMarkdownFileServiceResult> DownloadAsync(
		IEnumerable<Uri> uris, CancellationToken cancellationToken = default)
	{
		foreach (var uri in uris)
		{
			if (uri == null) continue;

			var fileName = Path.GetFileName(uri.AbsoluteUri);
			var extension = GetFileExtension(fileName);

			if (!IsValidExtension(extension)) continue;

			var markdownFile = new MarkdownFile(uri);

			try
			{
				await DownloadFileAsync(markdownFile, cancellationToken);
				markdownDownloads.Add(markdownFile);
			}
			catch (HttpRequestException hre)
			{
				downloadExceptions.Add(new DownloadMarkdownExecption($"{hre.Message}", hre));
			}
			catch (Exception e)
			{
				downloadExceptions.Add(new DownloadMarkdownExecption($"{e.Message}", e));
			}
		}

		return BuildDownloadResult();

		string GetFileExtension(string fileName) =>
			fileName.Contains('.') ? Path.GetExtension(fileName) : string.Empty;

		bool IsValidExtension(string extension) =>
			validFileExtensions.Contains(extension);

		async Task DownloadFileAsync(MarkdownFile markdownFile, CancellationToken cancellationToken)
		{
			var response = await httpClient.GetAsync(markdownFile.Path, cancellationToken);

			if (!response.IsSuccessStatusCode)
			{
				throw new HttpRequestException(
					$"Could not download file at {markdownFile.Path}", null, response.StatusCode);
			}

			markdownFile.Contents = await response.Content.ReadAsStringAsync();
		}

		DownloadMarkdownFileServiceResult BuildDownloadResult()
		{
			downloadAsyncResult.DownloadExceptions = downloadExceptions;
			downloadAsyncResult.MarkdownFiles = markdownDownloads;
			return downloadAsyncResult;
		}
	}
}
```
