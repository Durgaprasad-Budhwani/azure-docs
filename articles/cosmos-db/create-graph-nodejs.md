---
title: Build an Azure Cosmos DB Node.js application using the Graph API | Microsoft Docs
description: Presents a Node.js code sample you can use to connect to and query Azure Cosmos DB
services: cosmosdb
documentationcenter: ''
author: mimig1
manager: jhubbard
editor: ''

ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmosdb
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/21/2017
ms.author: arramac

---
# Azure Cosmos DB: Build a Node.js application using the Graph API

Azure Cosmos DB is Microsoft’s globally distributed multi-model database service. You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB. 

This quick start demonstrates how to create an Azure Cosmos DB account for Graph API (preview), database, and graph using the Azure portal. You then build and run a console app using the OSS [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure) driver.  

> [!NOTE]
> The NPM module `gremlin-secure` is a modified version of `gremlin` module, with support for SSL and SASL required for connecting with Azure Cosmos DB. Source code is available on [Github](https://github.com/CosmosDB/gremlin-javascript).
>

## Prerequisites

* Before you can run this sample, you must have the following prerequisites:
	* [Node.js](https://nodejs.org/en/) version v0.10.29 or higher
	* [Git](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Create a database account

[!INCLUDE [cosmosdb-create-dbaccount-graph](../../includes/cosmosdb-create-dbaccount-graph.md)]

## Add a graph

[!INCLUDE [cosmosdb-create-graph](../../includes/cosmosdb-create-graph.md)]

## Clone the sample application

Now let's clone a Graph API app from github, set the connection string, and run it. You'll see how easy it is to work with data programmatically. 

1. Open a git terminal window, such as git bash, and `cd` to a working directory.  

2. Run the following command to clone the sample repository. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started.git
    ```

3. Then open the solution file in Visual Studio. 

## Review the code

Let's make a quick review of what's happening in the app. Open the `app.js` file and you'll find that these lines of code. 

* The Gremlin client is created.

    ```nodejs
    const client = Gremlin.createClient(
        443, 
        config.endpoint, 
        { 
            "session": false, 
            "ssl": true, 
            "user": `/dbs/${config.database}/colls/${config.collection}`,
            "password": config.primaryKey
        });
    ```

The configurations are all in `config.js`, which we edit in the following section.

* A series of Gremlin steps are executed using the `client.execute` method.

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

## Update your connection string

Now go back to the Azure portal to get your connection string information and copy it into the app.

1. In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Keys**, and then click **Read-write Keys**. You use the copy buttons on the right side of the screen to copy the URI and Primary Key into the `app.js` file in the next step.

    ![View and copy an access key in the Azure portal, Keys blade](./media/create-documentdb-dotnet/keys.png)

2. Copy your Gremlin URI value from the portal (using the copy button) and make it the value of `config.endpoint` key in config.js. The Gremlin endpoint must be only the host name without protocol/port number like `mygraphdb.graphs.azure.com` (NOT `https://mygraphdb.graphs.azure.com` or `mygraphdb.graphs.azure.com:433`).

    `config.endpoint = "GRAPHENDPOINT";`

3. Then copy your PRIMARY KEY value from the portal and make it the value of config.primaryKey in config.js. You've now updated your app with all the info it needs to communicate with Azure Cosmos DB. 

    `config.primaryKey = "PRIMARYKEY";`

4. Enter the database name, and graph (container) name for the value of config.database and config.collection. 

Here is an example of what your completed config.js file should look like:

```nodejs
var config = {}

// Note that this must not have HTTPS or the port number
config.endpoint = "mygraphdb.graphs.azure.com";
config.primaryKey = "OjlhK6tjxfSXyKtrmCiM9O6gQQgu5DmgAoauzD1PdPIq1LZJmILTarHvrolyUYOB0whGQ4j21rdAFwoYep7Kkw==";
config.database = "graphdb"
config.collection = "Persons"

module.exports = config;
```

## Run the console app

1. Open a terminal window and `cd` to a the installation directory for the package.json file included in the project.  

2. Run `npm install` to install required npm modules. This includes `gremlin-secure`.

3. Run `node app.js` in a terminal to start your node application.

You can now go back to Data Explorer and see query, modify, and work with this new data. 

## Browse using the Data Explorer

You can now go back to Data Explorer in the Azure portal and browse and query your new graph data.

* In Data Explorer, the new database appears in the Collections pane. Expand **graphdb**, **graphcoll**, and then click **Graph**.

    The data generated by the sample app is displayed in the Graphs pane.

## Review SLAs in the Azure portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## Clean up resources

If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps: 

1. From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created. 
2. On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.

## Next steps

In this quickstart, you've learned how to create an Azure Cosmos DB account, create a graph using the Data Explorer, and run an app. You can now build more complex queries and implement powerful graph traversal logic using Gremlin. 

> [!div class="nextstepaction"]
> [Query using Gremlin](tutorial-query-graph.md)