[//]: # "title: Write. Push. Publish. Separating the concerns."
[//]: # "slug: write-push-public-separating-concerns"
[//]: # "pubDate: 17/9/2024 15:40"
[//]: # "lastModified: 17/9/2024 15:40"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

For the [local-first, feature-less, and admittedly boring static site generator that runs this blog](https://github.com/Danielovich/RubinStatic), I have had a few thoughts around a different model for materializing response output. You can always [read about the initial design thoughts](https://github.com/Danielovich/RubinStatic/blob/main/designthoughts.md) for Rubin.Static, I still do sometimes.

Today, when I write an article or post, I type the text in my favorite text editor as Markdown. When I’m finished writing, I push that Markdown file to a repository on GitHub.

Now, I have a file persisted in what I call my content repository. You could also call it my write store.

That write-push workflow is completely independent from publishing and updating the actual site, and that was an initial design decision. I wanted to keep writing content and pushing it to whatever write store I chose, independent of whoever or whatever wanted to utilize that content later on.

### Write. Push. Publish?

This article you are reading is not being materialized from data upon your request. It was put together before the request, hence the "static" part. It is a file on disk — a static HTML file.

When I want to update this site, I run a program locally on my machine called **Rubin.Static**. I execute a console program by running **.\run.ps1**, and from there I wait until the program completes its task.

- The program downloads every Markdown file from the write store every time it runs.
- The program parses every Markdown file every time it runs.
- The program generates all static files every time it runs.
- The program does not publish any files externally.

This is where my fraudulent programming mind starts playing tricks on me. I start thinking about parallelism for HTTP requests, wasted clock cycles when generating files whose content hasn’t even been touched and why even download files that haven’t changed ? And so on. Overengineering is the root of...and all that jazz. But I fallen for the traps before so these days I do not have to let the "creative department on the top floor" take over completely.

This approach around "write -> push" leaves the actual site eventually consistent. Even though I may have content in the write store, which is set as published, the site will not reflect that until the static files are generated and the site is updated. We could choose to call this a manual event, but it was no accident, I designed it this way on purpose.

### How is a post or article generated?

For every Markdown file downloaded, **Rubin.Static** parses it into a strongly typed model and generates a static HTML page from that model.

It does this via the Razor templating engine, with some Razor views and a Layout view. But it’s all within a console application, without a web server being fired up. There are some bounded dependencies.

At the end of an execution, I can navigate to **Rubin.Static's** output directory and find the site ready to run in a browser. It’s just HTML, CSS, and a little external JavaScript for code highlighting.

Where data is written to has nothing to should not matter. Hence the "reading" of data is almost entirely ignorant of where it was written to. That results in not using the same store for writes and reads. Write store is a github repository. Read store is a directory of web associated files.

[This is an example of separation of concerns](https://www.cs.utexas.edu/~EWD/transcriptions/EWD04xx/EWD447.html). I like this approach, and every time I use it, I smile. The whole notion of writing anything should be totally separated from any other concerns. And it is.

Today, when I execute **Rubin.Static** and add a new Markdown file (post/article), the Index page must be updated to reflect that. The Index page lists the contents of N new posts/articles.

There are also category pages generated if a post or article has a category "attached" to it.

So, with the Index page and Category pages, multiple static HTML pages are generated whenever a new post or article is committed to the repository (or removed, renamed, updated, etc.). However, today **Rubin.Static** cannot generate Category or Index pages without scanning every single Markdown file.

If I were to take this to an extreme and let my "creative department" go wild, I would experiment with event-driven file generation.

If I were to event-drive this, it would require a completely different page generation mechanism. I would most likely need to persist some interim data between receiving an event (such as a new Markdown file or a deleted one) and generating the static pages.

### How do you materialize views from data?

To make it crystal clear, the following **Rubin.Static**-supported Markdown file would generate a static HTML file called **this-is-the-slug-for-some-post.html** and also contribute to generating **software.html**, **code.html**, and **index.html**:


> [//]: # "title: Some post" \
 [//]: # "slug: this-is-the-slug-for-some-post" \
 [//]: # "pubDate: 14/6/2024 12:01" \
 [//]: # "lastModified: 17/6/2024 10:20" \
 [//]: # "excerpt: Whatever you want!" \
 [//]: # "categories: software, code" \
 [//]: # "isPublished: true" \
 > > Content goes here!


I would bet the most common way online applications materialize their views is based on collecting data "just in time." With the data at hand, we can then materialize and render it for the user. 

- User hits the endpoint (cache is invalidated!).
- The endpoint queries a database.
- The database retrieves the data.
- Some modeling based on that data occurs.
- The endpoint renders the model.
- 200 OK.

I have no issues with this approach. At all. The power of querying a relational datastore is incredibly useful. But since I’m already heading down the event-driven path in my mind, I imagine a different approach. This wouldn’t necessarily be the preferred option, but event-driven design could allow my read model to be eventually consistent based on events already received. 

In theory, this sounds good, but in practice, I find that designs and architectures around eventual consistency often being evanglized into a too too simple yet too broad approach. But who am I to judge without any context.

### How might events be applied in Rubin.Static?

- Publishing a new post or article to the content repository triggers an event.
- The event prompts **Rubin.Static** to only download the new post or article, or the event itself carries the data.
- **Rubin.Static** generates a new static HTML page based on the slug.

Now, the tricky part—because we don’t have all the data like we used to.

- **Rubin.Static** appends to the **index.html** view with content from the newly added post or article.
- **Rubin.Static** appends to each attached category with the new content.

The same process would apply for deleting or updating a Markdown file. Broad strokes, okay.

And suddenly, I’ve come up with an event-based update mechanism for the read model. It’s completely over-engineered and far too complex for where these designs apply correctly. But that's just me. I might try it as an experiment, but it will not end up in the main branch — it’s just too much.

With **Rubin.Static**, there is no relational data. I initially decided to forego that complexity, opting for Markdown as a low-common-denominator format. I wanted to write software without the need for a database.

So, there are only Markdown files to parse, and my own format parser to apply during the process. It’s all rather sequential. This has resulted in parsing and generating HTML pages, but I’ve already explained that part.