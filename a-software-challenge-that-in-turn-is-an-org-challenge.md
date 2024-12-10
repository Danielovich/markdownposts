[//]: # "title: A Software Challenge that in turn is an Organizational Challenge"
[//]: # "slug: a-software-challenge-that-in-turn-is-an-org-challenge"
[//]: # "pubDate: 10/12/2024 12:01"
[//]: # "lastModified: 12/12/2024 13:20"
[//]: # "excerpt: "
[//]: # "categories: organisation, software"
[//]: # "isPublished: true"

This is an exercise where there is no right or wrong answer. It is up to you to determine how the establishment of communication between two pieces of software should be put in place. You have no organizational power, so you cannot change how management has laid out the organization. You are part of a product team that develops a software product and uses other teams' products and data.

### Today you have

1. **ImageTexts** (a Git repository hosted on GitHub).  
2. **ImageGen** (an executable program).  

Today, **ImageGen** is an executable program that **pulls** file data from **ImageTexts** and works its magic from there.

The only thing ImageGen knows about ImageTexts is the **Uri** of the Git repository. **ImageTexts** implements a file data format that ImageGen also knows about - it could just as well have been a binary format like PNG or JPEG. However, they do not share any code paths. They are completely separate on a technical level aside from knowing the same file format.

This separation exists partly because they deliver two different value streams to the business, and the orgnisation is unaware of how this might cause trouble:

1. **ImageGen** is a product, a piece of software, created by your product team.  
2. **ImageTexts** is essentially a file store (really just a Git repository at its core) implemented by another product (pick a name!) that is managed by a different team. Marketing staff use this product to write the texts that generate AI images. Marketing, of course, does not know anything about the ImageTexts Git repository. Nor should they.

**ImageGen** generates and saves images by pulling the files and formats persisted by ImageTexts. For simplicity, we can assume ImageGen saves its created images locally on disk.

The thing is, *this works fine*. Pulling information is easy and has worked well for a long time. Creating images is also straightforward. Disk is blazing fast, and everyone has been happy.

### Your new Problem

But one day, someone in marketing - the people who write the cool texts that ImageGen turns into images - says:  

*"Wait, why do we have to wait 10 to 20 minutes for our images to show up? Why can't I get the AI image as soon as I save the text that it should generate from?"*  

Faster results they want!

You know this delay might be because **ImageGen** is pulling data from the **ImageTexts** Git repository at some fixed intervals. The execution frequency seems to be too small for marketing's needs. They're busy people man!

It also sounds like marketing would like to use some kind of **push mechanism** so that ImageGen "wakes up" immediately when a file is saved to the ImageTexts git repo.


### Push Mechanism and Events

This problem sounds familiar: some **event** happens (a file is saved in ImageTexts), and something else should respond to it (ImageGen generates an image).  

Conceptually it could like a bit like any other event mechanism:  

```
Action -> Event -> Action -> Event
                          \
                           -> Event
```

You are well aware of the technical side, having done this for many different products. You believe it would not be a problem to implement a push mechanism, on the technical side. 

For example, the product that marketing uses could push events about new files in the **ImageTexts** repo to a **queue, bus, or publisher**. Then, on the other side, **ImageGen** could start listening to those events.

There are other things to be aware of.

You are not part of the team responsible for the **ImageTexts** repository. This means you would likely need to create a ticket asking for the new functionality. They don't accept pull-requests from others. But how busy is that team? Can they prioritize it? And marketing is on your back every week about this.

If the team cannot prioritize the request, are there alternative approaches that don't require changes on their side?

As you think a little about this, new questions arise:

Who should be responsible for running and maintaining the queue, bus, or publisher?

If something breaks in this proposed push-based mechanism, who will troubleshoot it? The ImageTexts team? The ImageGen team? Someone else?

You also wonder...

Can we just increase the execution frequency of the current pull mechanism in ImageGen? Maybe this simple solution will be sufficient for marketing's needs ?

Who is to determine the golden path of how important this is and who is in charge ? Do I call on some kind of leadership ?

Remember a few of the things you are dealing with:  

1. Two systems, decoupled and unaware of each other at a technical level.  
2. Two different product teams: one for ImageGen and another for ImageTexts.
3. No shared packages or code - only a URI and a file format, implemented separately in each system. 

If you have read this being a programmer, you might already have come to a technical solution. In that case, have you thought about what strains the solution will leave on the rest of the organisation ? 

Have you made any considerations of how large the organisation is ? How slow or fast it is ? Have you considered how communication flows and who is in charge ? Even though this might resemble the company you work for today, it is not. Respect that and understand that context when you start inspecting for solutions.

Like I stated in the beginnning, use this as an excersise. There is no right or wrong answer. You might be able to learn from it or you might have a too large ego or too simple answers for contexts you are unaware of.

But given these constraints, what would you do?