# Requirements analyse

## Purpose of the system

Formålet med systemet er at lave et socialt medie med fokus på Computer Science, hvor registeret brugere kan tilføje nyheder og historier fra diverse websites. Alle nyheder og historier som bliver tilføjet til websitet skal indeholde en relevans i forhold til områderne indenfor Computer Science. 
Alle registrerede brugere af systemet kan upvote, downvote og kommentere en nyhed. Dette resulterer i, at man kan få øjeblikkelig feedback på den nyhed, som man har tilføjet.

---

## Scope of the system

Der skal bygges en web-applikation, hvor der kan tilføjes nye nyheder og kommentarer til nyhederne. Web-applikationen skal præsentere en oversigt over af nyhederne. Herfra kan man gå til nyheden eller gå til kommentarerne tilknyttet.

---

## Objectives and success criteria of the project

Der er op til flere succeskriterier for, at web-applikationen kan kaldes en succes. Web-applikationen skal leve op til både de funktionelle og non-funktionelle krav. Dertil skal der være en brugervenlighed hvorpå man ikke er i tvivl om, hvordan web-applikationen bruges.
Et af kravene er f.eks. ” your system has to have an uptime of more than 95% “. Dette ville være et succeskriterie, da der forventes, at man skal kunne publishere nyheder og kommentere nyhederne via REST API. Hvis systemet ikke har en stabil oppetid vil det resultere i, at man ikke kan tilgå REST API’et. Systemet vil gøre det let for brugere at kunne publishere nyheder og få tilhørende feedback fra andre brugere. 

---

## Definitions, acronyms, and abbreviations

### Definitioner


### Brugervenlighed

Er den måde, at det byggede system ikke udviser en tvetydighed om hvordan det bruges. Det er designet på en måde hvorpå de nye best practices benyttes med hensyn til design (fonts, farver mm.)

### Rest API

**Representational state transfer (REST)**.

Et RESTful API er et application program interface (API) som bruger HTTP requests til GET, PUT, POST og DELETE data.

### Frontend

Frontend associeres ofte med webbaserede softwaremoduler baseret på HTML, CSS og JavaScript, da disse typer programmering alle afvikles af klienten

### Backend

Server

## Acronyms and abbreviations

REST - Representational state transfer 

## References

https://en.wikipedia.org/wiki/Hacker_News

https://news.ycombinator.com/

Object-Oriented Software Engineering Using UML, Patterns, and Java Bernd Bruegge & Allen H. Dutoit

## Overview

Systemet bliver bygget til at være en pendant til Hacker News. Funktionaliteten vil grundlæggende være den samme. Systemet forventes at være brugervenlig og stabil.

## Current System

Det nuværende system, Hacker News, er et socialt nyhedswebsted med fokus på computervidenskab og iværksætteri. Formålet var at genskabe et nyhedswebsted svarende til Reddit. I modsætning til Reddit, hvor nye brugere straks både kan stemme op og ned stemme, tillader Hacker News ikke brugere at ned stemme indhold, før de har akkumuleret 500 "karma" -point. Karma-point beregnes som antallet af opvoter, som en given brugerens indhold har modtaget minus antallet af downvotes. Med Karma har alle brugere mulighed for at markere indsendte indhold som spam.

## Proposed system

Det nye system vil være en kopi af Hacker News med henblik på nogle ændringer, som vi ser manglende i det nuværende system. Først og fremmest vil der blive reduceret i de antal af point som kræves for at upvote eller downvote. I det nye system vil det kun kræve 50 point, da vi ser det som en fordel jo flere bruger som kan give deres mening til kende, uden at skulle kommentere på en nyhed. 
Du vil tjene 1 point om dagen. Dertil vil du tjene flere point hvis du publisherer nyheder.
På det nuværende system er det svært at se hvad guidelines er. Det resultere i irrelevante opslag og hadefulde kommentarer. Det nye system vil i højere grad havde fokus på guidelines, og sikre sig, at alle nye brugere bliver præsenteret for guidelines.

# Functional Requirements

* Man skal kunne oprette en bruger
* Man skal kunne logge ind
* Man skal kunne logge ud
* Kunne poste nyheder
* Kunne kommentere på nyheder
* Kommentere på andre kommentarer
* Kunne upvote en nyhed
* Kunne downvote en nyhed
* Pointystem
* Man skal kunne markere en nyhed som spam
* Vise en liste af nyheder
* Man skal kunne skjule en nyhed
* Glemt password


# Non Functional Requirements

## Usability
Alle som har kendskab og har benyttet nyhedssites som f.eks. Reddit og Hacker News, vil have let ved at bruge systemet. Det er en kopi af Hacker News, og vil derfor have et brugervenligt interface samt genkendeligt feed. Det vil gøre, at der ikke vil opstå tvivl i forhold til brug af systemet.

## Reliability 
The system has to have an uptime of more than 95%. Your system shall not 'loose' any content, which is sent to it from the simulator program. That is, even if your system is for example down for upgrade, there should be a mechanism buffering incoming content, which can be published to the system when it is operational again.
## Performance 
Det vigtigste indhold på siden skal være visningsklar inden for tre sekunder, således at siden er brugbar. Mindre vigtigt indhold kan godt indlæses efterfølgende.

## Supportability
Vi understøtter de større browsere som FireFox, Chrome, Safari, Edge, Opera. Logning af systemet til fejlløsning. 

## Implementation
Vi kører med en Heroku Hobby server. Never Sleeps, gratis SSL, Application Metrics, Multiple Worjers, 512 MB RAM. $7/month. Vi bruger både en MongoDB samt SQL database på serveren. 

## Legal
Brugeren skal acceptere cookies

## Interface
Vi bestræber os på, at lave en simpel brugergrænseflade som er intuitiv, simpel og naturlig for brugeren.
Packaging: Vores system kører via internetbrowser, som er platformuafhængig.

# System Models

## Scenarios

Succesful logging into the system
1. Brugeren går ind på web-applikationen.
2. Brugeren skriver sine credentials i de tomme tekst felter: ”username” og ”password”.
3. Brugeren trykker på ”log in” knappen.
4. Web-applikationen linker til frontpagen.

Unsuccesful logging into the system
1. Brugeren går ind på web-applikationen.
2. Brugeren skriver forkerte credentials i de tomme tekst felter: ”username” og ”password”.
3. Brugeren trykker på ”log in” knappen.
4. Web-applikationen printer ”The username or password are incorrect”.

Reporting a story for spam
1. Brugeren klikker på ”report for spam” under en post som brugeren har set før.
2. Web-applikationen printer ”Thanks for your report”.

De følgende scenarier forventer at brugeren allerede er logget ind i systemet. (se evt. ”Logging into the system”).
Succesfully creating a story
1. Brugeren klikker på ”Create story” knappen.
2. Brugeren udfylder den tomme formular.
3. Brugeren klikker på ”Post story” knappen.
4. Web-applikationen printer ”Story was posted”.

Unsuccesfully creating a story
1. Brugeren klikker på ”Create story” knappen.
2. Brugeren udfylder ikke den tomme formular.
3. Brugeren klikker på ”Post story” knappen.
4. Web-applikationen printer ”Story was not posted because there was no content”.

Succesfully commenting on a story
1. Brugeren klikker på et opslag, som vedkommende ville kommentere.
2. Brugeren skriver en kommentar i det tomme tekst felt.
3. Brugeren klikker på ”Comment” knappen.
4. Kommentaren bliver nu vidst i tråden.

Unsuccesfully commenting on a story
1. Brugeren klikker på et opslag, som vedkommende ville kommentere.
2. Brugeren skriver intet i det tomme tekst felt.
3. Brugeren klikker på ”Comment” knappen.
4. Web-applikationen printer ”The comment was empty and was therefor not added”.

Succesfully commenting on a Comment
1. Brugeren klikker på et opslag.
2. Brugeren klikker på ”reply” knappen på en kommentere som vedkommende ville kommentere på.
3. Brugeren skriver en kommentar i det tomme tekst felt, som dukkede op efter at have klikket på ”reply” knappen.
4. Brugeren klikker på ”Comment” knappen.

Unsuccesfully commenting on a Comment
1. Brugeren klikker på et opslag.
2. Brugeren klikker på ”reply” knappen på en kommentere som vedkommende ville kommentere på.
3. Brugeren skriver intet i det tomme tekst felt, som dukkede op efter at have klikket på ”reply” knappen.
4. Web-applikationen printer ”The comment was empty and was therefor not added”.

Up-voting story
1. Brugeren klikker på pilen der pejer op ved siden af et opslag som vedkommende godt kan lide.
2. Web-applikationen highlighter pilen for at indikere at opslaget er blevet up-voted.

Down-voting story (with 500 karma points)
1. Brugeren klikker på pilen der pejer ned ved siden at et opslag som vedkommende ikke kan lide.
2. Web-applikationen highlighter pilen for at indikere at opslaget er blevet down-voted.

Down-voting story (without 500 karma points)
1. Brugeren klikker på pilen der pejer ned ved siden at et opslag som vedkommende ikke kan lide.
2. Web-applikationen printer ”You need more then 500 karma points to downvote”.



## Use Case Models

![](https://github.com/hilleer/hackerNews-clone-project/blob/master/use%20case.png)

### 1: Use case description for at logge ind
__Use case name__ Login

__Summary__ System validerer brugeren.

__Actor__ User

__Precondition__  Man skal være på login siden.

__Main sequence:__

1. Brugeren klikker på login menuen.
2. Systemet spørger efter brugernavn og password.
3. Brugeren indsætter brugernavn og password.
4. Systemet tjekker brugernavn og passwordet.
5. Hvis inputværdierne er valide vil man blive logget ind og få vist forsiden, hvor man kan se, at man er logget ind.

_**Alternative sequence:**_

Step 5: Hvis brugernavn eller password er forkert, vil systemet komme med en fejlmeddelse og spørger efter det korrekte brugernavn og password.

### 2: Use case description for at markere en nyhed som spam(report story)

__Use case name__ Report story

__Summary__ Brugeren markere en nyhed som spam.

__Actor__ User

__Precondition__ Man skal være logget ind.

__Main sequence:__

1. Brugeren er inde på forsiden og kigger på feedet. 
2. Brugeren klikker på en “marker som spam” knap. 
3. Systemet registrer det som spam. 

_**Alternative sequence:**_

Step 3: Hvis systemet har modtaget adskillige klager, vil systemet slette nyheden. 

### 3: Use case description for at publishere en nyhed
__Use case name__  Create story

__Summary__ Brugeren publisherer en nyhed

__Actor__ User

__Precondition__ Man skal være logget ind.

__Main sequence:__

1. Brugeren er forsiden og klikker på “Create”. 
2. Brugeren er inde på en side, hvor der kan indsættes information omhandlende nyheden. 
3. Brugeren klikker på “publish” 

4. Systemet validerer input felterne. 

5. Systemet tilføjer nyheden til feedet. 

_**Alternative sequence:**_

Step 4: Hvis systemet ikke kan validere input felterne, vil der blive vist en fejlmeddelse.

### 4: Use case description for at registerer sig som bruger

__Use case name__  Register user

__Summary__ En person kan registere sig som bruger af sitet.

__Actor__  Person

__Main__ sequence:

1. Personen tilgår sitet.
2. Personen klikker på register på forsiden. 
3. Brugeren indsætter brugernavn og password.
4. Systemet validerer brugernavn og password.
5. Systemet tilføjer personen til systemet.

_**Alternative sequence:**_

Step 4: Hvis systemet ikke kan validere input felterne, vil der blive vist en fejlmeddelse.

### 5: Use case description for at kommentere på en nyhed

__Use case name__ Comment story

__Summary__ Brugeren kommenterer en nyhed.

__Actor__ User

__Precondition__ Man skal være logget ind.

__Main sequence:__

1. Brugeren er inde på forsiden og kigger på feedet. 
2. Brugeren klikker på “comments” under nyheden som har interesse. 
3. Brugeren ser alle kommentarer til den pågældende nyhed. 

4. Brugeren klikker på “comment”. 

5. Brugeren indsætter input til feltet.

6. Brugeren klikker på “Send” 

7.  Systemet validerer input. 

8. Systemet tilføjer kommentaren til nyheden.

### 5: Use case description for at kommentere på en kommentar til en nyhed.

__Use case name__  Comment on a comment story

__Summary__ Brugeren kommenterer en kommentar på en nyhed.

__Actor__ User

__Precondition__ Man skal være logget ind.

__Main sequence:__

1. Brugeren er inde på forsiden og kigger på feedet. 
2. Brugeren klikker på “comments” under nyheden som har interesse. 
3. Brugeren ser alle kommentarer til den pågældende nyhed. 

4. Brugeren klikker på “comment” under en kommentar.

5. Brugeren indsætter input til feltet.

6. Brugeren klikker på “Send” 

7.  Systemet validerer input. 

8. Systemet tilføjer den nye kommentar til kommentaren.

_**Alternative sequence:**_

Step 7: Hvis systemet ikke kan validere kommentaren, bliver der vist en fejlmeddelse.

### 6: Use case description for at upvote en nyhed.

__Use case name__  Upvote story 

__Summary__ Brugeren upvoter en nyhed

__Actor__    User

__Precondition__    Man skal være logget ind og have optjent minimum 50 point.

__Main sequence:__

1. Brugeren er inde på forsiden og kigger på feedet. 
2. Brugeren klikker på “upvote”-ikonet ved siden af en nyhed, som brugeren synes godt om.
3. Systemet validerer  at brugeren har optjent 50 point.

4. Systemet tilføjer et upvote på nyheden.

_**Alternative sequence:**_
Step 6: Hvis systemet ikke kan validere at brugeren har minimum 50 point, bliver der vist en fejlmeddelse.

### 7: Use case description for at downvote en nyhed.

__Use case name__ Downvote story 

__Summary__ Brugeren downvoter en nyhed

__Actor__ User

__Precondition__ Man skal være logget ind og have optjent minimum 50 point.

__Main sequence:__

1. Brugeren er inde på forsiden og kigger på feedet. 
2. Brugeren klikker på “downvote”-ikonet ved siden af en nyhed, som brugeren ikke synes godt om.
3. Systemet validerer  at brugeren har optjent 50 point.
4. Systemet tilføjer et downvote på nyheden.

_**Alternative sequence:**_

Step 6: Hvis systemet ikke kan validere at brugeren har minimum 50 point, bliver der vist en fejlmeddelse.


# Glossary

* __Flag__ - En funktion, der giver en bruger mulighed for at markere/rapportere en nyhed som krænkende, spam, spoiler.
* __Upvote / Downvote__ - Brugere skal have mulighed for at bedømme andre brugeres indlæg ved brug af upvote og downvote funktion.
* __Scenarie__ - et usage scenarie er beskrivelsen af et "real-world" eksempel, hvor en eller flere mennesker bruger en af systemets funktioner
