# Den triangulære organisation - en historie om et arkitekt og produkt teams organisatoriske forbandelse.

> Published on Jan 23, 2024

I forlængelse af [Switchboard Managers](https://zerfro.mataroa.blog/blog/switchboard-managers-et-fortidens-levn/), vil jeg forsøge at dele lidt ud af nogle erfaringer om diverse organisationsdesign.

Samtidigt, [posten om dedikerede teams, hvis rolle bliver en flaskehals for andre teams](https://zerfro.mataroa.blog/blog/side-effekten-af-et-dedikeret-clouddevopsplatform-team/), forklarer jeg noget omkring, hvordan teams skal have mandat og ansvar til selv at bestemme, hvordan de ønsker at behandle deres software, og hvordan forankringen af et kollaborativt samarbejde skal ske internt i teamet, og ikke designes omkring forlængelser hos andre teams.

Kort fortalt, så skal teams, der bygger software, om det er produkter, features eller eksperimenter, selv eje alt ansvar omkring den software. Og det skal de netop fordi, at når man bygger software, så er det vigtigste at få det afprøvet så hurtigt som muligt i hænderne på rigtige mennesker, om det man har bygget også er det, som forventningen er i den virkelige verden.

Den her historie og erfaring udspiller sig i en større dansk virksomhed. Konteksten er omkring et initiativ, som kommer direkte fra ledelsen og som handler om, at "virksomheden skal mere effektivt kunne bedømme likviditeten hos kunden, ved optagelse eller ændring af nye eller eksisterende engagementer".

### Eksternt produkt, internt produkt. To teams der ikke taler sammen.

Vi befinder os i et triangulært hierarki, hvor der eksisterer et dedikeret arkitekt-team (som dog også skriver kode), et produktteam (potentiel aftager af arkitekt-teamets produkt) og et øvre ledelseslag.

Hvert team har hver deres ledelse i toppen af hver deres trekant.

![org1](https://mataroa.blog/images/6c5f220a.png)

#### arkitekt teamet

- Arkitekt-teamet har fået til opgave at lave et internt produkt, som andre teams kan være aftager af.

- Det er arkitekt-teamet, som ud fra ledelsens idé har til opgave at udvikle og vedligeholde dette interne produkt.

- Der bruges mange måneder fra enkelte medlemmer i arkitekt-teamet på at tegne diagrammer, UML og andet, og trykteste diagrammerne hos andre arkitekter.

- Der udvikles software på baggrund af disse diagrammer, mundtlige overleveringer fra arkitekt-teamets ledelse, og der nedfældes ikke skriftlige krav, som ikke er af teknisk karakter.

- Når det viser sig, at udviklerne ikke forstår diagrammerne eller formår at løse opgaverne, fordi de hverken er nedfældede eller på anden måde formaliseret, bliver ledelsen involveret.

- På en hånd kan der tælles, i de første 6 måneder, hvor mange gange en bruger til softwaren er involveret i udviklingen. Og det er på et ledelsesniveau.

- Frustrationerne er store i arkitekt-teamet, både hos udviklerne og hos forretningens domæneeksperter, fordi diagrammerne ikke hænger sammen med virkeligheden, når først udviklingen er påbegyndt.

- Ledelsen i arkitekt-teamet kommer ind fra højre med påstanden om, at der ikke er blevet lyttet ordentligt efter ved overleveringerne. At X skal være Y, og at modellerne i V skal være som W.

- Der tilføres med spredt fægtning, og som månederne går uden at kunne fremføre et reelt outcome, flere mennesker til opgaven. Nogle forsvinder igen. Andre er der kun gradvist.

#### produkt teamet

Man fornemmer den næsten konstante frustration hos produkt-teamet, hvis opgave det er at bygge et kunde-vendt produkt, hvor en af søjlerne i fundamentet for den software er det værktøj, som arkitekt-teamet udvikler.

Det er svært at gengive produkt-teamets hovedtræk for mig, fordi jeg var ikke en del af det, og fordi det triangulære hierarki i organisationen ikke tilgodeså, supporterede eller var designet omkring en involvering fra enkelte medlemmer i arkitekt-teamet, så var det meget svært at drive en positiv forandring.

- "Vi får ikke det, vi skal bruge fra arkitekt-teamet, hvorfor?"

- "Hvis vi ikke kan få det, vi skal bruge, så bygger vi det selv."

- "Vi laver om i vores systemdesign, derfor skal arkitekt-teamet også kunne understøtte det systemdesign."

- Tovtrækkeri imellem de to ledelsesfunktioner i hhv. produkt og arkitekt-team.

### Ledelsen og den virkelige verden.

Når ledelsen får en idé, sker der ofte det, at den bliver tilgodeset af velmente kolleger og samarbejdspartnere. Jo længere en idé får lov til at flyde rundt i organisationen og blive genfortalt, jo mere forplanter den sig, og på et tidspunkt kan den være svær at skille sig af med igen.

Idéer kan være nok så gode, men de er intet værd, hvis deres hypoteser ikke bliver valideret, og gerne så hurtigt som muligt. I stedet gør man, som man altid har gjort - bygger først, sælger senere, smider væk, når man indser, at man tog fejl.

### Hvad skulle man have gjort ?

Det er altid svært for mig at skulle rådgive virksomheder om, hvad de skal gøre anderledes for at nå frem til et bedre udgangspunkt. Det kræver en betydelig mængde viden omkring den kontekst, et team eller en organisation befinder sig i, og derfor er det både naivt og egoistisk at tro, man kan forandre tilstanden, når ens job er at bidrage andetsteds.

Men det er ligeså meget en del af en organisationsdesign at kunne formidle og forandre tilstanden i den omstændighed, man befinder sig i. Det kan ikke lade sig gøre i en organisation, som er så forankret omkring topstyrede teams, hvor individer enten handler ud fra en leders ordre.

I dette tilfælde var der ingen tvivl om, hvordan den software skulle se ud; det var altsammen inde i hovedet på lederen. Og det blev ikke efterladt til hverken arkitekterne eller udviklerne i samme team at forandre det.

Når man ikke kan ændre ting, man er en aktiv del af, bliver man frustreret. Det er helt præcist det, som der ligger i ordet magtesløshed. Og det er hverken sundt for en medarbejder, et team eller en virksomhed. Det er som jeg skriver i [Switchboard Managers](https://zerfro.mataroa.blog/blog/switchboard-managers-et-fortidens-levn/), et levn.

Skulle jeg alligevel komme op med en løsning, så havde jeg:

- Samlet de to teams i ét team og rapporteret direkte til idé-ejeren.
- Havde bedt teamet lave en prototype af produktet, uden brugen af rigtig data, baseret på én vigtig forretningsmæssig egenskab (f.eks.: tegne ny forsikring).
- Valideret preto/prototypen overfor rigtige kunder.
- Præsenteret data og kunde-feedback til idé-ejeren.
- Afventet mulig omstilling på baggrund af den feedback.

![org3](https://mataroa.blog/images/922875a9.png)

(for en god ordens skyld vil jeg lige tilføje, at jeg aldrig nogensinde ville anbefale nogen som helst virksomhed, at bygge teams op omkring enkelte beslutningstagere eller ansvarsfratagere i andre teams. Billedet herover er blot et stilbillede af hvordan et enkelt teams skal kommunikere med ejeren af en idé. Og det er en god idé, fordi det får idé-ejeren til at forstå hvad det er han får tilbage fra det feedback loop som et eksperiment udløser. )

Lad os nu sige, at ovenstående ville have taget 3 mand 1 måned at lave, og så ville man pludselig have reel feedback fra rigtige kunder. Ens idé ville pludselig leve i den virkelige verden.

Derfra ville man have en valideret hypotese omkring den ene forretningsmæssige problemstilling. Den ville give indsigt i, om der skulle valideres yderligere, eller om teamet mente, det havde tro nok på data, til at gå i gang med at udvikle den ene ting, man havde valideret.

![validation1](https://mataroa.blog/images/b50766db.png)

På den måde ville man spare 12 måneders udviklingstid på en idé, som ingen kunne præsentere data for, om var god eller dårlig. Det er en stor chance at tage.

Hverken velviljen, troen på ledelsen, idéen, eksekveringen, trygheden i teamet, tilhørelsesforholdet eller gennemskueligheden vokser ved at lade sin tro på egne idéer triumfere. Det er bad for business hele vejen igennem.

At gennemtvinge en idé er altså et grundlæggende tegn på, hvordan en organisation er designet, hvor nogen mener, de er klogere end virkeligheden, og hvor andre ikke er hierakisk egnet til at modsige sig det eller forandre det.

Lad være med at spilde jeres tid på at tro på jeres idéer. Det er håbløst. Få dem valideret, og validér dem gerne af flere omgange, så I trygt kan komme videre når den viser sig enten at holde vand, eller bedst egner sig til arkivet.
