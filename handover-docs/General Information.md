# General information

## How to connect to the Mongo database running on the server
If you need to connect to the database that is running on the server from your local machine, it is really easy. All you have to do is to download the Robomongo software. Here is a link: `https://robomongo.org/`.

Once you have downloaded the software and opened it you can create a connection. To do that you need an ip address and a port number. Enter the following `46.101.190.192:32768`

- `46.101.190.192` : the specific ip address on which the database is hosted
- `32768` : the specific port that mongo db is listening on

Now, you should be able to look at what is inside the database. 

## About the missing requirements
The system does not contain all use cases as described in the original requirements analysis document. The complete and finished use case diagram is attched in the documents.

## System architecture
The complete system is divided in 2 seperate components 1) a `frontend component`, and 2) a `backend component`.

* __*The Backend System*__  

  The backend is a RESTful webservice completely written in Java using the Spring Boot framework. The database used is a         MongoDB.
  
* __*The Frontend System*__  

  The frontend is a website written in JavaScript using a popular JS template engine named Pug. 
  
* __*Containerization*__  

  We use Docker to containerize our environment.
  
* __*Continous Integration and Deployment using Travis CI*__  

  We are utilizing the benefits of Travis CI and Docker to easily deploy out api to Digital Ocean

* __*Hosting__*  

  The frontend website is hosted at GitHub. The backend is hosted at Digital Ocean.

