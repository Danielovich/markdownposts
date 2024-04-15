[//]: # "title: Side-effekten af et dedikeret cloud/devops/platform team."
[//]: # "slug: side-effekten-af-et-dedikeret-clouddevopsplatform-team"
[//]: # "pubDate: 18/1/2024 12:01"
[//]: # "lastModified: 19/1/2024 13:07"
[//]: # "excerpt: "
[//]: # "categories: organisation"
[//]: # "isPublished: true"


- software/produkt/feature/* team,
- cloud/devops/platform/* team,
    - et team, som kontrollerer og er ansvarshavende overfor ovenstående teams ifbm. infrastruktur, pipelines, servere, databaser, netværk, monitorering, alerts, disk, etc.

I forbindelse med, at cloud providers, som AWS og Windows Azure, de sidste 10-12 år er blevet evangeliseret af Amazon, Microsoft og communitiet, og siden adopteret af virksomheder, er forankringen af disse i selve virksomhederne ikke meget anderledes, end hvordan man hverken anskuede eller designede sin organisation omkring de tidligere kendte "admin"/"infra" teams.

I dag kalder man det bare noget andet, fordi alle ved, at *buzzwords skaber forandring*, som gør ens teams i stand til at flytte sig i et mere bæredygtigt tempo.

Udfordringen er stadig kommunikation på tværs af teams, som hæmmer muligheden for, at lade de enkelte udviklingsteams bestemme og kunne tage ansvar for, hvad og hvordan de selv ønsker, at bruge det, de finder relevant for det, de bygger. Altså, det udstiller lynhurtigt, hvordan organisationen er designet omkring, hvor beslutninger og kontrol er placeret - command and control.

Fordelen ved, at lade de enkelte teams have dette mandat, siger sig selv. De bliver ikke blokeret af idle-time fra andre teams. De lærer og forankrer, hos dem selv, hvad det vil sige, at administrere den software, de bygger. Og det skaber ydermere et større tilhørelsesforhold hos teamet.

At have et dedikeret team til at styre eller forankre en platform, er en løsning, som bør leve i et absolut minimum antal måneder. Og deres fornemmeste rolle er absolut ikke at skabe flere abstraktioner, men at gøre software- og produktteams i stand til selv at kunne varetage den eller de cloud platforme, som man har valgt, at understøtte i virksomheden.

Hvad tror du, der sker, når 3 forskellige teams uafhængigt af hinanden beder det såkaldte devops/cloud-team om at få adgang til, at oprette noget infrastruktur?

En cloud-provider vil altid out-performe ens egne cloud-teams, så i stedet for, at bygge abstraktioner ovenpå, så skab en lige linje til cloud-provideren.

Vi ser den her konstellation mange steder stadigvæk. Og det fremmer ikke noget som helst godt hos teams, der er sat i verden for at skabe resultater i et så bæredygtigt tempo som muligt.

Men der er ingen gode argumenter for at lade være med, at give enkelte teams det ansvar og mandat til selv, at kunne styre sine behov direkte imod en cloud-provider. Det handler udelukkende om at kunne kontrollere og bestemme, hvor beslutningerne træffes, og det indgyder mere et strejf af positionering af magt mere end forankring af ansvar, mandat, tryghed og et devops mindset.

Sådan her ser det altså ud, når man laver et cloud team, som er flaskehals for andre teams, og som er fuldstændig identisk med tidligere infra/admin/* teams, vi kender fra de sidste 25 år.

![wrong1](https://mataroa.blog/images/4ce96097.jpeg)

Minder det til forveksling f.eks. om det samme design, som når man har et dedikeret DBA team?

![dba](https://mataroa.blog/images/1e9e3cb8.jpeg)

Det her design har intet at gøre med, at lade ens software og produkt teams komme så bæredygtigt som muligt, fra A til B. Det handler udelukkende om kontrol og hvor beslutningerne træffes.

Så tror jeg, du har forstået det. Hvordan kan det ellers se ud?

Hvis vi nu lige kan etablere konsensus omkring, at det, som det handler om for at skabe resultater med software, som minimum, må være, at et team kan iterere over, hvad de har lært siden sidst, deres software blev udgivet. En stor del af mantraet i agilitet handler om feedback loops fra de mennesker, som bruger ens software. Så hvis ens team ikke har mulighed for, at udgive sin software, som de finder mest bæredygtigt, så har man fjernet fundamentet for, at lade dem lære nye ting fra de feedback loops.

Derfor må vi lave en antagelse om, at teams skal kunne udgive deres software, som det passer dem, også uanfægtet af, om de både skal bruge den ene eller den anden funktionalitet hos en cloud-provider. Og derfor er det nødvendigt for dem, at kunne kontrollere det setup selv. Og hvis et internt cloud/devops/platform/* team ikke tilbyder - og de kan ikke tilbyde det samme som en cloud provider - det, så piller man ved det mandat.

Heldigvis findes der virksomheder, som stoler på, at deres medarbejdere både kan selv og som stoler på, at de tager ansvaret seriøst. Hvor kontrollen er sluppet, fordi man har forstået for længe siden, at kontrollen også skal administreres.

De teams, der kan selv, er ofte gladere, netop fordi kulturen omkring dem er fri for kontrol og styring af beslutninger. Man stoler på dem, fordi de er professionelle, voksne mennesker, som er dygtige til deres arbejde. Hvor man finder de mennesker, er en anden historie.

![cloudcorrect](https://mataroa.blog/images/ab2ca272.jpeg)

Jeg har skrevet nogle tekniske ting her, det behøver du ikke bide for meget mærke i. Du skal derimod bide mærke i, at hvert team uafhængigt af et andet team kan kommunikere direkte med en given cloud provider. Og det gør de igennem det toolset, som den gældende provider stiller til rådighed, og ikke igennem et internt team.

Ting sker naturligvis ikke af sig selv, så derfor er en ikke fremført heuristik på tegningen, iblandt de forskellige teams, at der naturligvis skal eksistere en egenskab til at kunne kommunikere med hinanden. Ikke indblanding, kontrol eller bestemmelse. Spørgsmål og svar om, hvordan en udfordring kan løses fornuftigt i et enkelt team eller på tværs af flere teams ifbm. med den givne cloud provider.

Her findes der ingen teams, som har ansvar for, at andre teams kan komme videre med deres eget foretagende. Der er heller ingen teams, som bestemmer hvordan andre teams skal bruge en platform eller hvordan platformen skal se ud. Det enkelte team bestemmer fuldstændig selv, hvordan den udfordring, de står med, ift. den software, de bygger, skal løses og hvordan cloud-platformen kan afhjælpe den udfordring.

Vi har oplevet af og til, at der eksisterer en nervøsitet og bekymring omkring "cost". Omvendt har vi også erfaret, at netop enkelte software teams har en langt bedre mulighed for, at kunne svare på, hvad deres forbrug er, fordi de i modsætning til et eksternt silo cloud team, ved præcis, hvad de skal bruge og hvad de kan lukke ned for. Det ved et cloud team ikke på samme måde, fordi de ofte partitionerer ressourcer på baggrund af flere teams, og derfor bliver cost uigennemsigtig og svær at styre.

Facit er altså stadigvæk, at når man bygger software, så er det vigtigste, at få det afprøvet så hurtigt som muligt, i hænderne på rigtige mennesker. For at kunne nå derhen, skal teamet have mulighed for at udgive det sammen med dets afhængigheder. Teamet skal selv kunne bestemme, hvordan det gøres og uafhængigt af indblanding, kontrol og bestemmelse fra andre teams.
