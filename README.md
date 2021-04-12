<p align="center">
  <img width="100" src="https://www.pngkey.com/png/full/17-179750_discord-icon-discord-logo.png">
</p>

# Discord Bot
![discord-net](https://img.shields.io/badge/discord--net-v2.2.0-blue)
![csharp](https://img.shields.io/badge/C%23-8.0-blue)
![ide](https://img.shields.io/badge/IDE-vs2019-blue)

All-around Discord bot written in C# using Discord.NET (new repo) 

## About

The Discord bot is part of a [microservice architecture](https://github.com/roedebaron/discordbot-microservice-architecture) managed by Docker Compose and depends on multiple other services in order to execute its' functionality - please see my other projects: 

- [image-caption-ai](https://github.com/roedebaron/image-caption-ai)
- [aau-schedule-scraping](https://github.com/roedebaron/aau-schedule-scraping)
- [google-drive-service](https://github.com/roedebaron/google-drive-service)
- [corona-stats](https://github.com/roedebaron/corona-stats)
- [reddit-service](https://github.com/roedebaron/reddit-service)
- [chart-service](https://github.com/roedebaron/chart-service)

The internal architecture is based on the MVC-pattern following the principles from Domain-Driven Design (DDD). 

A brief overview of the layers: 

**Core**: Business logic. Represents domain objects and services. 

**View**: Represents the Discord messages to be returned to a Discord guild i.e. a "View" is simply a formatted string to be interpreted by the Discord UI client. 

**Controller**: Recieves incoming Discord commands which are then delegated to **Core**. The result from Core is represented as a Discord message (a View) and returned to the client.  

**Infrastructure**: Data persistence and external web services. 

> TODO: 
> - Insert architecture illustration.

## Getting Started

It is possible to run the application independently of the other services in the microservice architecture (except the postgres database), however, only limited functionality will be available. 

### Configuration

#### Bot

I will recommend to create two bots on Discord - one for development and one for production. The development bot should only have access to discord servers created for testing.

Place the production bot token in `appsettings.Production.json` and the development bot token in `appsettings.Development.json`

#### Other

Change the logging configuration defined in `appsettings.json` if necessary.  

### Running the bot

Either run the project natively or use the provided `docker-compose.yml` to run it in Docker using the Docker Compose tool.

Clone the project: 
1. `git clone https://github.com/roedebaron/discord-bot.git`
2. `cd discord-bot/DiscordBot.Api`

#### Native

First, set up a postgres database for your platform. Then run the following commands: 

3. Start up the postgres database and make sure it is ready
4. Run `dotnet run`. This will automatically download all dependencies, build the project and then run the service.  
5. If no other port has been specified in the configuration, the service is now running on port 5000. 

#### Using Docker 🐳
3. Run `docker-compose up` to build the images and run the containers in attached mode
4. If no other port has been specified in the configuration, the service is now running on port 5000. 


> TODO: 
> - Setting own config/env values.
> - Link to other project dependencies. 
> - Link to docker-compose.yml 
> - Native + Docker command
 

## Usage 

> TODO:
> - Overview of all commands with link to example section
> - Example of all commands
> - Insert screenshot, demo gif, code examples... 

## Project TODO
- [ ] Push code to repo
- [ ] Refactor
- [ ] Migrate to latest Discord.NET package
- [ ] Migrate to .NET 5/C#9




