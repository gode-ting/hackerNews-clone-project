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
