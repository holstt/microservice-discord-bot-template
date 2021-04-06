# Discord Bot
![discord-net](https://img.shields.io/badge/discord--net-v2.2.0-blue)

All-around Discord bot written in C# using Discord.NET (new repo) 

## About

The internal architecture is based on the MVC-pattern following the principles from Domain-Driven Design (DDD). 

The Discord bot is part of a microservice architecture managed by Docker Compose and depends on multiple other services in order to execute its' functionality - please see my other projects. 

A brief overview of the layers: 

**Core**: Business logic. Represents domain objects and services. 

**View**: Represents the Discord messages to be returned to a Discord guild i.e. a "View" is simply a formatted string to be interpreted by the Discord client. 

**Controller**: Recieves incoming Discord commands which are then delegated to **Core**. The result from Core is represented as a Discord message (a View) and returned to the client.  

**Infrastructure**: Data persistence and external web services. 

> TODO: 
> - Insert architecture illustration.

## Getting Started

It is possible to run the application independently of the other services in the microservice architecture (except the postgres database), however, only limited functionality will be available. 

Either run the project natively or use the provided Dockerfile.

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
- [ ] Migrate to .NET 5




