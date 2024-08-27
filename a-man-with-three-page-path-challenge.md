[//]: # "title: A Man with a Three Page Path Challenge"
[//]: # "slug: a-man-with-a-three-page-path-challenge"
[//]: # "pubDate: 1/8/2024 10:22"
[//]: # "lastModified: 27/8/2024 9:22"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"

During a stay on a small island in Kattegat, one night I was thirsty for company and crept offboard and into the sunsetting night of imagination and warm air around me. The waves licking shore into true randomness of sparkling diamonds from the low sun and the sandy banks still covering the desert while people calmly sipping on colored drinks and wine in their cockpits.

I found a small bar in the northwestern end of the harbor, a wooden shack with an older lady as barkeep. Twenty years ago, this place was nothing but fishing boats and an anchorage bay. She looked like she couldn't do her job much longer, but she still looked nothing like other people's integrity. I didn't blame her. We are all for sale, I thought. I ordered two drinks for myself, which is an old habit, one for a thirsty palate and the delightful possibility of not drinking just one thing.

Then I sat down, leaning my back on the wooden shack and watched the sea with no land in sight. Hazy colors of yellow and light brown are easy on the eyes, and it's the only time in the summer I am not wearing sunglasses. Next to me sat a few people that looked like they were about to leave. They greeted me, and I returned a handwave. The mind wanders when you are off the load for a longer period. A short man with round glasses and a beard approached the shack, came up to me, and sat right across. Nothing stopped him as he started talking to me. I looked around to see if he was conversing with someone unknown to me. I realized he was talking to me, and he was letting that tongue go about his trip and soon his long-awaited vacation from his work.

As Dire Straits' "Wild West End" played softly in the background, we chatted a bit. A lot of people love talking about their jobs; sometimes, it seems to me it is their sole adventure for success. Work are most people's lives, I suppose. So there we went. Luckily for me, it turned out he was very much into programming, as was I, how the wondrous alleys of playful thoughts and creativity can be enjoyable at first, but get a tunnel vision and try to auction it off, will fool you, and when money is involved, your chance of getting out in time is low. You can't succeed trading your passion. Yet he seemed tense and wanted to let me know that what he did was very special. Often people really believe their jobs are special. As the subtle talk went on, it came to my knowledge that, at this given time, he was looking for candidates for a job in some industry I had soon forgot.

He had found it difficult to interview programmer candidates; either they were too stubborn or too con-man-like. Down my alley, I thought. If you cannot lie, you have no chance - look at the industry I thought, perhaps it looks like Klondike - but most people seem to forget that they are silently applauding the ones who are cheating them from their own success. And it always ends up in the same dead end, claiming "it's just a job." And nobody really seems to mind - the circus is full of light, the man on the stool takes your money and in the morning we do it all over again. We talked about his candidate interview practices, and as he had calmed down a bit, I found him to be a decent man. Since I am absolutely not a finalist in the act of job interviews, I could lean into it without coming across as someone with potential. I have always had a quite extinguished dislike for the human across from me who wants so badly to make me answer questions in time or show how I perform with the hourglass turned. Looking for the truth without ever questioning their our own answers.

I needed to refill my two drinks and went back to the barkeep, who had become angry. Cheer up, I said, summer is almost over. She smiled, but it was hardly genuine, which is almost like being turned down for a job - with an almost genuine smile. She would be a brilliant interviewer, I thought. A vodka with sparkling water and a rum and chocolate milk. Thank you.

I came back to my new acquaintance, who had lit a cigarette and had become quiet. The quiet didn't last very long. As we sat there talking about the recent programming books we had read, I had to remind myself not to mix business with sailing (or any other pleasure), as it's often a given that when I start answering questions, I either do not understand why the questions are relevant or why we are so obsessed with finding a quick answer. It's a contradictory world. What nature and the sea offers you is slow and takes its time, compared to the world of technology, it is incomparable. Yet we tell ourselves over and over, trying to convince, that things on a screen can be beautiful. People either do not spend much time outside or need larger glasses. You can't save them all. More want's more, while time flies, being the only true currency I measure things in.

The drinks help me to swallow it all. Somehow, I fumbled and asked what my new friend's challenges were during interviews with programming candidates. Now I had done it, I had walked right into it, I thought, and I regretted it instantly. But somewhere in the back of my skull, emptied from being on the sea, I knew I had to know why this was so difficult.

An hour went by, both of us loosened up in regards to our stance towards the challenge thar these programming candidates were given during an interview. The night soon turned dark and hollow. Most of the harbor asleep, and the sizzling water had calmed down, not so diamond sparkly any longer. I found my way back to the boat, stepped aboard, and slowly turned into my woman's warm body. I lay there in the stern cabin and thought about the challenge this human I had had a friendly encounter with. As a programmer with an issue at hand, you start thinking about a solution. This time was certainly no different.

### The Three Page Pattern Challenge for the Programming Interview.

You have a log file which is from a website. It has 3 columns. There are a million rows in the log file.

The columns in the file is "UserId", "Path", "LoadTime". It's a given that its ordered by a timestamp which is not present.

A few rows could look like this. Here I have taken the authors executive and ordered the list for you but in the log file users are not ordered.


> 1, default.html, 87 \
2, default.html, 37 \
3, default.html, 57 \
1, jobs.html, 22 \
2, drinks.html, 34 \
3, jobs.html, 58 \
1, about.html, 11 \
2, food.html, 89 \
3, about.html, 43 \
1, food.html, 36 \
2, jobs.html, 78 \
3, order.html, 66

So can we imagine an interviewer would ask us ? Horrible question to get under pressure. What programming was ever done with quality under pressure ?

Try to see if you can make sense of these questions ?

- What is the most common three page pattern for all users ? 
- What is the frequency (how many times) of the most common pattern for all users ?
- How many different three page patterns are there in total ?
- (bonus) What is the fastest most common three page pattern for all users ?
- (bonus) What is the slowest three page pattern for all users ?

Since this is not a question in an hour long interview I actually have a chance as well. I would not be able to make a programming solution for this challenge with dignity or certainty within a short time. I am not a fast programmer and I do not have faith or trust in the code that I write before I have iterated over it for some time.

From here on I will try to make an iterative solution for this challenge, write about my thoughts and solutions, and I might be wrong and fail, but at least I get to excersise my programming skills.

I have made a [log file for the challenge](references/a-man-with-a-three-page-challenge/logfile.csv) which you can download here if you want to give it a try.