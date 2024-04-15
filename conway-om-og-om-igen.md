[//]: # "title: Conway om og om igen."
[//]: # "slug: conway-om-og-om-igen"
[//]: # "pubDate: 18/12/2021 12:01"
[//]: # "lastModified: 14/12/2023 13:07"
[//]: # "excerpt: This is really interesting stuff!!!"
[//]: # "categories: code, acr, azure, easypeasy, iaas"
[//]: # "isPublished: true"


**Conways Lov: Magten I Den Organisatoriske Struktur**

Conways Lov, opkaldt efter Melvin Conway og hans veldkendte pointer fra artiklen ['How does Committees Invent'](https://www.melconway.com/Home/pdf/committees.pdf) fra 1968, fastslår, at "organizations which design systems (in the broad sense used here) are constrained to produce designs which are copies of the communication structures of these organizations". Altså; Vores systemer er en kopi af vores kommunikationstrukturer.

Nu er Conways Lov ikke møntet på softwareudvikling, den fortæller dig blot hvordan systemer bliver designet på baggrund af hvordan kommunikationen flyder I en organisation. 

Derfor kan vi heller ikke sige, at Conways Lov er god eller dårlig. Den er der blot.

Conways Lov udstiller måske nok hvordan en organisationer er mangelfulde eller dårlige til, at kommunikere, grundet netop deres organisations-design. 

Derfor eksisterer der og der eksisteret meget snak om Conways Lov I og omkring softwareudvikling. Fordi loven virkelig har formået at udstille - hvad jeg vil kalde - et ledelsproblem der igen udstiller udfordringen omkring hvordan organisering software til stadighed og fejlagtigt betragtes som kan tilpasses et industrielt organiserings-blueprint/design.

Derfor har Conways Lov dybtgående konsekvenser for softwareudvikling. Særligt omkring arkitekturen/strukturen/designet, da loven netop antyder, at disse ofte dikteres af den organisatoriske kommunikationsstruktur omkring og inde i det team, der udvikler systemet.

Et glimrende eksempel er to teams som laver overlappende APIer fordi ingen i organisationen har designet de to teams mandat eller ansvarsområder. Selvom de to måske teams kommunikerer vil der alligevel være en chance for, at ledelsen stadig beder om det overlappende funktionalitet, fordi ledelsen måske ikke kommunikere eller ikke interesserer sig for udfordringen.

Eric Steven Raymonds - bla. forfatteren af [The Cathedral & the Bazaar](https://www.amazon.com/Cathedral-Bazaar-Musings-Accidental-Revolutionary/dp/0596001088) gav muligvis det mest original eksempel "If you have four groups working on a compiler, you'll get a 4-pass compiler." (Til jer der ikke kender til compilers, så er "pass" typen/metoden hvorpå en compiler travesere kildeteksten af program.)

Disse udfordringer har vi været eksponeret overfor I organisationer som særligt er forankret omkring det  industriel hierakiske organisations-design hvor bla. vigtigheden af "chain of command" og "hold kommunikationslinjerne" omkring organisations-diagrammet særligt diktereres.

**Conways Lov i Software Praksis**

**På Mikroniveau (egentligt lidt svær at forklare)**

Conways Lov kan opleves på mikroniveau, hvor teams, hvis medlemmer besidder forskellige tekniske kompetencer, vil lave software og systemer der netop svarer til disse kompetencer. Det skyldes at kommunikationen internt på teamet vil være funderet omkring individernes egne kompetencer, og deraf vil dette medføre at systemet designs på den baggrund. En frontender vil kommunikere sin "superkræft" og brugbare egenskab i systemet som værende frontend, også selvom det måske ikke egner sig i systemet her og nu.

**På Makroniveau**

Conways Lov kan også ses på makroniveau, hvor hele virksomheder er begrænset af de legacy-systemer, de har udviklet over tid. De her legacy-systemer kan blive så komplekse og indviklede, at de kræver specialiserede teams til at vedligeholde dem, hvilket så yderligere forstærker den organisatoriske struktur, som førte til kompleksitet og indvinklet i første omgang.

**Homomorfi : Matematiken, der Bevarer Systemstrukturen**

Hos det bagvedliggende i Conways Lov finder vi bla. den homomorfiske matematiske formel eller matematiske koncept, der antyder, at strukturen af et system har tendens til at bestå, også selvom systemet udvikler sig. Dette koncept kan derfor føre til ["Path dependency"](https://en.wikipedia.org/wiki/Path_dependence), hvor organisationer bliver låst fast i forældede systemer og kommunikationsmønstre.

**Ledernes Rolle i at Navigere Conways Lov**

Givet udbredelsen af Conways Lov spiller dygtige ledere en afgørende rolle i at afbøde dens negative effekter og designe mere effektive og funktionelle teams. Her er nogle af de overvejelser jeg mener er fuldstændig nødvendige for de mennesker som har mandat til, at designe teams.

* **Start småt og underbemand:** Undgå at træffe arkitektoniske beslutninger baseret på hverken teamets størrelse eller fremtidsønsker. Start i stedet med et lille, fokuseret team og tilføj gradvist ekspertise efter behov. Hvad er et lille team ? Én person er ikke et team. 2 mennesker kan komme rigtig langt. 3 og 4 kan komme længere. 6 er i min bog maksimalt, og nok for stort til at jeg synes det er småt. 2, 3, 4 eller 5 er bedst hvis du spørger mig.

* **Brug generalister:** Opfordre teamets medlemmer til at have et bredere repertoire af færdigheder snarere end en specifik specialisering omkring et enkelt eller et par områder. Dette vil hjælpe med at nedbryde siloer og fremme et mere fleksibel og tilpasset team.

* **Udfordre antagelser:** Udfordre regelmæssigt de antagelser, der ligger til grund for systemets design, især dem, der måtte være forankret i organisationens kommunikationsstruktur (Hvis i desværre havner der, så har organisationen nok et tag I jer som i skal finde en vej ud af.)

* **Brug prototyping:** Lav prototyper tidligt og ofte for at validere antagelser og indsamle feedback, netop istedet for, at bygge på antagelser og subjektive anekdoter som ingen kan validere. Stol på ingenting med mindre du kan måle det.

* **Udnyt Conways Lov til jeres fordel:** Anerkend, at Conways Lov kan være en nyttig pegepind til at forme et teams muligheder for, at udvikle de bedst mulige løsninger. Gå så langt som din integritet kan holde til for at opnå den autonomi og det mandat der skal til i et team, for at teamet kan levere målbar forretningsværdi. Spørg gerne "Findes der en bedre løsning for os, som ikke er tilgængelig, pga. vores organisation".

Conways Lov giver en værdifuld indsigt til at forstå forholdet mellem organisatorisk struktur og løsningerne der kommer ud af samme. Ved at anerkende og adressere de udfordringer, som Conways Lov opstiller, kan teams skabe mere effektive, fleksible og tilpasningsdygtige softwaresystemer, der tjener både organisationens og dens brugeres behov.
