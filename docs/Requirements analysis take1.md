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

