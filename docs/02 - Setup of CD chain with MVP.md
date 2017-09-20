# Setup of CD chain with MVP

**Table of content**.

* [Link](#link)
* [Description](#description)
* [Setup and configuration](#setup-and-configuration)

---

## Link

[Link to the MVP application](http://46.101.190.192:8080/choir-frontend/ChoirManager)

## Description

Vi har valgt at bruge det CI/CD setup, som vi blev præsenteret for i undervisningen. 

Det vil sige:

VCS: Git og GitHub som host.

 
Build Server: Jenkins


Docker containers og DockerHub som public registry.


Vagrant, til opsætning og håndtering af virtuelle maskiner.


Maven repository manager – Artifactory.


Cloud server provider – Digital Ocean.

Vi har alle prøvet at bruge Travis CI, som også er et værktøj til continuous integration. 
Travis CI er en hosted service, som gør det lidt lettere end Jenkins, hvor man selv skal hoste, installere og opsætte det.
Travis har dog ikke build jobs ligesom Jenkins, og med Jenkins er der mange muligheder for at tilpasse jobs til forskellige situationer og opsætninger. Derfor har vi valgt at prøve kræfter med Jenkins.

Git – kort opsummeret: versionsstyringssystem som kan tracke ændringer i computerfiler og koordinering af arbejdet på disse filer blandt flere personer. Det bruges primært til kildekodehåndtering.

Jenkins kort opsummeret: open-source CI software værktøj.

Docker containers – kort opsummeret: Bygge- og implementeringsværktøj. Baseret på ideen om, at du kan pakke din kode med afhængigheder til en deployable unit som kaldes en container.

Vagrant – kort opsummeret: Opbygning og vedligeholdelse af portable virtuelle udviklingsmiljøer.

Artifactory – kort opsummeret: Et repository hvor man kan hente filer fra.

Digital Ocean – kort opsummeret: Cloud computing platform. Server.

## Setup and configuration

Vi har opsat og konfigureret vores build jobs præcist som i [denne tutorial](https://github.com/datsoftlyngby/soft2017fall-lsd-teaching-material/blob/master/lecture_notes/05-Continuous%20Integration%20and%20Delivery.ipynb).

Vi har forket de nødvendige projekter og tilpasset build jobs til at hente fra dem.

HUSK (note to self): Skift til mocking branchen i frontend-choir og ændre repositories i build.grandle
