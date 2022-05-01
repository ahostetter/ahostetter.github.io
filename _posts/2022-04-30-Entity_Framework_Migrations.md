---
layout: post
title:  "Entity Framework Migrations"
date:   2022-04-30 08:00:00 -0400
categories: jekyll update
---
<h1 style="text-align: center;">Entity Framework Migrations</h1>

 Source: [Model Scaffolding with Entity Framework][Model Scaffolding with Entity Framework]

I was struggling with Visual Studios not Scaffolding my new SubCategory Model. I googled it and found that some had to run the CLI command to get it to run for them. This worked for me as well. This is the link: [dotnet-aspnet-codegenerator][dotnet-aspnet-codegenerator]

### Command I used:
`dotnet aspnet-codegenerator controller -m SubCategory -dc Personal_InventoryContext -name SubCategoriesController -async`
### If it needs updated use this command:
`dotnet tool update -g dotnet-aspnet-codegenerator`

[dotnet-aspnet-codegenerator]: https://docs.microsoft.com/en-us/aspnet/core/fundamentals/tools/dotnet-aspnet-codegenerator?view=aspnetcore-6.0

[Model Scaffolding with Entity Framework]: https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-6.0&tabs=visual-studio
