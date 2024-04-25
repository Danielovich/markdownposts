[//]: # "title: Stavekontrol er svært. Børnene vil hellere spille. Jaro, Hamming, Damerau og Levenshtein er alle døde."
[//]: # "slug: stavekontrol-er-svaert"
[//]: # "pubDate: 24/04/2024 12:01"
[//]: # "lastModified: 25/04/2023 08:46"
[//]: # "excerpt: "
[//]: # "categories: software"
[//]: # "isPublished: true"


> "Ordet 'sykel' på dansk oversættes til 'le'. Det er en gammeldags term for en type håndredskab, specifikt en lille sigd, der blev brugt til at klippe korn eller græs." - Hilsen ChatGPT.

I et forsøg på at afhjælpe en ung drengs udfordringer med at stave, tænkte jeg, at det samtidig ville være en fornuftig udfordring at lære noget om strengalgoritmer. Dette kommer delvist fra nogle samtaler omkring spisebordet om aftenen, hvor drengene udviser stor interesse for, hvordan en computer i virkeligheden fungerer.

De forstår – ligesom størstedelen af menneskeheden – ikke, hvorfor det skal være så svært at programmere et spil eller en bot, der kan spille WoW for dem, så de kan læne sig tilbage og senere nyde profiten af deres profilsignifikans – og også købe de sko og biler, de ser på TikTok og hører om i raptekster fra Biggie Smalls eller bare fra TV2s Fantastiske Toyota.

Ak, der er meget at forstå hvis man tror noget .

De fleste af os tager vel for givet, hvilken hjælp vi egentlig får, når vores computer udøver stavekontrol for os. Hvordan kan det gå til, at en computer forstår, at når vi forsøger at stave til "cykelhjelm", men i stedet taster "cykelhjælm", at noget inden i maskinen "forstår", at "æ" skal være et "e".

Hvis du ikke er blandt den sjældne race af algoritmedesignere eller på anden vis har studeret grundlæggende "computer science" – jeg har ikke – så er du måske også forundret over, hvordan en algoritme bærer sig ad. Vi er ofte ikke eksponeret direkte for algoritmens muligheder; de er ofte pakket væk fra os, ligesom når du åbner Word og staver forkert – du ser ikke direkte, hvordan et BK-tree eller et Bloom-filter fungerer på skærmen. Det er abstraheret væk fra dig, så du ikke skal bekymre dig. Medmindre du naturligvis ikke kan lade være.

Min logik er anderledes end din, og sådan er det at være menneske. Vi anskuer udfordringer forskelligt, nogle mere end andre. Men heldigvis er der mennesker, som har fundet gode løsninger på fælles udfordringer, så vi ikke selv behøver at tænke alt for længe over dem, medmindre – igen – at vi synes, de på en eller anden måde fortjener noget opmærksomhed.

At beskæftige sig med ["Tokenization"](https://en.wikipedia.org/wiki/Lexical_analysis#Tokenization), hvor vi har med mere tekst at gøre, og faktisk også noget kontekst, end blot et enkelt ord, har jeg ikke beskæftiget mig med her. Men det gør nødvendigvis ikke udfordringen mindre.

Prøv i stedet at få din logik til at forstå, hvad der skal ske inde i et computer-program, når du har stavet "sykeljælm" og ikke "cykelhjelm". Vores egen hjerne staver det korrekt, hvorfor mon ?

Vi ved begge godt (medmindre vi lider af ordblindhed), hvad det korrekte ord er; "cykelhjelm". Men hvordan ville vi ændre ordet til det rigtige ud fra den forkerte sekvens ("sykeljælm")? En computer kender ikke på forhånd ordets korrekthed, så den er nødt til at gætte sig frem ud fra den sekvens, den har fået at arbejde med. "Sykeljælm", tænker computeren, hvad gør jeg nu? Hvilke ord "minder" ordet om?

Vi er ude over, at ordet er korrekt. Så når computeren slår op i ordbogen, hvor alle danske ord er stavet korrekt, finder den ikke noget. Øv! Så langt, så godt, så vi skal have fat i en eller anden algoritme for, hvordan ordet kan "transformeres" indtil noget, vi kan genkende.

Efter et par omgange med vores favoritsøgemaskine kan vi blandt andet finde en fremragende post af [Peter Norvig](https://norvig.com/spell-correct.html), men vi finder også hurtigt ud af, at for at kunne arbejde med vores forkerte sekvens "sykeljælm" skal vi over i noget "distance ensarthed" i forhold til, hvor meget en sekvens minder om en anden sekvens. Jeg kunne godt have sat mig ned og brugt en masse tid på at gøre som Peter Norvig, men jeg ville aldrig have nået et lignende resultat af den simple grund, at min viden omkring emnet er for lille og jeg tvivler egentlig også på, at jeg er ligeså smart.

Jeg brugte rundt regnet et par dage på at læse op på, hvordan de mest gængse "distance ensarthed" algoritmer fungerer, og det er som alt muligt andet i programmering: trade-offs omkring performance, kompleksitet og tid. Jeg kan efterhånden forstå, at det på ingen måde er nemt eller ukompliceret at bygge software, som kan udøve en stavekontrol, som du for eksempel finder i Word.

Wikipedia siger om ["Edit Distance"](https://en.wikipedia.org/wiki/Edit_distance): *"In computational linguistics and computer science, edit distance is a string metric, i.e., a way of quantifying how dissimilar two strings (e.g., words) are to one another, that is measured by counting the minimum number of operations required to transform one string into the other."*

Jeg bider her mærke i *"measured by counting the minimum number of operations"*. Hvis vi bruger det i vores eget eksempel her, kan vi måske finde ud af, hvor meget forskel der er på "sykeljælm" og "cykelhjelm".

En tilsyneladende velkendt "edit distance" algoritme er [Levenshteins](https://en.wikipedia.org/wiki/Levenshtein_distance), som i helt korte træk lyder: *"The Levenshtein distance between two words is the minimum number of single-character edits (insertions, deletions, or substitutions) required to change one word into the other."*

Læg mærke til, hvordan og ud fra hvad algoritmen arbejder: (insertions, deletions, or substitutions).

Så hvis vi lige prøver at bruge den algoritme, og lige for en stund leger, vi kender den korrekte sekvens "cykelhjelm", så vil algoritmen fortælle os, at de to sekvenser har en "edit distance" på 3, fordi det kræver tre ændringer til inputsekvensen at ændre den til den korrekte sekvens.

Input: sykeljælm. Test imod: cykelhjelm.

- Ændre 's' til 'c' på position 1. Resultat: "cykeljælm" 
- Ændre 'æ' til 'h' på position 6. Resultat: "cykelhjlm" 
- Indsæt 'h' mellem 'h' og 'j' på position 7. Resultat: "cykelhjelm"

(1 insertion, 2 substitutions, 0 deletions).

Det er som sagt tre ændringer.

Jeg forsøgte initielt også med [Hamming-distance algoritmen](https://en.wikipedia.org/wiki/Hamming_distance) men opdagede hurtigt, at den forventer, at de to sekvenser er af samme længde, hvilket vi ikke kan garantere.

Der er en enkelt ting ved den oprindelige Levenshtein distance algoritme, som vi potentielt godt kunne tage højde for, og det er det, der kaldes "transpositions". Det betyder, at algoritmen også forsøger at bytte om på hver karakter i en given sekvens. Wikipedia beskriver kompleksiteten heraf således: *"Adding transpositions adds significant complexity. The difference between the two algorithms consists in that the optimal string alignment algorithm computes the number of edit operations needed to make the strings equal under the condition that no substring is edited more than once, whereas the second one presents no such restriction."*

Frederick J. Damerau bliver beskrevet på Wikipedia: *"In his seminal paper, Damerau stated that in an investigation of spelling errors for an information-retrieval system, more than 80% were a result of a single error of one of the four types."*

Derfor findes der en [Damerau–Levenshtein distance algoritme](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance), som er en udvidelse af den oprindelige Levenshtein distance algoritme, men som også tager højde for "transpositions". Initiativt ville jeg helst lave en løsning, der kunne stave bedst muligt på baggrund af betydningsbærende danske ord, altså ikke ord som f.eks. "hvad", "hvem", "stop" eller "kat", men mere ord som "kokkeskole", "psykolog", "lærerinde". Jeg har dog ikke testet simple ord med nogen af disse algoritmer.

Vores eksempel med "sykeljælm" og "cykelhjelm" ændrer ikke ved antallet af ændringer. Det er stadig 3 ved brugen af Damerau–Levenshtein.

Hvis det ikke allerede er faldet dig ind, så handler det om at have så få ændringer som muligt for at kunne kvalificere de bedste gæt på, hvilket ord der er korrekt.

Lad os løbe igennem et par forskellige tests, hvor vi bruger en given kandidat og en liste med ca. [650.000 danske ord](https://da.wikipedia.org/wiki/Ordforråd).

Levenshtein resultat med sekvensen "sykel":

- cykel, 1
- sekel, 1
- skel, 1
- ankel, 2
- bydel, 2

Fair nok, det er jo et gæt. Men jeg tænker allerede helt stille og roligt, vi måske kan bruge det resultat og gøre det bedre. Måske.

Vi prøver lige med Damerau-Levenshtein og "sykel". Giver samme resultat.

- cykel, 1
- sekel, 1
- skel, 1
- ankel, 2
- bydel, 2

I begge tilfælde, med hver af vores valgte algoritmer, er resultatet 1.

Hvad nu hvis vi antager et øjeblik, at når resultatet er 1, så forsøger vi at kvalificere vores gæt på baggrund af antallet af korrekte karakterer i sekvensen "sykel" sammenlignet med antallet af karakterer i de sekvenser, som vores valgte algoritme har foreslået.

Så "sykel" vil altså minde mere om "cykel" fordi de to sekvenser deler 4 karakterer. Det er en naiv tilgang og vil ikke fungere særlig godt i praksis, men vi kan altid kassere resultatet, såfremt ensartetheden ikke er over en vis procentdel. Jeg forsøgte også at give resultatet af Levenshtein algoritmen til en [Jaro algoritme](https://en.wikipedia.org/wiki/Jaro%E2%80%93Winkler_distance#Jaro_similarity), men her fik jeg "skel, cykel, sekel" tilbage.

Inden jeg gik i gang med at kigge på udfordringen, vidste jeg godt, at det ikke ville være let at lave. Og jeg er fuldstændig klar over, at jeg kun lige har dyppet min storetå i streng-distance algoritmerne, så min læring har været ret fundamental og relativt ny. Men det er præcis derfor, disse ting er gode og lærerige at begive sig ud i.

Til min forbavselse, bliver "sykel" til "sekel" i min version af Word.