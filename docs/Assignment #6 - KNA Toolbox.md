The goal of this assignment is to create the basis for a FKA-Toolbox contract
between the frontend and the backend subsystems of your large systems
development project.


## 1 Divide into sub-systems
Vi har delt vores projekt ind i to subsystemer:

**Backend**: [Backend](https://github.com/gode-ting/hackernews-clone-backend)

**Frontend**: [Frontend](https://github.com/gode-ting/hackerNews-clone-project-frontend)

## 2 Logical Data Model

*Logical data model of our system*

![alt text](https://github.com/gode-ting/hackerNews-clone-project/blob/master/docs/Logical%20Datal%20Model_updated.png "Logical Data Model")



## 3 Use Case Model

### A complete use case diagram

![alt text](https://github.com/gode-ting/hackerNews-clone-project/blob/master/docs/Use%20Case%20Diagram.png "Use case diagram")



### A fully dressed use case description for all use cases, or at least the use-cases identified above.


### 1: Use case description for at logge ind

__Use case__ Log ind

__Scope__ N/A

__Level Goal__ User

__Primary Actor__  Man skal være på login siden.

__Precondition__ 

__Main succes scenario:__

1. Brugeren klikker på login menuen.
2. Systemet spørger efter brugernavn og password.
3. Brugeren indsætter brugernavn og password.
4. Systemet tjekker brugernavn og passwordet.
5. Hvis inputværdierne er valide vil man blive logget ind og få vist forsiden, hvor man kan se, at man er logget ind.


__Success guaratess__ 


__Extensions__ 


__Special Requirements__ 


### 2: Use case description for oprette en profil

__Use case__ Oprette en profil

__Scope__ N/A

__Level Goal__ Blive en registeret user, således, at man kan få rettigheder til at uploade, up/downvote m.m

__Primary Actor__  User

__Precondition__ Ingen

__Main succes scenario:__

1. User tilgår sitet.
2. User klikker på register på forsiden.
3. User indsætter brugernavn og password.
4. Systemet validerer brugernavn og password.
5. Systemet registerer useren til systemet.


__Success guaratess__ Useren er blevet registeret på siden.


__Extensions__ Ingen


__Special Requirements__ NONE

 
### 3: Use case description for at logge ud

__Use case__ Log ud

__Scope__ N/A

__Level Goal__ Logge ud af systemet og dermed ikke have de samme rettigheder, som da man var logget ind.

__Primary Actor__  User

__Precondition__ Man skal være logget ind.

__Main succes scenario:__

1. Brugeren klikker på logout knappen.
2. Man har ikke de samme rettigheder, som da man var logget ind.

__Success guaratess__ Useren er logget ud af systemet.


__Extensions__ Log ind


__Special Requirements__ NONE


 
### 4: Use case description for at oprette en post

__Use case__ Opret en post

__Scope__ N/A

__Level Goal__ At oprette en post og den forekommer på forsiden.

__Primary Actor__  User

__Precondition__ Man skal være logget ind

__Main succes scenario:__

1. Useren er forsiden og klikker på “Create”.
2. Useren er inde på en side, hvor der kan indsættes information omhandlende posten.
3. Useren klikker på “publish”
4. Systemet validerer input felterne.
5. Systemet tilføjer nyheden til feedet.

__Success guaratess__ N/A

__Extensions__ N/A

__Special Requirements__ NONE
 

### 5: Use case description for at redigere en post

__Use case__ Redigere en post 

__Scope__ N/A

__Level Goal__ At indholdet på det pågældende post bliver ændret til det ønskede.

__Primary Actor__ User

__Precondition__ Useren er logget ind.

__Main succes scenario:__

1. Useren klikker på sin post, som han vil redigere.
2. Useren er inde på en side, hvor der kan redigeres information omhandlende posten.
3. Useren klikker på “publish”
4. Systemet validerer input felterne.
5. Systemet redigerer posten.


__Success guaratess__ At posten bliver redigeret

__Extensions__ Opret post

__Special Requirements__ NONE
 

### 6: Use case description for at slette en post.
__Use case__ Slet post

__Scope__ N/A

__Level Goal__ Useren sletter sin post og den forsvinder.

__Primary Actor__  User

__Precondition__ Useren er logget ind og har oprettet en post.

__Main succes scenario:__

1. Useren klikker på "Delete" ud fra sin post.
2. Systemet sletter posten.

__Success guaratess__ Posten bliver slettet

__Extensions__ Opret post

__Special Requirements__ NONE

 
### 7: Use case description for at upvote en post

__Use case__ Upvote post

__Scope__ N/A

__Level Goal__ En post bliver upvoted og en tilhørende user får karma points

__Primary Actor__ User

__Precondition__ En post er oprettet og useren er logget ind.

__Main succes scenario:__

1. Useren klikker på upvote knappen tilhørende en post.
2. Posten får et upvote.
3. Den tilhørende User får karma point.

__Success guaratess__ Den pågældende post bliver upvoted.

__Includes__ User får karma point.

__Special Requirements__ NONE

 
### 8: Use case description for at downvote post

__Use case__ Downvote post

__Scope__ N/A

__Level Goal__ Posten får et downvote og den tilhørende User få fjernet karma point

__Primary Actor__  User

__Precondition__ En post er oprettet, User er logget ind, og har nok karma point til at downvote.

__Main succes scenario:__

1. Useren klikker på downvote knappen tilhørende en post.
2. Systemet tjekker om brugeren har nok karma points til at downvote
2. Posten får et downvote.
3. Den tilhørende User mister karma points.

__Success guaratess__ Den pågældende post bliver downvoted.

__Extensions__ N/A

__Special Requirements__ Useren skal have det krævet antal karma points for at kunne downvote.

 
### 9: Use case description for at kommentere på en post

__Use case__ Kommenter på post

__Scope__ N/A

__Level Goal__ At der bliver oprettet en kommentar på en post.

__Primary Actor__  User

__Precondition__ Useren skal være logget ind.

__Main succes scenario:__

1. Useren er inde på forsiden og ser på feedet.

2. Useren klikker på “comments” under nyheden som har interesse.

3. Useren ser alle kommentarer til den pågældende nyhed.

4. Useren klikker på “comment”.

5. Useren indsætter input til feltet.

6. Useren klikker på “Send”

7. Systemet validerer input.

8. Systemet tilføjer kommentaren til nyheden.


__Success guaratess__ N/A 


__Extensions__ N/A

__Special Requirements__ NONE 
 
### 10: Use case description for at kommentere på en kommentar

__Use case__ Kommenter på en kommentar

__Scope__ N/A

__Level Goal__ Kommentaren får en kommentar

__Primary Actor__  User

__Precondition__ Useren skal være logget ind.

__Main succes scenario:__

1. Useren er inde på forsiden og ser på feedet.

2. Useren klikker på “comments” under nyheden som har interesse.

3. Useren ser alle kommentarer til den pågældende nyhed.

4. Useren klikker på “comment” under en kommentar.

5. Useren indsætter input til feltet.

6. Useren klikker på “Send”

7. Systemet validerer input.

8. Systemet tilføjer den nye kommentar til kommentaren.


__Success guaratess__ Kommentaren bliver tilkoblet kommentaren(parent)


__Extensions__ N/A


__Special Requirements__ N/A

 
### 11: Use case description for at markere en post som spam

__Use case__ Marker som spam

__Scope__ N/A

__Level Goal__ Den pågældende post bliver markeret som spam.

__Primary Actor__  User

__Precondition__ Useren skal være logget ind.

__Main succes scenario:__

1. Useren er inde på forsiden og kigger på feedet.
2. Useren klikker på en “marker som spam” knap.
3. Systemet registrer det som spam.

__Success guaratess__ Posten er blivet markeret som spam.


__Extensions__ N/A


__Special Requirements__ N/A

 

### A brief use case description for all other use-cases if any

### A description of all actors, including responsibilities.

**User**: 

Vi har én actor, som er en User. 

*Hvorfor?*

Fordi alle besøgende på siden vil blive behandlet som en user. En user kan være logget ind og deraf have flere rettigheder end en user, som ikke er logget ind. 
En user som er logget ind kan oprette posts, kommentere, markere en post som spam, optjene karma point, up/downvote m.m.

Ved at kunne foretage disse handlinger medfølger der et *naturligt ansvar* for at up/downvote retfærdigt. Ydermere, skal man heller ikke markere en post for spam, hvis man ikke kan lide en anden user. 

**Man kunne godt** argumentere for, at der skulle være flere actors. 

I opgavebeskrivelse er der beskrevet en simulator *simulator program to publish stories and comments to your system.*
Men simulatoren simulerer en User og derfor vil den kunne gå under som en User. 
 

### A Sub-system sequence diagram for all identified scenarios in the usecases

![alt text](https://github.com/gode-ting/hackerNews-clone-project/blob/master/docs/System%20Sequence%20Diagram%20-%20Login.png "SSD")

![alt text](https://github.com/gode-ting/hackerNews-clone-project/blob/master/docs/System%20Sequence%20Diagram%20-%20All%20Posts.png "SSD")

![alt text](https://github.com/gode-ting/hackerNews-clone-project/blob/master/docs/System%20Sequence%20Diagram%20-%20Comment%20Post.png "SSD")

![alt text](https://github.com/gode-ting/hackerNews-clone-project/blob/master/docs/System%20Sequence%20Diagram%20-%20Upvote.png "SSD")


