<p align="center">
  <img width="100" src="https://pnggrid.com/wp-content/uploads/2021/05/Discord-Logo-Circle-768x768.png">  
</p>

<h1 align="center">Discord Bot</h1>

<p align="center">
  <img src="https://img.shields.io/badge/discord--net--labs-v3.1.7-blue" alt="discord-net" style="max-width:100%;">
  <img src="https://img.shields.io/badge/C%23-8.0-blue" alt="csharp" style="max-width:100%;"> 
  <img src="https://img.shields.io/badge/.NET Core-3.1-blue" alt="csharp" style="max-width:100%;"> 
  <img src="https://img.shields.io/badge/IDE-VS2019-purple" alt="ide" style="max-width:100%;">
</p>

<p align="center">
  Discord bot template that makes it possible to run multiple different Discord bots under a single bot in a Docker based microservice architecture managed by Docker Compose. Written in C# using Discord.NET.
</p>

## About
 
To obtain a seperation of concerns, it is tempting to seperate different functionality into multiple different Discord bots in a microservice architecture. This projects acts as a template/library that allows to easely share common implementation, such as messaging, configuration, audio playing, help commands etc., between multiple different bots and at the same time make the bots appear as a single bot in the Discord UI client. In this way, it is not neccessary to add all the different bots to a guild, as only one bot need to be added. 

To summarize, the template bot make it possible to: 
- Share common Discord bot functionality between multiple bots 
- Make all bots appear as a single bot

This template is used by my other discord bot projects: 
- [image-caption-ai](https://github.com/roedebaron/image-caption-ai)
- [aau-schedule-scraping](https://github.com/roedebaron/aau-schedule-scraping)
- [google-drive-service](https://github.com/roedebaron/google-drive-service)
- [corona-stats](https://github.com/roedebaron/corona-stats)
- [reddit-service](https://github.com/roedebaron/reddit-service)
- [chart-service](https://github.com/roedebaron/chart-service)

## Features
- A global `!help` command that auto-generates a message giving an overview of all command categories.
- Auto-generated help messages for all command categories. Simply call `!help <command_category`.
- Audio player with search functionality that dynamically loads and play audio files placed in a given internal folder for each bot.
- Sending status updates in a specified guild only for the developer e.g. a message with bot name and version number at startup of each bot. ,
- Registration and identification of all services in the microservice architecture. This ensures only a single bot answers the global `!help` command.
- Docker and Docker Swarm commands for the Developer that provides an overview of the docker containers and VM's running in the microservice arhitecture.
- Disposing of services according to their specified scopes after command has been executed (...as they should, but apparently Discord.NET is not doing this for you ATM)
- Automatic deletion of command and bot messages after a custom default time period.
- Set custom command prefix for every guild and what it should default to for new guilds.
- Database integration that can be extended with custom entities. 


### Feature: Global help command

A command category is only included in the global help command, if the command module class representing the category is decorated with the `Group` and `Summary` attributes. 
`Group` should specify the category name. `Summary` should give a description of the category. As such, a category can be excluded from the overview e.g. by not decorating it with a `Summary` attribute. 

--INSERT EXAMPLE IMAGE HERE

### Feature: Auto-generated help command for each command category

Required:
- A help message for all commands in a category is only provided if the command module class representing the category is decorated with the `Group` and `Summary` attributes.
- A command is only included in the help message if it is decorated with the `Summary` attribute. The content of the `Summary` attribute for each command should give a description of the command. The description is included just below the command call specification.

Optional:
- If the command module is decorated with the `Alias` attribute, the aliases will be included in the top of the help message together with the default command name.
- If the command module is decorated with the `Remarks` attribute, the content will be included in the top of the help message after aliases. 
- The command call specification will be displayed with command parameters in *snake_case*. This can be overriden by a custom parameter specification by decorating the individual command with a `Summary` attribute containing the custom parameter text.
- If a command is decorated with the `Remarks` attribute, the content will be included after the command description. 


The commands will appear in the help message following the order they are declared in the command module class. 

All this behaviour can be overridden for a specific command category by decorating your own custom help command with a `Priority` attribute of 1 or higher. 

--INSERT EXAMPLE IMAGES HERE

### Feature: Audioplayer with search funtionality
- Simply specify the name of the folder that contains audio when configuring the bot. Audio can now be played using the `AudioCient` as a dependency in a command module.
- Use the XXX method to get the audio file that best matches the requested audio name according to the order: 'exact match', 'starts with' and 'contains'.
- Optimized by using an internal cache of sounds available in order to minimize I/O. It is possible to request a cache update if new sounds are added on runtime. Simply call `GetAvailableAudioNames` with the optional parameter `updateCache` set to `true`.

--LINK TO EXAMPLE PROJECT ON HOW TO USE--

### Feature: Automatic deletion of messages
It is possibly to specify a default delay for when messages should be deleted. This feature applies to both the command messages issued by the user and the replies issued by the bot. Futhermore, it is also possible to override the default and specify a custom deletion delay for each individual reply if desired. Of course, replies can also be persistent.   


## Getting Started

The idea is that all bot projects reference the `TemplateBot` project and create an instance of the `DiscordBot` class, which also means that they will share the same bot token.

However, certain functionality must be provided by one bot only e.g. a global `!help` command. To agree on the bot which will react on `!help` a bot is promoted to **main bot** whereas the other bots will be **secondary** bots. It is the responsibility of the **main bot** to generate a help message that contains all command categories from all bots in the microservice architecture. This is ensured by letting all **secondary bots** register their command categories at the main bot at startup.  

## Architecture

The internal architecture is based on the MVC-pattern following the principles from Domain-Driven Design (DDD). 

A brief overview of the layers: 

**Core (Model)**: Business logic. Represents domain objects and services. 

**Messages (View)**: Represents the Discord messages to be returned to a Discord guild i.e. a "View" is simply a formatted string to be interpreted by the Discord UI client. 

**Commands (Controller)**: Recieves incoming Discord commands which are then delegated to **Core**. The result from Core is represented as a Discord message (a View) and returned to the client.  

**Infrastructure**: Data persistence and external web services. 

> TODO: 
> - Insert architecture illustration.
> - Update to new arhitecture

## Getting Started

### Configuration

Please see the folder `Config_Example` for an example of the required configuration files and values. Here you will find configuration files for the Production and Development environment as well Docker and Localhost. Rename the folder to `Config` in order to load the configuration files at startup. 

#### Bot Types
- Create one **main** bot and in its' `Config/appsettings.json` set `IsMainBot: true` 
- Create zero to many **secondary** bots and in its' `Config/appsettings.json` set `IsMainBot: false` 

#### Bot Token

I recommend creating two bots on Discord - one for development and one for production. The development bot should only have access to Discord guilds created for testing.

Place the production bot token in `appsettings.Production.json` and the development bot token in `appsettings.Development.json`:

```JSONC
"Discord": {
    "BotToken": "YOUR_BOT_TOKEN"
}
```

#### Database

The bot uses a local SQLite database for development. You should use a database server for production such as PostgreSQL. Insert the connection string for the production database in `appsettings.Production.json`.

```JSONC
"DbConnectionString": "YOUR_CONNECTION_STRING"
```

#### Logging

The project uses [Serilog](https://github.com/serilog/serilog) for logging. Change the logging configuration defined in `appsettings.json` or you can leave it as is. 

It is possible to log using Elastic Stack (Elastic Search, Kibnana and Beats) - Dockerfiles are included. 

#### Dashboard

Currently, a dashboard for the Discord bot is available in Kibana as a custom dashboard that reads log messages from the bot and display useful statistics. 

### Running the bot

> :warning: Remember to set up configuration first!

Either run the project from source or use the provided `docker-compose.yml` to run it in Docker using the Docker Compose tool.

Clone the project: 
1. `git clone https://github.com/roedebaron/discord-bot.git`
2. `cd template-bot/TemplateBot`

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
- [ ] Migrate to .NET 6/C#10
- [ ] Push code to repo
- [ ] Refactor





