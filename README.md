# PROG7311 2021

# LAC Task one:
## Ensure that you have completed the following before you attempt LAC 1:
1. You have installed Ubuntu on Hyper-V
2. You have installed the .netcore SDK on your  Ubuntu Virtual Machine 
3. You have installed Visual Studio Code on Ubuntu Virtual Machine 

### You should now be able create a .netcore web api using the dotnet tool. Read this article and follow the steps to create the API. 

1. Use the terminal to create a folder called dev in your home folder. 
1.1. Some help on how to make and navigate folders in Ubuntu here :
[navigate folders in Ubuntu](https://www.codegrepper.com/code-examples/shell/how+to+navigate+to+a+folder+in+terminal+ubuntu)

2. Create a folder called your student number in the dev folder 

4. Make sure you are in the folder labelled your student number 
5. Run the following command to create your web api:

```dotnet new webapi -o my-api```

5. Now run the ls command in the terminal - you see a foldr called my-api
6. You can now open the folder from within Visual studio code with File >Open Folder 
6.1. Some help on Visual studio code here : 
[Using Visual Studio Code](https://stackoverflow.com/questions/51151953/how-to-open-folder-in-new-vs-code-instance-by-right-clicking-on-the-folder#:~:text=This%20extension%20allows%20you%20to,in%20a%20new%20vscode%20window.&text=You%20can%20go%20via%20File,open%20the%20second%20folder%20there.&text=cmd%20prompt%20will%20open%20with%20the%20folder%20URL)

8. Now  run your web api and access the JSON that it serves by default
[creae web api with Visual Studio Code](https://www.youtube.com/watch?v=UG1AdlmrJGQ)

### Push your code to github 
[how to push your project to Github](https://www.youtube.com/watch?v=KlggT7ZjKOw)

9. Add me to your repo (user name = TillCM)
[How to add a collaborator to a Github Repo](https://docs.github.com/en/github/setting-up-and-managing-your-github-user-account/inviting-collaborators-to-a-personal-repository)


# PROG7311 2021

# LAC Task Two: Advanced OOP Concepts 

You should be familiar with OOP in terms of :
1. Abstraction
2. Inheritance
3. Encapsulation
4. Polymorphism 

We now need to extend on these topics by looking at what SOLID and Loosely coupled applications are 
We are going to start with developing a class diagram for the following functional and non functional requiremetns :
You have been contacted by VarsityCollege to create a Result calculator that students can use to pre-calculate their results when needed. Here is a reminder of how results are calculated for BCAD 1
### PROGf5112
1. ICE = 10%
2. Assignment 1 = 25%
3. Test = 35%
4. Exam = 30% 
We first calculate cass which counts for 70% of the course grade (in this example) 
CASS = (Mark for ICE + Mark for Assignment 1 + Mark for Test)/70*100*
We then calculate the final result Fina result = (CASS+ Exam)/100*100*

### PRLD5112:
1. ICE = 10%
2. Assignment = 50%
3. FInal Exam  = 40%

### PRSE6212
1. ICE = 10%
2. Assignment = 30%
3. Assignment = 35%
4. Final Exam  = 35%

You are required to design a system that allows an administrator to create a course that can contain any number of modules. The modules can contain any of the following assessment points :
1. *One Formative Assessment Modules* Contains ICE of 10% , a Single other assessment of 50% and an Final ExM of 40%
2. *Two Formative Assessment Modules* Containd ICE of 10%, an assessment that counts 30% , and assessment that counts 35% and a Final exam of 35%
3. *Three Formative Assessment Modules* Contains ICE of 10%, three assessments of 20% each and a inal exam of 30%

The assessment weightings will change every year , they cannot be hard coded. THe module names will change every year they cannot be hard coded 
You can use the following links to help you:
1. [Understanding Losely coupled Applications](https://nordicapis.com/the-difference-between-tight-coupling-and-loose-coupling/)
2. [What is SOLID](https://mari-azevedo.medium.com/s-o-l-i-d-principles-what-are-they-and-why-projects-should-use-them-50b85e4aa8b6)

DONT FREAK OUT, THIS IS NEW, IT IS WEE BIT TRICKY ..... BUT WE GOT THIS..... WE CAN DO THIS...... 

## Final instructions
1. Work in groups ( you are welcome to use the Discord server - even those of you who have said terrible things about it)
2. Create your class diagram using draw.io or Lucidchart (ONLINE just google)
3. We will get your diagrams into your repos tommorow. 

## Microservices, Docker and Docker Compose 
1. You need to ensure that you have a running web api that implements the factory pattern to return JSON data.... 
2. You then need to containarize your app by making use of Docker. You can find out more about docker here :
3. [What is docker ]("https://www.youtube.com/watch?v=_dfLOzuIg2o&t=70s")
4. Here is some help to containarize your wep api 



```# https://hub.docker.com/_/microsoft-dotnet
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app

# install debug tools

#RUN apt-get install tree

#Environment Variables 
#ENV ConnectionStrings: "Data Source=db,1433;Database=teamfu;User Id=SA;Password=Your_password123;"

# copy csproj and restore as distinct layers
COPY src/com.teamfu.be/team-reece/team-reece.csproj ./
#RUN tree
RUN dotnet restore team-reece.csproj
RUN ls -l

# copy everything else and build app
COPY src/com.teamfu.be/team-reece/. /app/team-reece/
WORKDIR /app/team-reece
RUN mkdir /publishedApp
ENV DATABASE_SERVER =db
RUN dotnet publish team-reece.csproj -c release -o /publishedApp

# final stage/image
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS runtime
ENV TZ=Africa/Johannesburg
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y tzdata curl
WORKDIR /app
COPY --from=build /publishedApp .

HEALTHCHECK CMD curl --fail http://localhost:80 || exit 1

ENTRYPOINT ["dotnet", "team-reece.dll"]
```
5. This Dockerfile will create a Docker Image  wich will contain your app. You will see that I am copying over my own app in the example. 
6. Create a DockerFile to create a Docker Image that contains your web api 

## SQL Server with Docker-comple 
[what is docker-compose/]("https://www.youtube.com/watch?v=vQmk9moF8vw")

7. You now need to start your webapp with docker-compose 
8. AND you need to add a SQL server docker container user docker-compose 
```version: '3.8'
services:
  webapp:
    image: team_reece/teamfu:latest
    build: 
      context: ../
      dockerfile: Dockerfile
    ports:
      - "5001:80"
    environment: 
    - DATABASE_SERVER=db
    - DATABASE_PORT=1433
    - DATABASE_USER=SA
    - DATABASE_PASSWORD=Your_password123
    - DATABASE_NAME=teamfu 
    networks: 
     - team-reece
  ```
  
  8. Here is a docker-compose file that creates a SQL Server 

```version: "3.8"
services:
  
  db:
    image: "mcr.microsoft.com/mssql/server:2019-latest"
    restart: always
    environment: 
      MSSQL_TCP_PORT: 1433
      MSSQL_PID: Developer
      SA_PASSWORD: "Your_password123"
      ACCEPT_EULA: "Y"
    extra_hosts: 
      - "localhost:192.168.10.55"  
    user: root 
    networks: 
      - team-reece
    ports: 
      - 1433:1433
    volumes:
      - ./mssql/data:/var/opt/mssql/data
      - ./mssql/log:/var/opt/mssql/log
      - ./mssql/secrets:/var/opt/mssql/secrets
      - ./mssql/sql:/tmp/sql
```

We finally need to ensure that our two containers are talking to each other and that our web api can deliver data out of db..

COME TO CLASS IF YOU ARE UNSURE !!!!!!!
