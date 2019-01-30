# v3NodeBingSearchSDK-bot
This is v3 Node.js bot app that invokes Bing Search service via *azure-cognitiveservices-websearch* SDK.


## Packages to install
The following are the npm packages that are installed.
```
    "azure-cognitiveservices-websearch": "^2.0.0",
    "botbuilder": "^3.15.0",
    "dotenv": "^6.2.0",
    "ms-rest-azure": "^2.6.0",
    "restify": "^7.6.0"
```

## Code changes discussion
In order to invoke the Bing Search API, utilized the **azure-cognitiveservices-websearch** npm package. The request/ response management is more streamlined by this. The source code can be found [here](https://github.com/Azure/azure-sdk-for-node/blob/master/lib/services/cognitiveServicesWebSearch/lib/webSearchClient.d.ts), and the repo [here](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesWebSearch).


```
var websearchAPIClient = require('azure-cognitiveservices-websearch');
var msrestazure = require('ms-rest-azure');
..
..
        // pass Bing Search API key, and get a new object for type CognitiveServicesCredentials
        var credentials = new msrestazure.CognitiveServicesCredentials(process.env.BingSeachApiKey);

        // pass CognitiveServicesCredentials, get a new search client object
        var searchAPIClient = new websearchAPIClient(credentials);

        searchAPIClient.web.search(session.message.text)
            .then((result) => {
                var properties = ["news"]; //["images", "webPages", "news"];
                for (let index = 0; index < properties.length; index++) {
                    if (result[properties[index]]){
                        //console.log(result[properties[index]].value);
                        var botResults = result[properties[index]].value
                                                .map(item => item.name + ' :: ' + item.url);
                        session.endDialog(JSON.stringify(botResults));
                    } else {
                        console.log(`No ${properties[index]} data`);
                    }               
                }
            }).catch((err) => {
            console.log(err);
        }); 
```

## How to run?
You can clone the repo. And, run command **npm install** in order to install the dependency packages. Then, in order to run the app, just say *node app.js*.
