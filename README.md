<p align="center">
  <img width="100" src="https://pnggrid.com/wp-content/uploads/2021/05/Discord-Logo-Circle-768x768.png">  
</p>

<h1 align="center">Discord Bot</h1>

<p align="center">
  <img src="https://img.shields.io/badge/discord--net-v2.3.1-blue" alt="discord-net" style="max-width:100%;">
  <img src="https://img.shields.io/badge/C%23-9.0-blue" alt="csharp" style="max-width:100%;"> 
    <img src="https://img.shields.io/badge/.NET-5.0-blue" alt="csharp" style="max-width:100%;"> 
  <img src="https://img.shields.io/badge/IDE-vs2019-blue" alt="ide" style="max-width:100%;">
</p>

<p align="center">
  Discord bot template written in C# using Discord.NET.
</p>

## About
 
To obtain a seperation of concerns, it is tempting to seperate different functionality into multiple discord bots. This projects acts as a template that allows to easely integrate multiple different bots into one, making it possible to share common behavior such as messaging, logging etc. 

The idea is to define the commands, business logic, dependencies etc. for each bot in its' own project. Integrating the bot into the template only requires registering the bot's assembly in the configuration file of the template and provide a way for the template to register the bot's dependencies into the service container.

This template is used by my other discord bot projects: 

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

Please see the `appsettings.Example.json` file for an example of the required configuration values. Rename the file to `appsettings.Production.json`. If you wish to develop on the bot, create a copy named `appsettings.Development.json`. 


#### Bot Token

I will recommend to create two bots on Discord - one for development and one for production. The development bot should only have access to discord servers created for testing.

Place the production bot token in `appsettings.Production.json` and the development bot token in `appsettings.Development.json`:

```JSONC
"Discord": {
    "BotToken": "YOUR_BOT_TOKEN"
}
```

#### Bot Integration

Add the absolute or relative assembly path of all bots you wish to integrate with the template bot in the configuration file.

```JSONC
"AssemblyPaths": [
    "..\\..\\MyDiscordBotProject\\MyDiscordBot\\bin\\Debug\\net5.0\\BotExtensions.dll" // Example on relative path
]
```


#### Database

The bot uses a local SQLite database for development. You should use a database server for production such as PostgreSQL. Insert the connection string for the production database in `appsettings.Production.json`.

```JSONC
"DbConnectionString": "YOUR_CONNECTION_STRING"
```

#### Logging

The project uses [Serilog](https://github.com/serilog/serilog) for logging. Change the logging configuration defined in `appsettings.json` or you can leave it as is. 

It is possible to log using Elastic Stack (Elastic Search, Kibnana and Beats) - dockerfiles are included. 

#### Dashboard

Currently, a dashboard for the Discord bot is available in Kibana as a custom dashboard that reads log messages from the bot and display useful statistics. 

### Running the bot

> :warning: Remember to set up configuration first!

Either run the project from source or use the provided `docker-compose.yml` to run it in Docker using the Docker Compose tool.

Clone the project: 
1. `git clone https://github.com/roedebaron/discord-bot.git`
2. `cd discord-bot/DiscordBot.Api`

#### Running from source

First, set up a postgres database for your platform. Then run the following commands: 

3. Start up the postgres database and make sure it is ready
4. Run `dotnet run`. This will automatically download all dependencies, build the project and then run the service.  
5. If no other port has been specified in the configuration, the service is now running on port 5000. 

#### Running with Docker Compose 🐳
3. Run `docker-compose up` to build the images and run the containers in attached mode.
4. If no other port has been specified in the configuration, the service is now running on port 5000. 


> TODO: 
> - Setting own config/env values.
> - Link to other project dependencies. 
> - Link to docker-compose.yml 
 

## Usage 

> TODO:
> - Overview of all commands with link to example section
> - Example of all commands
> - Insert screenshot, demo gif, code examples... 

## Project TODO
- [x] Migrate to latest Discord.NET package
- [x] Migrate to .NET 5/C#9
- [ ] Push code to repo
- [ ] Refactor





