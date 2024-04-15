# Microservices udstiller organisationens kommunikationsstrukturer, hvilket medfører, at enhver svaghed og styrke i den, bliver synlig.

> Published on Feb 11, 2024

Microservices er en ubarmhjertig arkitektur. 

Ikke kun fordi den stiller andre krav til de teknologiske aspekter der differentierer den fra den traditionelle organisations systemudvikling. 

Men i højere grad fordi den udstiller organisationens design men samtidig forventer klarhed over, hvor de enkelte services er forankret, i hvilke teams.

Microservices er blevet udskammet og rost, ligeligt. Udskammet af teknologister fordi designet ikke kan stå alene, teknologisk. Rost fordi, organisationen der rent faktisk formår at ændre kommunikationsstrukturene imellem deres teams, også er dem som får mere ud af designet end blot en teknologisk edge.

Microservices primære succes-parameter handler næsten udelukkende om hvordan kommunikationen flyder i organisationen, ift. hvordan kontrol og forankring placeres. 

Microservices repræsenterer således et udfordrende paradigme, primært fordi grundlaget for succes i høj grad ikke blot afhænger af teknologisk professionalisme, men i større graf afhænger af de udøvende teams autonomi i organisationen. 

Derfor er nødvendigheden eksplicit omkring hvordan organisationens kommunikation flyder.

Dette er jeg personligt overbevist om er et must, selv for meget små organisationer.

Et system er en helhed sammensat af flere dele. Forstil dig vi ser systemet med udgangspunkt i software eller andre menneskeskabte ting. 

Hver af disse dele er designet, udviklet og bygget et eller andet sted i en organisation, og højst sandsynligt omkring et domæne, noget viden og erfaring.

Og hver eneste menneske som er involveret i denne process vil blive kognitivt påvirket ved at indgå i dets arbejde. 

> Gestalt refererer til en helhed bestående af flere enkelte dele. Helheden betragtes som større end summen af de enkelte dele, der danner gestalten. For eksempel er en rytme mere end summen af de enkelte slag, og en melodi er mere end summen af de enkelte toner. [Kilde](https://www.udviklingsterapi.dk/hvad-er-gestaltterapi/)

Selv en lille del i et system kan have stor betydning. Ofte bruger jeg en analogi omkring en motor. En motor er et system bestående af mindre helheder, som igen består af mindre dele.

![800px-Unit_injector_early_(cropped)](https://mataroa.blog/images/1964c345.jpeg)

Her er en brændstofsdysse. Det kan vi betragte som en helhed. Den udgør et facit som er brugbart hos andre helheder og givetvis i forbindelse med andre systemer. Den er samtidig opdelt og samlet af mindre dele.

![896437](https://mataroa.blog/images/e9d15442.png)

Motorens mange dele handler rent teknisk om at lade den indgå i samarbejdet med andre dele og sammen udgøre en enhed - en mindre enhed - men som samtidig *kan* spille en rolle i en større helhed af et system (en motor f.eks.). 

Men om helheden af en brændstofsdysse sidder i en diesel-generator, en græsslåmaskine eller en industri-ovn er ikke systemets ansvar at bestemme. Omverdenen må altså også beslutte hvor en helhed i deres eget system, giver mening - men de bestemme over helheden. 

![9079_-_photo_0_1657032161_big](https://mataroa.blog/images/a80716d5.jpeg)

> En af mine egne maskiner set fra siden; luftindtag, vandpumpe, starter-motor, manifold etc. Brændstofdysserne sidder forbundet på hver det som ligner omvendte "U'er".

Hver lille del samarbejder til at blive noget større, som potentielt igen bliver til noget endnu mere. 

- Del. Enhed. System.
- Del. Helhed. System.
- Endpoint. Service. System.
- Endpoint. Input/Ouput. System.

Men det er også her, vi kan begynde at betvivle vores hidtidige opfattelse af helheden af systemet som værende forkert. 

Hvis vi antager, at den enkelte del kun bør indgå i én systematisk sammenhæng, hvorfor skulle vi så ikke integrere delen fuldstændig i helheden? Hvad er fordelen ved at bygge noget som kan "leve" i en anden sammenhæng ? 

Se på motoren og overvej så at jeg ikke kan skifte vandpumpen (den lille bronze del nederest til venstre) uden at jeg også skal skifte hele blokken.

Vandpumpen suger vand ind. Og pumper vand videre. Det er ansvaret for den helhed. Det er dens facit. Hvordan resten af motoren bruger vandet er vandpumpen hverken interesseret eler ansvarlig for.

> Vandpumpen på billedet kan f.eks bruges på motorer af Nanni, Volvo Penta, Perkins og Mercedes.

Vi kan altså opfatte vores services som egne helheder. Det er selvfølgelig ikke et krav. Vi kan også betrage dem som værende interne dele som i sidste ende udgør en helhed.

Om helheden kan anvendes - ikke kun af ét system - men også i og af flere andre, er det som gør helheden i stand til at differentiere sig, og ikke have en nær relation/kobling til andet software.

Dette svarer dog ikke på spørgsmålet om, hvorfor microservices kan være et fordelagtigt design. Men det fører os måske lidt nærmere til erkendelsen af, at en del, som kan fungere som en helhed - se på brændstofsdyssen - kan udvikles uden at skulle forholde sig nært til hvordan andre helheder eller systemer. Og hvis man ikke skal forholde sig til det så er det mindre at fylde hovedet op med.

![Untitled (10)](https://mataroa.blog/images/8727f9ae.jpeg)

Herover er der dele som udgør enhender.

![Untitled (11)](https://mataroa.blog/images/d08968c7.jpeg)

Helhederne bliver måske en del af to forskellige systemer. 

Forestil dig det sådan, at hvis virksomheder, som fremstiller dæk til biler, også var nødt til at designe og udvikle aksler og undervogne for at få deres dæk til at passe på bilen, ville det så være fordelagtigt ? Næppe.

Det er ikke ligefrem en subjektiv antagelse jeg fremfører, at jo mindre kompleksitet mennesket behøver at forholde sig til, desto større er sandsynligheden for at ens hjerne har en bedre chance for danne overblik og tænke mere klart. 

Når en organisation beder sine ansatte om at bygge et system, sker der ofte det, at de ansatte starter med at forsøge at forstå helheden omkring systemet.

> The human mind is incredibly averse to uncertainty and ambiguity; from an early age, we respond to uncertainty or lack of clarity by spontaneously generating plausible explanations. What’s more, we hold on to these invented explanations as having intrinsic value of their own. Once we have them, we don’t like to let them go. [Kilde](https://www.newyorker.com/tech/annals-of-technology/why-we-need-answers)

I begyndelsen kan det muligvis være nemt nok, fordi systemet endnu ikke er stort nok til at skabe betydelige kognitive udfordringer for medlemmerne. Men hvis du har bygget software længe nok, i teams af mere end én person (og selv da er det svært, når tingene vokser), så opstår der pludselig alle mulige udfordringer, som faktisk har meget lidt at gøre med teknologi. Derimod meget at gøre om organisation, deraf kommunikationen også ganske enkelt psykologien hos mennesket.

![Untitled (13)](https://mataroa.blog/images/4e0985cf.jpeg)

> Kruglanski conceptualizes our need for cognitive closure as consisting of two major stages, seizing and freezing. In the first stage, we are driven by urgency, or the need to reach closure quickly: we “seize” whatever information we can, without necessarily taking the time to verify it as we otherwise would. In the second stage, we are driven by permanence, or the need to preserve that closure for as long as possible: we “freeze” our knowledge and do what we can to safeguard it. (So, for instance, we support policies or arguments that validate our initial view). And once we’ve frozen? Our confidence increases apace. [Kilde](https://www.newyorker.com/tech/annals-of-technology/why-we-need-answers)

Vi skal derfor designe systemer omkring, at en del eller en helhed bør forblive tilstrækkeligt lille, således at både systemet, organisationen, teamet og den enkelte person kan rumme det. Ja det er meget at bede om. 

Her kan vi, istedet for at betragte de største kasser som helheder for deres bærende dele, også betragte dem som værende teams, med et ansvar for helheden og dets bærende dele. Hver farvet kasse er nu et team der bygger det som farven har ansvar for.

![Untitled (10)](https://mataroa.blog/images/8727f9ae.jpeg)

Små helheder er ofte tæt knyttet til noget, der har større betydning end den enkelte helhed selv. 

Når man bygger noget, der skal kunne fungere afkoblet, men samtidig interagere med andre helheder og systemer, skal denne helhed altså kunne bidrage med mere end bare at være en helhed for sig selv. 

Jeg bliver gerne at sige, at det er den enkelte enhed som indeni sig selv har ansvaret for, at manipulere de data som er vigtig for enhedens jeg. Hvad der dernæst udstilles på den baggrund er op til modtageren at undersøge om er brugbart og nyttigt for sit dets eget jeg.

![Untitled (15)](https://mataroa.blog/images/31106be5.jpeg)

> intet der ikke er en intern del af helheden skal kunne manipulere med den.

Microservices skal give nogle forbedrede evner hos organisationen, hvoraf nogle af mulighederne er:

- reducere mængden af ting som der skal foholdes sig til
- minimere kommunikationen fordi ikke ikke er forankret samme sted
- øge helhedernes og systemernes transparens så det tydeliggør hvad og hvor noget er kompliceret/dyrt.

En anden evne er, at en del eller heldhed kan repræsentere en forretningsmæssig værdi eller mulighed. For eksempel på samme måde som vandpumpen, der kunne anvendes af mange forskellige motorer og ikke blot én.

Derfor, hvis vi ønsker at bygge mindre, for bla. at øge transparensen omkring kompleksiteten, forandre kommunikationsstrukturer og dele ansvaret op hvor sømningerne i domænet også splitter, så kan vi ikke længere organisere eller kommunikere på samme måde som hvis vi ville bygge systemer med ideen om, at alle dele skulle være forankret på samme sted.

Der følger trade-offs med til alle valg. Så uden først at acceptere at der naturligvis også følger trade-offs med i valget af et microservice design, så vil dit valg føre til nye frustrationer som blot vil være anderledes. De frustrationer vil højst sandsynligt komme fra organisationens forsøg på at adoptere nye måde at kommunikere på, eller dens umodenhed til samme. 

> Hvordan ser jeres kommunikationsstrømme ud ? Hvordan bygger I software på baggrund af den ? Tegn den op så i kan se hvordan strømningen er, men lad den være baseret på jeres eget data.

Personligt har jeg aldrig set en microservice arkitektur fungere, uden at der til grunds var dannet teams der ejede dele og små helheder, snarere end hele systemer eller store helheder. 

Jeg har set det være en rigtig modbydelig tilgang mere end en gang. Der førte til skyttegravskrig imellem teams, elfenbenstårnsarkitekter der valgte at prøve at gøre det forfra - bare i andre teams. At koblingen imellem helhederne var så "tight" men menneskerne så adskilt, at den asynkroni og det mandat der skulle gøre ansvaret mindre hos det enkelte team, istedet mangedoblede den og samtidig udpenslede organisationen som værende både umoden og til sidst perfid. 

Jeg ved godt det lyder vildt og dramatisk men det kan blive grimt når ens organisation faktisk viser sig patologisk, og ikke vil formår at tage ansvar for dets valg. 

Teams skal kunne operere inden for rammerne, trygheden og autonomien for, hvad deres del og mindre helhed gør det muligt for dem at tilbyde andre og andet. 

Hvis en organisation ikke formår at adskille disse begreber - dele, helheder, og systemer - både på et højere abstraktionsniveau og især i den praktiske udførelse, løber den en risiko for at blive overvældet af services, som reelt set kun er løst forbundet software centreret om teknologi frem for domæner, teams, og forretningsegenskaber.

Jeg har set interne service-registre i virksomhed, udviklet i hundredevis, som kunne alt fra at sende, emails, booke aftaler eller registere parkering. Problemet med den slags services er, at de ikke udgør en del af en større helhed som er forankret omkring et doæne i forretningen. De lever bare et passivt liv og ingen tager rigtig ansvar for dem af samme grund.

Jeg ville aldrig anbefale organisationer, der opererer ud fra traditionelle organisationsstrukturer, at forfølge en idé om at udvikle systemer baseret på microservices. Det kræver en vis modenhed fra ledelsens side - især omkring autonomi i de enkelte teams syntes at være meget svært at accepterer - men ofte også en teknologisk forandring blandt programmørernes tankevirksomhed, da de givetvis skal begynde at forholde sig til nogle andre begreber og tilstande, end de højst sandsynlig har været vant til.
