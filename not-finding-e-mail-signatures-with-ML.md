[//]: # "title: Not finding e-mail signatures with ML.Net."
[//]: # "slug: not-finding-e-mail-signatures-with-ML"
[//]: # "pubDate: 3/1/2025 12:01"
[//]: # "lastModified: 3/1/2025 13:20"
[//]: # "excerpt: "
[//]: # "categories: programming"
[//]: # "isPublished: true"

This is a piece on me trying to work with ML.Net, a machine learning library from Microsoft, training a model for predicting what is
an email signature and what is email body content.

I hoped that I would end up with a trained model that could predict with greater certainty what an email signature looks like in any email
I would serve to the model.

I am not a trained data scientist and I have no background in statistics either, but as a human-programmable problem-solver, I believed it would be
both a fun task and a fine approach to learning about both the ML.Net library as well as machine learning in general.

I started off totally blank but read the documentation on ML.Net and began writing some code. It struck me how well the library is put together; being a programmer,
it felt like I could start fairly easily and not dive too heavily into the underlying subjects. I did some research into the different machine learning algorithms due to
my passion for knowledge, but it did make my head spin a bit, which I definitely also expected, since nothing comes for free or without proper preparation.

I will perhaps gradually learn more in the time to come, but it does feel like a branch of computer engineering that deserves its own dedicated space of knowledge.

The feeling of using a trained model can be overwhelmingly positive, and what I believe is the average human impatience for searching for answers themselves,
makes models seem like both genuine progress and perhaps a bit of magic. But if you really want to know what is going on, as with everything in life, the journey is long and answers
don't arise without hard work.

As you will find, I did not succeed in my endeavor, whether because I am not good enough at labeling my data or, just as plausibly, because the library I chose is either not
mature enough or I used it incorrectly.

It becomes even more humorous since the email messages I had available were in "emlx" files, and therefore I had to write a parser before I could even start with the actual machine learning code. Emlx files are Apple Mail's proprietary format, and even though they resemble RFC 5322 and in turn RFC 2822, they act as a superset in some aspects. A typical email formatted as emlx could look like this:

```
5028      
Return-Path: <kate@xzy.com>
X-Original-To: www@vvv.com
Delivered-To: xxx@xxx.com
Received: from localhost (localhost [127.0.0.1])
	by xxx.wannafind.dk (Postfix) with ESMTP id C282E2710010
	for <xxx@xxx.com>
X-Virus-Scanned-By: Wannafind A/S
X-Spam-Flag: NO
X-Spam-Score: -0.449
X-Spam-Level: 
X-Spam-Status: No, score=-0.449 tagged_above=-999 required=3 tests=[AWL=0.249,
	DKIM_SIGNED=0.1, DKIM_VALID=-0.1, HTML_MESSAGE=0.001,
	RCVD_IN_DNSWL_LOW=-0.7, URIBL_BLOCKED=0.001] autolearn=unavailable
Received: from mail26.wannafind.dk ([127.0.0.1])
	by localhost (mail26.wannafind.dk [127.0.0.1]) (amavisd-new, port 10024)
	with ESMTP id Q1wWb6BNZMRl for <xxx@xxx.com>;
	Fri, 17 Mar 2017 22:07:15 +0100 (CET)
Received: from mail-qt0-f182.google.com (mail-qt0-f182.google.com [209.85.216.182])
	by mail26.wannafind.dk (Postfix) with ESMTP id CF0952710001
	for <xxx@xxx.com>; Fri, 17 Mar 2017 22:07:15 +0100 (CET)
Received: by mail-qt0-f182.google.com with SMTP id n21so72649531qta.1
        for <xxx@xxx.com>; Fri, 17 Mar 2017 14:07:15 -0700 (PDT)
DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed;
        d=swellsailing-com.20150623.gappssmtp.com; s=20150623;
        h=mime-version:from:date:message-id:subject:to;
        bh=PAhF8i05o9wx4Ma9+vOYJAusyA2JndcqLmzT4Q3MKAU=;
        b=UKtfvvYoLf5dG59oJ46upzV1C/tmVCnm04CFaILxSx4ZHqBcZkafCUopnNwnlvJHdW
         9ucIZV+4C80+IOI2aIv3cS3NG968qGkBn3mdl8Y6N/lfe3i8GHLtYaA0ci+wTft5VDun
         TdJXacCe8P4eRhkUlkYcWOfn3NWWv21gJccveBakoyUYXjxejWy3Dz+wLMqRU993XeUL
         ffIG455Zwzky8rZ7AQzV4CoGa+TXxOlOcdZZCOLqXlLgSM/QD6pXCYktnuNm43ekK5r5
         JCdPN+Xz1J1cRoXpdo14PfCXMVX5WMFK/MvAGDpfOBP7nCH1wCVa0AVCDymZZVp02la3
         ++/w==
X-Google-DKIM-Signature: v=1; a=rsa-sha256; c=relaxed/relaxed;
        d=1e100.net; s=20161025;
        h=x-gm-message-state:mime-version:from:date:message-id:subject:to;
        bh=PAhF8i05o9wx4Ma9+vOYJAusyA2JndcqLmzT4Q3MKAU=;
        b=aZydM85qslAlbt2d1eu7DMrz6oaqrnQ6NEhKga59CFFZWL63Jfyz8cWEGjw7cVMIbC
         11yNazwr5tez0n/NuuC6wxxWUabUKx8dNXWdCEkHBP5gsd19ktRg/jIS2i+LUB7mO1Nb
         89XZo7E40ukf7GkGsGfox1nG4VbVR7NlJphr+IwlJErzAR/sw+OK778HgWvbS1rLTwqo
         a+/4EtPazKYl2QbkGtO/M0tpcbi05XZMIxEIz+PCAQOsCpiijERJGubhp0Vo+6aq7OsQ
         yOJ/aw7bIEtp7fhnOARyHD++He0LAhbemz/OiZxguB06E6u015AEHt8jrGNAcfb6TxI7
         ILbA==
X-Gm-Message-State:
X-Received: 
MIME-Version: 1.0
Received: 
From: Kate <kate@xzy.com>
Date: Fri, 17 Mar 2017 17:06:34 -0400
Message-ID: <CABvh=O_K-0rtWDVAjspNPR6UijEnu0ga4xEXn+04hMoaAyYL4A@mail.gmail.com>
Subject: schedule 3/18 - 19
To: Andrew <andrew@xxx.com>, Colin <colin@xxx.com>
Content-Type: multipart/alternative; boundary=001a113d0ce6dfa2e0054af38f25

--001a113d0ce6dfa2e0054af38f25
Content-Type: text/plain; charset=UTF-8

hi hi
your charts are clear

have a lovely weekend!
xo

Kate Moshbure
Swell Sailing Caribien <http://swellsailing.com>
Orlando Street, Suite 112
O.345.124.1433
C.918.112.4322
@swellsailing <http://instagram.com/swellsailing>

Cancellation Policy: Confirmed bookings cancelled less than one calendar week
from the depature date are subject to a 50% cancellation fee. Confirmed bookings
cancelled within 3 calendar days or less from the depature date require 100%
payment.

--001a113d0ce6dfa2e0054af38f25
Content-Type: text/html; charset=UTF-8
Content-Transfer-Encoding: quoted-printable

<div dir=3D"ltr">hi hi<div>your charts are clear</div>

--001a113d0ce6dfa2e0054af38f25--
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>date-last-viewed</key>
	<integer>1490088118</integer>
	<key>date-received</key>
	<integer>1489784836</integer>
	<key>flags</key>
	<integer>8590195717</integer>
	<key>remote-id</key>
	<string>2151</string>
</dict>
</plist>
```

As I write this, I am looking at the code for extracting the actual content of the email. Since you cannot control the output, all kinds of things can appear in such a message. If you have ever parsed HTML documents for validation, you know a bit of what I am talking about. You would also be aware that removing and trimming whitespace and new lines is crucial. I am merely trying to emphasize that inspecting computer formats can be difficult.

Ok, so after running through some 30GB of emlx files, I ended up with a 335MB CSV file containing email content.

A content blob in the CSV could look like this (the CSV columns are there too):

```
date,from,to,subject,body
"2017-03-17T15:37:52.0000000+01:00","ddd@ggg.com","xxx@eee.com","Re: Avffen","Kære Krølle
Tusind tak for begge mails med flotte skibe.
Jeg vil gemme skibene i vores arkiv og så vender jeg tilbage når vi har nogle spændende nye sejladser :-)
Ønsker dig en dejlig weekend.
 LOVE
 Johhny Money
 Project Manager 
 Sugar Sailing ApS
 Hannesgade 16 - 1st floor
 1208 Copenhagen K
 Denmark
 Office: +45 1234 1234
 sugarsailing.com <
 FOLLOW US ON INSTAGRAM <"
```

Now, this message is actually decently formatted. It has left me with a proper message body and a well-established signature too, which is what I came for in the first place.

However, it is not uncommon for some of the content to contain gibberish and strange formatting like > < + ? and so on. What I found out later is that your data must be absolutely as clean as possible. The more solid your data foundation, the more solid your model becomes. Cleaning and preparing data is definitely not a job to take lightly; it takes time and multiple iterations. And when you have lots of data, it takes even more time.

If you are reading on and believe your favorite AI chatbot could do this in a jiffy, be my guest and try. Please let me know how well it went.

The next task for me from here is to label email bodies and signatures, somehow building a file for later use that can tell our model builder in ML.Net what is signature text and what is actual email text. So, I wrote another program to help me with that. It allows me to mark what text in an email is a signature and what is not. It would be persisted in a new CSV file for later use and have a format similar to this:

```
Text  |   Label 

"Best Regards", "positive"
"Med venlig hilsen", "positive"
"Tak fordi du vendte hurtigt tilbage", "negative"
```

As you might have noted already, one of these is a Danish signature "Med venlig hilsen," and I knew it might cause me trouble—I wasn't certain, though—since I mixed
Danish and English signatures, as well as email content. But hey, why wouldn't a static algorithm be able to handle both languages?

Of course, in real life, a signature rarely looks like any of the above. There might be images, GIFs, HTML, long corporate marketing phrases, and sometimes... nothing at all.

A more realistic signature looks a lot more like this:

```
Med venlig hilsen / Best Regards Peter Jensen Firma ApS office: +45 22 11 22 11 cell: +45 11 22 22 11 www.jajaja.dk < instagram < Store Kongensgade 12  2.sal 1264 København K Danmark", Positive
```

Before I continue, let me just recap for the sake of my own memory and try to make it clear what I learned from a few days of programming these things.

Extracting emlx files from Apple Mail is not difficult, but reading a macOS-formatted disk can be tricky on Windows. I used HFSExplorer.

Emlx is closely related to multiple RFCs and is quite okay to work with for tasks such as determining what is a "From" field and what is an "HtmlBody." However, the DTD of http://www.apple.com/DTDs/PropertyList-1.0.dtd is not documented anywhere but can represent XML like this:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>date-last-viewed</key>
	<integer>1490088118</integer>
	<key>date-received</key>
	<integer>1489784836</integer>
	<key>flags</key>
	<integer>8590195717</integer>
	<key>remote-id</key>
	<string>2151</string>
</dict>
</plist>
```

1. Any actual textual extraction from a computer format, such as HTML, CSS, or emlx, is tricky. Parsers of such kind are, to my knowledge, impossible to get right with RegEx. I relearned this while writing this software since I used RegEx for extracting content.
2. I didn't anticipate or prepare for a cleaning pipeline around emails to have this many steps. This was not because I didn't respect the complexity, but simply because I didn't prepare beforehand.
3. The cleaning pipeline consists of:
   1. Raw emlx format extractions
   2. Content extractions from raw email format
   3. Labeling signatures from actual communicative content

To sum it up, I now have a 335MB CSV file with email content, cleaned reasonably well. I also have a file with positive and negative examples of what a signature could look like and what is definitely not a signature. This file is based on a subset of the file with email content.

As far as I understand from reading about basic machine learning concepts, this leaves me with the option of training a model using the positive and negative file. The model is supposed to learn from those entries and "teach itself" how to determine what an email signature looks like.

Now, if you are wondering why I am doing this, it is because if I succeed in extracting signatures from the actual content, I plan to take on a different task: trying to classify the actual email content. To what extent, I haven't decided yet. One could imagine a machine learning model capable of identifying historical tendencies in emails. That model could potentially focus on sales patterns, negotiation strategies, or other interesting insights. But that is for future exploration.

As I mentioned earlier, ML.Net seems to be an easy enough library to work with. The challenge is that it packages functionality for which I have little cognitive reference,
which, of course, is not ML.Net's fault. This is the nature of such abstractions. They may be too easy to use for the uninitiated, and when you really need to tweak the parameters, you cannot avoid learning the underlying details. I believe I have fallen into that trap with this model.

```
var mlContext = new MLContext();

var posNegFilePath = "posneg.csv";

IEnumerable<EmailText> data = File.ReadAllLines(posNegFilePath)
    .Skip(1) // header of csv
    .Select(line =>
    {
        var columns = line.Split(',');
        return new EmailText
        {
            Text = columns[0].Trim(),
            Label = columns[1].Trim().Equals("Positive", 
                StringComparison.OrdinalIgnoreCase)
        };
    });

var dataView = mlContext.Data.LoadFromEnumerable(data);

...
```

From here on out there were some back and forth on how to use the ```mlContext``` for resulting as high a predictability as possible. 

```
var pipeline = mlContext.Transforms.Text.FeaturizeText("Features", nameof(EmailText.Text))
    .Append(mlContext.BinaryClassification.Trainers.SdcaLogisticRegression(
        labelColumnName: nameof(EmailText.Label),
        featureColumnName: "Features"));

var signatureModel = pipeline.Fit(dataView);
```

I am really not sure if TF-IDF is the best option for what I am trying to do here, since, to my knowledge at least, words that occur commonly are given
lower (IDF) weights. But this is shaky ground for me, as it feels like I am configuring a black box rather than truly understanding what I am doing, one abstraction at a time.

```
var predictions = signatureModel.Transform(dataView);
var metrics = mlContext.BinaryClassification.Evaluate(predictions, labelColumnName: nameof(EmailText.Label));

Console.WriteLine($"Accuracy: {metrics.Accuracy:P2}");
```

Luckily, the model is easy enough to train and evaluate, and neither process takes long. However, I find the results questionable. How can I achieve an accuracy above 90% from just 1,000 entries in the underlying file with positives and negatives? Could it be because I am evaluating the model on the same data that I used to train it?


```
Train or Run
train

Accuracy: 94.00%
```

When I run and use the model, rather than training it, it does not yield any results I am confident in. I could argue that this is somehow an accumulated positive bias, but then again, not really. I might be wrong, but "Denmark" should not have a score of 99.71%, and "Follow us on Instagram" should perhaps have a higher score?

```
[Signature Detected]: Ønsker dig en dejlig weekend. (Probability: 96.05%)
[Signature Detected]:  LOVE (Probability: 96.73%)
[Signature Detected]:  Robert Tobiassen (Probability: 93.27%)
[Signature Detected]:  Project Manager  (Probability: 98.21%)
[Signature Detected]:  ZigZag ApS (Probability: 96.91%)
[Signature Detected]:  Johnny Madsensgade 6 - 1st floor (Probability: 92.83%)
[Signature Detected]:  1208 Copenhagen K (Probability: 99.69%)
[Signature Detected]:  Denmark (Probability: 99.71%)
[Signature Detected]:  Office: +45 3344 7722 (Probability: 99.15%)
[Signature Detected]:  zigzag.com < (Probability: 98.41%)
[Signature Detected]:  FOLLOW US ON INSTAGRAM < (Probability: 81.16%)
```

Look at this one 

```
[Signature Detected]: How old are they? Are they full time?  (Probability: 98.99%)
[Signature Detected]: Thank you (Probability: 99.86%)
``` 

It does seem like a bad model to me, and it also seems that my data might not be good enough. So, I need to revisit the cleaning pipeline and labeling of data. While I am at it, I think I will use only English.

A failed attempt so far, but I will try to give it another go in the not-too-distant future.