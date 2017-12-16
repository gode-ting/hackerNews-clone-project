# LSD Exam Report – Group C

## Introduktion

## 1 Requirements, architecture, design and process**

Denne sektion beskriver projektets krav, den valgte arkitektur, design af komponenter og processen bag tilblivelse af disse.

### 1.1. System requirements

Opgaven var at lave en kopi af [Hacker News](https://en.wikipedia.org/wiki/Hacker\_News). Vi skulle ud fra en kort beskrivende tekst af hjemmesiden, udforme vores egen kravspecifikation til systemet. Kravene kan opdeles i funktionelle og ikke funktionelle krav.

#### Funktionelle krav

- Man skal kunne oprette en bruger
- Man skal kunne logge ind
- Man skal kunne logge ud
- Man skal kunne få vist en liste af alle posts
- Kunne poste nyheder
- Kunne kommentere på nyheder
- Kommentere på andres kommentarer
- Man kan først upvote/downvote når man har opnået minimum 50 karma point
- Man skal kunne modtage/miste karma point
- Kunne upvote en nyhed
- Kunne downvote en nyhed

#### Ikke funktionelle krav

- Systemet skal have en oppetid på mere end 95%
- Systemet skal være konsistent. Det må ikke &#39;tabe&#39; noget indhold, der sendes til det fra simulator programmet. Det vil sige, selvom dit system f.eks. er nede grundet opgradering, bør der være en mekanisme, der buffer indgående indhold, som kan offentliggøres til systemet, når det er i drift igen
- Prisen for drift og vedligeholdelse må ikke overstige $50 pr. måned
- Programmet skal kunne håndtere flere 100.000 requests dagligt.
- Backenden skal udvikles i Java med brug af Spring Boot frameworket
- Kommunikationen imellem frontend og backend skal ske over http-protokollen

Her ses et use case diagram, der beskriver vores system:
<p align="center"> 
<img src="https://github.com/gode-ting/hackerNews-clone-project/blob/master/handover-docs/Complete%20Usecase%20Diagram.png">
</p>

Der er flere succeskriterier der skal opfyldes før at webapplikationen kan kaldes en succes. Webapplikationen skal leve op til både de funktionelle og non-funktionelle krav. Dertil skal der være en brugervenlighed hvorpå man ikke er i tvivl om, hvordan webapplikationen bruges. Et af kravene er f.eks. &quot; your system has to have an uptime of more than 95% &quot;. Dette ville være et succeskriterie, da der forventes, at man skal kunne oprette nyheder og kommentere nyhederne via REST API. Hvis systemet ikke har en stabil oppetid vil det resultere i, at man ikke kan tilgå REST APIet. Systemet vil gøre det let for brugere at kunne oprette nyheder og få tilhørende feedback fra andre brugere.

Til refleksion har vi i gruppen snakket om, hvorvidt vi synes at hjemmesiden er færdig eller ej. Vi er enige om at vores program, er det man kalder et minimum viable product (MVP) og derfor kun lige lever op til opgavebeskrivelsen af systemet. Det betyder at vores system mangler nogle basale features, før det ville være anstændigt at sætte det i produktion. Det er en beslutning taget helt fra starten, og vi føler derfor alligevel at vi er noget i mål med opgaven.

### **1.2. Development process**

In this part you should show off by telling us all you know about software development processes and describe which concepts you used to structure your development.

I forhold til dette projekt har vi kigget på 3 forskellige softwareudviklingsmetoder nemlig Scrum, UP og XP.

UP er et framework, hvor der er mulighed for tilpasse processen til mange forskelligartet software ved hjælp af dens principper.

**UPs principper:**

Iterative and incremental – Udviklingen er opdelt i små iterationer, som kan varer op til flere uger. Udfaldet af hver iteration er gennemtestet, integreret og er funktionelt.

En iteration repræsenterer alle punkterne i udviklingen af softwaren, det vil sige, en iteration indeholder requirements, analysis, design, implementation og testing.

Incremental betyder at softwaren vokser trinvist over tid.

Use-case driven – Der bruges use-cases til at finde alle de funktionaliteter som softwaren skal indeholde. Use-cases beskriver interaktionen mellem forskellige dele af softwaren, og selve softwaren, fra en brugers point of view.

Risk driven – Hvert use-case er rangeret ud fra nogle forskellige kriterier. Der er tale om architectural significance, business value og high risk. De alle indgår i en risikovurdering, hvor man tager dem med højest score først.

Architecture centric – Arkitekturen er central for at opnå et sikkert slutprodukt, derfor er det vigtigt at have en solid arkitektur tidligt i forløbet.

Ydermere, er UP overordnet opdelt i faser:

**Inception** – Formålet her, er at få et overblik over kravene til softwaren. Relativ kort fase.

**Elaboration** – Fokus på at udvikle de centrale dele i softwaren, yderligere fokus på at sætte sig og få en bedre forståelse af kravene til softwaren. Dette er en lidt længere fase.

**Construction** – Formålet her, er at få udviklet det resterede til softwaren.

**Transition** – Færdiggørelse og overdragelse.

**Extreme Programming** har værdier som kan tilnærmelsesvis findes i SCRUMs iterationer. XP har nogle foreskrevne tekniske praksisser, som kunne være interessante i forhold til projektet. En af praksisserne er Test Driven Development (TDD). I XP skriver man unit tests, som fejler, hvorefter man skriver det kode, der kræver at testen bliver grøn. Det hjælper en, som udvikler, til at overveje de forskellige komponenter og fremgangsmåder til udviklingen af komponenterne. XP har også en praksis som pair programming, hvor to personer sidder og udvikler ved samme computer. Dette øger produktiviteten. En anden praksis er fokusset på refaktorering til for at opretholde kode kvaliteten.
XP har den fordel at man konstant har et produkt som virker. Alle testene man skriver hjælper til at undgå fejl, der kan spare tid i længden.

**Scrum** har den fordel, ligesom XP, at man kan ændre kravspecifikationerne undervejs. Vi har i forhold til dette projekt valgt Scrum. Grunden til dette valg er, at Hacker News klonen ikke er færdigdefineret. Der er stadig mulighed for, at der kan komme tilføjelser undervejs, hvorfor denne agile tilgang er nødvendigt. Scrum kan kombineres med XP, da Scrum har øget fokus på projektledelse, end udviklingsmæssige praksisser, som XP har. Vi har derfor prøvet at lave en kombination af disse metodologier på bedst mulige vis. XP praksisser som at opretholde en kode standard og refaktorering kan være nyttige.

**Github issues.**

Gennem projektet har vi benyttet Githubs issue feature. Featuren gør det muligt, at oprette diverse issues på sit repository, som kan benyttes til at tracke fx implementationen af en ny feature, eller en fejlrettelse. På denne måde kan vi på en nem og overskuelig måde holde styr på vores tasks, og følge deres udvikling samt hvem der arbejder på hvad. Dette er med til at skabe overblik, og hjælper os med at strukturerer arbejdet.

Waffle.

Til at gøre vores issue håndtering endnu nemmere, har vi gjort brug af Waffle board.
Waffle integrerer med github, og minder lidt om et scrumboard. Formålet er nemt at kunne styre sine github issues, og strukturere dem bedre end hvad man kan på github.

 ![](https://github.com/gode-ting/hackerNews-clone-project/blob/master/exam-report/waffle-board.png)

Continous Integration(CI) har været en stor hjælp til at sikre os, at den kode som bliver kørt lokalt også virker når det bliver smidt op på vores GitHub repository. Vi har dertil udvidet processen lidt og benyttet os af Pull Requests hver gang vi havde en stor ændring til projektet. Her kom Travis frem og fortalte hvis der ville opstå problemer hvis Pull Requestet blev godtaget. Det har hjulpet en del, i forhold til Merge Conflicts.

#### Continuous integration.

Vi har igennem projektet benyttet os af continuous integration praksissen. Continuous integration går ud på, at udviklerne sammenfletter (merger) sine ændringer med en delt (shared) hovedline (mainline) flere gange om dagen.

Vi valgte at integrere continuous integration i vores projekt for:

- At træne vores forståelse for praksissen, og samtidig øve os i, at arbejde med et stort projekt, hvor denne praksis indgår. På den måde vil vi være bedre rustet til at arbejde på lignende måder i fremtiden.
- Forenkle processen hvor vi arbejdede flere i samme projekt samtidig. Når vi med continuous integration ofte merger vores ændringer, undgår vi merge konflikter og lignende, som vi derved ikke spilder tid på.
- Simulere et &quot;real world&quot; projekt, hvor vi ofte kunne rette bugs og tilføje features til systemet, så snart koden var klar.
Derudover kunne vi tilføje features på både front- og backend sideløbende, hvor frontenden afhang af backenden, da vi pushede nye features på backenden til produktion, mere eller mindre så snart de nye features var færdige.

### 1.3. Software architecture

**Systemet** har tre hovedkomponenter, en backend, en frontend og en database. Vores frontend kommunikere med vores backend via et API, og vores backend persisterer og henter information til og fra vores database. Et færdigt klasse diagram kan ses herunder: ![her](https://github.com/gode-ting/hackerNews-clone-project/blob/master/handover-docs/Complete%20Class%20Diagram.png)

**Backenden** er en RESTful web service skrevet i Java, hvor vi benytter os af Spring Boot frameworket.

**Backenden** består af controllers, models og repositories. Vi har to forskellige models, en til de bruger der har oprettet en account, og en til Posts som brugere opretter. Hver model har sit eget repository, og sine egne controllers. Controllerne sørge for at kalde vores repositories udfra de request der kommer fra vores frontend, og vores repositories sørger så for at persisterer og hente den information, den får fra de givne controllers.

**Frontenden** er lavet som en server-rendered side, som er lavet med node.js og er skrevet i javascript. For at lave en server-rendered side i node.js, skal man bruge et modul/framework. Vi har valgt at bruge express.js1, som er et af de mere kendte moduler til dette formål.  For at express kan vise content hos klienten, skal det have en template engine, som omdannes til html. I teorien kunne man bare benytte ren html, men ved at benytte en template engine, får man en række øgede funktionaliteter som gør, at det både er nemmere og hurtigere at skrive. I vores projekt har vi brugt pug.js2. Frontenden er bygget op omkring en hoved javascript fil (src/app.js), som er den fil opsætter og håndterer request til siden. Det er her, at de forskellige routes håndteres og middleware indlæses. Routes opsættes som eksterne filer, som står for at håndtere de pågældende requests til et givent url, og derudfra rendere et specifikt html content hos klienten.


### 1.4. Software design

Det første problem vi stødte på, dukkede op lige efter vi havde fået styr på hvad systemets requirements var, og hvad en bruger skulle kunne. Problemet lød på hvordan vi skulle designe vores model udfra de entiteter vi havde noteret ned fra vores requirements. Vi havde entiteter som en story og en comment, og ikke nok med det, så kunne en story være forskellige ting så som et simpelt link, eller en discussion. Vi vidste at alle de forrige nævnte entiteter havde nogle ting til fælles f.eks. det at man skulle kunne upvote og downvote dem. derfor ville det jo give mening at have dem alle under det samme objekt. men på den anden side ville det også give redundans i vores kode og vores database, fordi de forskellige entiteter ikke benytter sig af alle variabler. Vi blev enig om at vi sagtens kunne håndtere redundans, hvorimod den anden løsning ville kræve flere håndteringer og derfor mere arbejde.

Vi løb hurtigt ind i endnu et problem på grund af entiteterne, og problemet lød på hvilken slags database vi skulle vælge. Både SQL og NoSQL databaser ville kunne løse problemet fint, på hver deres måde. VI valgte at gå med NoSQL, da disse databaser ikke bruger skemaer og dermed er rigtigt gode til at modellere dokumenter, som kan kan variere i indhold.

En graph database ville have været et fint valg, på grund af de mange relationer mellem vores entiteter, og hvordan de er kædet sammen f.eks. comments på comments osv. Men selvom at en graph database var et godt valg, endte vi med at bruge en document based database. Det valgte fordi at et entitet-set med opslag og kommentare passer perfekt til documents based databaser til den struktur. Fordi hvert document ville indeholde dets children documents, ville vi nemt kunne hente al informationen fra en given post uden problemer.

Udover at en document based database var et godt valg på grund af strukturen på entiteterne, var det også et godt valg, fordi vi jo havde valgt at kommentare og de forskellige typer opslag skulle gemmes som det samme slags objekt, og der er det jo en fordel af document based databaser er skemaløse.

### 1.5. Software implementation

Vi har haft fokus på at følge alle de funktionelle krav, som vi også har kunnet realisere under implementationen.
Den proces som vi havde sat os for at følge, var en kombination af Scrum og XP. Den primære årsag var, at vi ikke kunne forudse, hvis der skulle foretages ændringer undervejs i udviklingsprocessen og derfor måtte gardere os mod dette. På papiret var det hele tilrettelagt, men der har været huller i forhold til at realisere dette 100%. Vi prøvede i starten at køre sprints, men endte ud i at køre det meste ad-hoc.

Vores software design har vi kunnet realisere og benytte fra start til slut. 

Der er har været problemer undervejs som har gjort, at vi har været nødsaget til at foretage ændringer. Vi har altid arbejdet med mindre systemer, hvor der ikke har været større datamængder. I dette projekt bliver der hver dag sendt data, i form af posts, til vores system som skal persisteres. Det hober sig op, og hver gang man skal hente information ud fra databasen, vil operationen, af naturlige årsager, tage længere tid. Hver gang system endpointet &quot;/latest&quot; bliver kaldt, tager vores kode alle posts, som er sendt fra simulatoren, sorterer dem, og tager det første elements hanesstID. Det er mange elementer som den skal query hver gang, og det kræver hukommelse. Det endte ud i en Out of Memory error, og vi var nødsaget til at refakturere vores kode, således, at den bedre kunne håndtere dette.

Vi har ligeledes været ude for angreb, hvor vi var nødsaget til at lave IP-restriktions. Ydermere, et NoSQL angreb, hvor vi havde forsømt at sanitize vores inputfelter.

## 2. Maintenance and SLA status

Denne sektion beskriver vedligeholdelse af systemet. Perioden strækker sig fra hand-over til vi slukker for systemet. Sektionen er skrevet ud fra et operatør synspunkt.

### 2.1. Hand-over

Kvaliteten af dokumentation var fin i forhold til de informationer der skulle bruges til at kunne overvåge deres system. 

Der var links til de forskellige komponenter/systemer, en guide til hvordan de ønskede at vi angav fejl. Til slut var der en oversigt over deres REST-API med fokus på post og comments. Alt i alt, følte vi os klædt på til at kunne vedligeholde systemet.

### 2.2. Service-level agreement

_Nedenstående metrics dannede grundlag for SLA._

**Uptime**

The uptime of the running application must not go below 98.3 %

**Mean Response Time**

The mean response time for all http requests must not be above 500 ms

**Mean Time to Recover**

Must not be above 4 hours

**Errors/Time**

Must not be above 10 errors per hour

Vi snakkede om hvordan man kunne måle en god service dvs. i dette tilfælde, systemets pålidelighed.

**Uptime** er helt klart med til at styrke systemets pålidelighed. Hvis systemetet er nede, er der ikke nogen der kan tilgå det giver det ingen trafik. Der er ingen der vil bruge et system, hvis det er konstant nede.
En uptime på 98.3% passer også godt i overensstemmelse med det overordnede systemkrav på 95%.

**Mean Response Time
&quot; **http requests must not be above 500 ms**&quot;**
Dette er af en svær størrelse. Det er blandt andet grundet, at det skal klart defineres hvilke http requests, som det omhandler. De har i deres dokumentation skrevet, at man kan få posts ud på følgende måde:
**&#39;/posts/:skip/:limit&#39;**

Man kan lave et request &#39;/posts/0/10000&#39; og response tiden vil klart overstige de 500ms. I hvert fald bidrage til, at gennemsnitstiden ikke bliver holdt under de 500ms.

 ![](https://github.com/gode-ting/hackerNews-clone-project/blob/master/exam-report/postman.png)

**Mean Time to Recover
&quot; **Must not be above 4 hours**&quot;.**  Denne hører sammen med uptime og er med af samme grund. Denne er helt klart med til at styrke systemets pålidelighed.

**Errors/Time
&quot; **Must not be above 10 errors per hour**&quot;** talte en smule om, at dette også var af en svær størrelse. Der er forskel på fejl, nogle er kritiske, nogle påvirker brugeroplevelsen og andre kan være på grænsen til harmløse.
Der blev derfor lagt vægt på, at det omhandler de kritiske fejl, som kunne påvirke brugeroplevelsen negativt.

### 2.3. Maintenance and reliability

_Dette afsnit indeholder en beskrivelse af hvordan vi oplevede at monitorere deres system._
Vi prøvede dagligt at tage et kig på deres metrics. I deres hand-over dokumenter beskrev de, hvordan de ønskede at man skulle melde et issue. Målet var, at det var lettere at adressere et issue, hvis der var givet et label med.

I forbindelse med aflevering 8, nærmere afleveringen omhandlende security, havde vi til opgave at finde potentielle sårbarheder i deres system. Vi benyttede selv MongoDB og kendte til deres manglede authentication ved opstart. Vi testede derfor deres system op i mod det udsagn. Vi kunne tilgå deres database og havde adgang til al deres data. Vi fandt ydermere ud af, at de gemte passwords for brugere af websitet i plain text. Det er et sikkerhedsproblem. Alt dataen er værdiløs testdata, men det er uden tvivl et bevis på, at hvis de kom ud for et angreb, vil de være i store problemer. Gruppen blev underrettet angående dette. Vi forsømte desværre  at tage billeder eller anden dokumentation.
Gruppen fik fikset dette, og reducerede kraftigt muligheden for, at SLA kunne blive brudt.
Deres system blev også mere pålideligt.

**Hvor pålideligt var deres system?**

 ![](https://github.com/gode-ting/hackerNews-clone-project/blob/master/exam-report/system-graph.png)
Tager man et kig på Helges graf, som viser antallet af errors, som simulatoren modtager, kan man se, at ud af næsten 1 million requests, er der 751032 responses, som har været en connection error. Det har haft betydning for systemets oppetid og dermed også overholdelse af SLA.

## 3. Discussion

### 3.1. Technical discussion

_Denne del er en opsummering af de foregående emner samt en oversigt af gode og mindre gode ting ved dette semesterprojekt._

**Opsummering**

Overordnet set kan man opdele projektet i 2 afsnit. Første afsnit, er en beskrivelse af hvordan vi udviklede hackernews hjemmesiden. Vi havde fokus på UP (Unified Process) principperne Inception, elaboration, construction og transition. Formålet med første afsnit var at beskrive vores proces fra idéudvikling til deployment af en færdig hjemmeside, som senere hen skulle overvåges og vedligeholdes af en anden gruppe.

Kravene til hjemmesiden kan opdeles i funktionelle og ikke funktionelle krav. De funktionelle krav beskriver funktionaliteten af hjemmesiden set fra brugerens point of view - eller med andre ord beskriver vores use cases. De ikke-funktionelle krav beskriver særlige forhold som systemet skal overholde så som teknologivalg, skalérbarhed, vedligeholdelse. pålidelighed, SLA (service level agreement) mm. De ikke funktionelle krav beskriver derfor hvad vores system skal leve op til, således at en anden gruppe kan overvåge vores system.

Afsnit 2 beskriver vores erfaringer med at overvåge et andet system end vores eget og kommer bl.a. ind på Handover dokument, SLA (service level agreement) og hvordan vores oplevelse med overvågningen var.

**Gode &amp; mindre gode ting ved projektet**

Alt i alt er kravene til projektet enormt ambitiøse, og det har været en lærerig oplevelse undervejs. Vi er alle blevet meget klogere på, hvorfor det er vigtigt med monitorering af store systemer, og endnu bedre har vi nu fået erfaring som vi kan gå ud og bruge i praksis. Men vi står nok tilbage med en følelse af, at flere af vores implementationer enten slet ikke er lavet, eller er kun er delvist lavet grundet manglende tid til fordybelse. Det er svært at forholde sig negativt til et projekt, der forsøger at være en tand for ambitiøst, da hensigten jo er at lære os så meget som muligt. Men sandheden er bare, at der er en naturlig limit på hvor meget viden der kan presses ind over kortere tid.

### 3.2. Group work reflection &amp; Lessons learned

Dette afsnit indeholder en kort refleksion over 3 mest vigtigste ting i forhold til hvad vi har lært igennem projektet.
Den første ting vi har lært er forskellige _do&#39;s and don&#39;t&#39;s_ i forhold til at arbejde med store systemer.Projektet har været med til at give indsigt i hvor hurtigt ting kan gå galt, og hvilke fejl man ikke må lave. Sikkerhedsmæssige fejl, såsom at forsømme sikkerhed på ens database, og hvor lang tid der går før, at der er mennesker ude i verden, som ønsker tilgang til den.
Det er en fejl og forsømmelse, som ikke vil ske i fremtiden.
Alle emner har været yderst relevante og har bidraget til systemets velvære.
En ting som gik mindre godt var _maintenance_-delen. Alle har forskellige ambitionsniveauer og problemer i forhold til projektet. Når man samtidig er afhængige af andre gruppers præstationer kan dette føre til en mindre god oplevelse. Vi må være ærlige at sige, at vi har fejlet på den front. Vi har haft en del problemer med f.eks. logging. Vores Droplet på Digital Ocean, som vi betaler for, havde som standard 1gb ram. Elasticsearch som tilhørte logging-delen krævede som standard 2gb. Med respekt til de andre komponenter og vores applikation blev vi nødsaget til at opgradere vores droplet og betale en del mere end vi havde forventet. Ydermere, gik det aldrig helt godt med at få logging til at fungere. Det går selvfølgelig ud over den gruppe, som skal monitorere os og skal have adgang til vores logs. Opsummeret set, kan det være skyld i, at den anden gruppe ikke kan aflevere en aflevering og kan miste study points på bekostning af andre.
Alt i alt, har det været en lærerig rejse.

En beskrivende faktor til hvorfor maintenance-delen ikke kørte helt optimalt, kunne være måden vores arbejdsprocess ændrede sig på undervejs. Under udviklingsfasen af systemet mødtes vi for det meste og programmerede sammen. Det gør det langt nemmere at kommunikere, og er også mere motiverende. Dét ændrede sig af forskellige årsager en smule, da vi startede på maintenance-delen. Efter systemet var færdigudviklet, og opgaverne blev mere og mere centreret omkring vedligeholdelse og logging, stoppede vi nemlig med at mødes. Når der skulle laves arbejde i form af udvikling, rapportering osv &quot;mødtes&quot; vi oftest om aften over Skype. Dette skyldtes til dels at flere i gruppen har jobs ved siden af studiet, som gjorde det svært at finde dage vi kunne mødes, og dels fordi at flere af os nok føler, at vi blev præsenteret for lidt for mange emner lidt for hurtigt. Ingen af os fik helt følelsen af at komme &quot;under huden&quot; med disse teknologier - hvilket er synd - da det er et spændende og vigtigt område. Hvad kan vi bruge denne viden til nu? For det første, skulle vi nok have holdt ved med vores oprindelige arbejdsprocess, hvor vi mødtes fysisk og arbejde sammen. For det andet, ville det have været en fordel at bruge endnu mere tid på at sætte sig ind i de mange forskellige teknologier. Men dét er dog lettere sagt end gjort, da vi ved siden af dette projekt har andre fag som ligeledes kræver tid.

---

## Referencer.

* Github branch: [https://guides.github.com/introduction/flow/](https://guides.github.com/introduction/flow/)
* Express.js https://expressjs.com/
* pug.js https://pugjs.org/api/getting-started.html
* UP https://github.com/datsoftlyngby/soft2017fall-lsd-teaching-material/blob/master/lecture_notes/10-Disciplined%20Methods/UP%20Use%20Cases.pdf
* XP https://en.wikipedia.org/wiki/Extreme_programming
