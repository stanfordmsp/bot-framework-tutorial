# Notesy: Recognize intents and entities with LUIS 

Workshop - Georgia Tech
By: Arvind Ramaswami (GT) Jhillika Kumar (GT) Kirtan Patel (GT) Agni Kumar (MIT) Racquel Butler (GSU) Ashwin Ramaswami (Stanford)
March 28, 2018
Slides: https://docs.google.com/presentation/d/1Vlhnj-0pzNtqU79tLroJoJJOzw1PB0VFm86zjsZ16FI/edit#slide=id.g28e4f442e9_0_39

<img src="https://user-images.githubusercontent.com/1689183/38057661-1fbee288-32ae-11e8-8a47-44b56507bd48.png" width="400">

This article uses the example of a bot for taking notes, to demonstrate how Language Understanding ([LUIS][LUIS]) helps your bot respond appropriately to natural language input. A bot detects what a user wants to do by identifying their **intent**. This intent is determined from spoken or textual input, or **utterances**. The intent maps utterances to actions that the bot takes, such as invoking a dialog. A bot may also need to extract **entities**, which are important words in utterances. Sometimes entities are required to fulfill an intent. In the example of a note-taking bot, the `Notes.Title` entity identifies the title of each note.

## Create a Language Understanding bot with Bot Service

1. In the [Azure portal](https://portal.azure.com), select **Create new resource** in the menu blade and click **See all**. (If you don't already have an Azure account, please go to http://imagine.microsoft.com/ and sign up with your student email. This gives you $100 of free credit to begin using the account.)

    ![Create new resource](https://user-images.githubusercontent.com/1689183/38473963-da82d8d0-3b4c-11e8-9dc0-4f68bf8e0b93.png)

2. In the search box, search for **Web App Bot**. 

    ![Create new resource](https://user-images.githubusercontent.com/1689183/38473965-dea454ca-3b4c-11e8-802b-e94803398e15.png)

3. In the **Bot Service** blade, provide the required information, and click **Create**. This creates and deploys the bot service and LUIS app to Azure. 
    * Set **App name** to your bot’s name. The name is used as the subdomain when your bot is deployed to the cloud (for example, mynotesbot.azurewebsites.net). This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot.
    * Select the subscription, [resource group](/azure/azure-resource-manager/resource-group-overview), App service plan, and [location](https://azure.microsoft.com/en-us/regions/).
    * Select the **Language understanding (Node.js)** template for the **Bot template** field.

![image](https://user-images.githubusercontent.com/1689183/38056201-dbbcca90-32a9-11e8-92a7-9aa7a4caed65.png)

4. Confirm that the bot service has been deployed.
    * Click Notifications (the bell icon that is located along the top edge of the Azure portal). The notification will change from **Deployment started** to **Deployment succeeded**.
    * After the notification changes to **Deployment succeeded**, click **Go to resource** on that notification.

## Try the bot

Confirm that the bot has been deployed by checking the **Notifications**. The notifications will change from **Deployment in progress...** to **Deployment succeeded**. Click **Go to resource** button to open the bot's resources blade.

Once the bot is registered, click **Test in Web Chat** to open the Web Chat pane. Type "hello" in Web Chat.

  ![Test the bot in Web Chat](https://user-images.githubusercontent.com/1689183/38473970-e3d786f6-3b4c-11e8-9814-853db6d9e764.png)

The bot responds by saying "You have reached Greeting. You said: hello". This confirms that the bot has received your message and passed it to a default LUIS app that it created. This default LUIS app detected a Greeting intent.

## Modify the LUIS app

Log in to [https://www.luis.ai](https://www.luis.ai) using the same account you use to log in to Azure. Scroll all the way down and click on **Create LUIS app** to get started. In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service. 

The LUIS app starts with 4 intents: Cancel, Greeting, Help, and None; and a fifth, custom intent you will add later! <!-- picture -->

### Add predefined intents
The following steps add the Note.Create, Note.ReadAloud, and Note.Delete intents: 

1. Click on **Prebuilt Domains** in the lower left of the page. Find the **Note** domain and click **Add domain**.

2. This tutorial doesn't use all of the intents included in the **Note** prebuilt domain. In the **Intents** page, click on each of the following intent names and then click the **Delete Intent** button.
   * Note.ShowNext
   * Note.DeleteNoteItem
   * Note.Confirm
   * Note.Clear
   * Note.CheckOffItem
   * Note.AddToNote

   The only intents that should remain in the LUIS app are the following: 
   * Note.ReadAloud
   * Note.Create
   * Note.Delete
   * None
   * Help
   * Greeting
   * Cancel 

    ![intents shown in LUIS app](https://user-images.githubusercontent.com/1689183/38473974-ea7bbb1c-3b4c-11e8-9e1a-272413495104.png)
### Add your own custom intent
Time to add your own custom intent! Click on "Create new intent" to create a new intent.

In this example, we are creating a new intent which lets the bot know if you want to drop out of school.

1. Click on "Create intent" and type "Dropout" as the intent name.
![image](https://user-images.githubusercontent.com/1689183/38036918-ce6cddfa-3275-11e8-8f31-dba7872cdf94.png)

1. Enter several examples of what the user may say in order for the bot to recognize the "Dropout" intent.
![image](https://user-images.githubusercontent.com/1689183/38037164-3fcf5bd0-3276-11e8-8571-de2ec7432d17.png)

After you've typed a few phrases, the screen should look like this:

![image](https://user-images.githubusercontent.com/1689183/38037106-27089a44-3276-11e8-9043-7690e5cfea17.png)

1. Finally, click on the "Train" button on the top right to train your bot. Then click on the "Test" button to test your intent. Type in a few phrases that have the same meaning as dropping out of school and see what the bot classifies it as. You can also try phrases such as "create a note" to see what intents it classifies your phrases as. You can see that the bot has varying levels of success with classifying intents; this may improve with better training data.

![image](https://user-images.githubusercontent.com/1689183/38037287-8e618fe8-3276-11e8-9a10-3bacd51b6d9a.png)

### Train and publish
Run the following steps; you will need to run these whenever you change your intents and configured phrases.
1.	Click the **Train** button in the upper right to train your app.
1.	Click **PUBLISH** in the top navigation bar to open the **Publish** page. Click the **Publish to production slot** button. And you're done!

## Modify the bot code

Back in the Azure portal, click **Build** and then click **Open online code editor**.

   ![Open online code editor](https://user-images.githubusercontent.com/1689183/38473979-f039b784-3b4c-11e8-9fad-b9e48c0dd489.png)

In the code editor, open `app.js`. It contains the following code:

```javascript
/*-----------------------------------------------------------------------------
A simple Language Understanding (LUIS) bot for the Microsoft Bot Framework. 
-----------------------------------------------------------------------------*/

var restify = require('restify');
var builder = require('botbuilder');
var botbuilder_azure = require("botbuilder-azure");

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword,
    openIdMetadata: process.env.BotOpenIdMetadata 
});

// Listen for messages from users 
server.post('/api/messages', connector.listen());

/*----------------------------------------------------------------------------------------
* Bot Storage: This is a great spot to register the private state storage for your bot. 
* We provide adapters for Azure Table, CosmosDb, SQL Azure, or you can implement your own!
* For samples and documentation, see: https://github.com/Microsoft/BotBuilder-Azure
* ---------------------------------------------------------------------------------------- */

var tableName = 'botdata';
var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

// Create your bot with a function to receive messages from the user
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send('You reached the default message handler. You said \'%s\'.', session.message.text);
});

bot.set('storage', tableStorage);

// Make sure you add code to validate these fields
var luisAppId = process.env.LuisAppId;
var luisAPIKey = process.env.LuisAPIKey;
var luisAPIHostName = process.env.LuisAPIHostName || 'westus.api.cognitive.microsoft.com';

const LuisModelUrl = 'https://' + luisAPIHostName + '/luis/v2.0/apps/' + luisAppId + '?subscription-key=' + luisAPIKey;

// Create a recognizer that gets intents from LUIS, and add it to the bot
var recognizer = new builder.LuisRecognizer(LuisModelUrl);
bot.recognizer(recognizer);

// Add a dialog for each intent that the LUIS app recognizes.
// See https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-recognize-intent-luis 
bot.dialog('GreetingDialog',
    (session) => {
        session.send('You reached the Greeting intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Greeting'
})

bot.dialog('HelpDialog',
    (session) => {
        session.send('You reached the Help intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Help'
})

bot.dialog('CancelDialog',
    (session) => {
        session.send('You reached the Cancel intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Cancel'
})  
```


> [!TIP] 
> You can also find the sample code described in this article in the [Notes bot sample][NotesSample].



## Handle the None intent
The [matches][intentDialog_matches] methods on the [IntentDialog][intentDialog] invokes a handler when the specified intent is detected in the user utterance. To add a handler for the `None` intent, copy the following code and paste it at the end of the file.
```javascript
bot.dialog('NoneDialog',
    (session) => {
        session.send("Hi... I'm the note bot sample. I can create new notes, read saved notes to you and delete notes.");

       // If the object for storing notes in session.userData doesn't exist yet, initialize it
       if (!session.userData.notes) {
           session.userData.notes = {};
           console.log("initializing userData.notes in default message handler");
       }
    }
).triggerAction({
    matches: 'None'
});
```

## Handle the Note.Create intent

Copy the following code and paste it after the handler for None that you just pasted:
```javascript
bot.dialog('NoteCreateDialog', [(session, args, next) => {
        // Resolve and store any Note.Title entity passed from LUIS.
        var intent = args.intent;
        var title = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');

        var note = session.dialogData.note = {
          title: title ? title.entity : null,
        };
        
        // Prompt for title
        if (!note.title) {
            builder.Prompts.text(session, 'What would you like to call your note?');
        } else {
            next();
        }
    },
    (session, results, next) => {
        var note = session.dialogData.note;
        if (results.response) {
            note.title = results.response;
        }

        // Prompt for the text of the note
        if (!note.text) {
            builder.Prompts.text(session, 'What would you like to say in your note?');
        } else {
            next();
        }
    },
    (session, results) => {
        var note = session.dialogData.note;
        if (results.response) {
            note.text = results.response;
        }
        
        // If the object for storing notes in session.userData doesn't exist yet, initialize it
        if (!session.userData.notes) {
            session.userData.notes = {};
            console.log("initializing session.userData.notes in CreateNote dialog");
        }
        // Save notes in the notes object
        session.userData.notes[note.title] = note;

        // Send confirmation to user
        session.endDialog('Creating note named "%s" with text "%s"',
            note.title, note.text);
    }]
).triggerAction({
    matches: 'Note.Create'
});
```

Any entities in the utterance are passed to the dialog using the `args` parameter. The first step of the [waterfall][waterfall] calls [EntityRecognizer.findEntity][EntityRecognizer_findEntity] to get the title of the note from any `Note.Title` entities in the LUIS response. If the LUIS app didn't detect a `Note.Title` entity, the bot prompts the user for the name of the note. The second step of the waterfall prompts for the text to include in the note. Once the bot has the text of the note, the third step uses [session.userData][session_userData] to save the note in a `notes` object, using the title as the key. For more information on `session.UserData` see [Manage state data](./bot-builder-nodejs-state.md). 



## Handle the Note.Delete intent
Just as for the `Note.Create` intent, the bot examines the `args` parameter for a title. If no title is detected, the bot prompts the user. The title is used to look up the note to delete from `session.userData.notes`. 



Copy the following code and paste it after the handler for `Note.Create` that you just pasted:
```javascript
bot.dialog('NoteDeleteDialog', [(session, args, next) => {
        if (noteCount(session.userData.notes) > 0) {
            // Resolve and store any Note.Title entity passed from LUIS.
            var title;
            var intent = args.intent;
            var entity = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');
            if (entity) {
                // Verify that the title is in our set of notes.
                title = builder.EntityRecognizer.findBestMatch(session.userData.notes, entity.entity);
            }
            
            // Prompt for note name
            if (!title) {
                builder.Prompts.choice(session, 'Which note would you like to delete?', session.userData.notes);
            } else {
                next({ response: title });
            }
        } else {
            session.endDialog("No notes to delete.");
        }
    },
    (session, results) => {
        delete session.userData.notes[results.response.entity];        
        session.endDialog("Deleted the '%s' note.", results.response.entity);
    }]
    ).triggerAction({
    matches: 'Note.Delete'
});
```

## Handle the Note.ReadAloud intent

Copy the following code and paste it after the handler for `Note.Create` that you just pasted:

```javascript
bot.dialog('NoteReadAloudDialog', [(session, args, next) => {
        if (noteCount(session.userData.notes) > 0) {
           
            // Resolve and store any Note.Title entity passed from LUIS.
            var title;
            var intent = args.intent;
            var entity = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');
            if (entity) {
                // Verify it's in our set of notes.
                title = builder.EntityRecognizer.findBestMatch(session.userData.notes, entity.entity);
            }
            
            // Prompt for note name
            if (!title) {
                builder.Prompts.choice(session, 'Which note would you like to read?', session.userData.notes);
            } else {
                next({ response: title });
            }
        } else {
            session.endDialog("No notes to read.");
        }
    },
    (session, results) => {        
        session.endDialog("Here's the '%s' note: '%s'.", results.response.entity, session.userData.notes[results.response.entity].text);
    }]
).triggerAction({
    matches: 'Note.ReadAloud'
});
```

## Add the `noteCount` helper function

The code that handles `Note.Delete` uses the `noteCount` function to determine whether the `notes` object contains notes. 

Paste the `noteCount` helper function at the end of `app.js`:
```javascript
function noteCount(notes) {

    var i = 0;
    for (var name in notes) {
        i++;
    }
    return i;
}
```

## Handle your custom dropout intent
Now it's time to handle your custom dropout intent! Note that ```session.endDialog``` gives the bot's response. The following code is an example of some code that helps pick a random response, once the bot understands that the user wants to drop out. Feel free to tweak this code or make it more complex!
```
bot.dialog('DropoutDialog', (session) => {
        var possibleResponses = [
          "Oh no! Don't drop out of school, it's worth it!",
          "Dropping out of school is a terrible decision.",
          "Don't worry! I'll support you!",
          "If Bill Gates dropped out of school, you can too!"
        ];
        var response = possibleResponses[Math.floor(Math.random() * possibleResponses.length)];
        if (noteCount(session.userData.notes) > 0) {
            response += " You even have " + noteCount(session.userData.notes) + "notes!";
        }
        session.endDialog(response);
    }
).triggerAction({
    matches: 'Dropout'
});
```

The `session.userData.notes` object is passed as the third argument to `builder.Prompts.choice`, so that the prompt displays a list of notes to the user.

Now that you've added handlers for the new intents, the full code for `app.js` contains the following:

```javascript
/*-----------------------------------------------------------------------------
A simple Language Understanding (LUIS) bot for the Microsoft Bot Framework. 
-----------------------------------------------------------------------------*/

var restify = require('restify');
var builder = require('botbuilder');
var botbuilder_azure = require("botbuilder-azure");

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword,
    openIdMetadata: process.env.BotOpenIdMetadata 
});

// Listen for messages from users 
server.post('/api/messages', connector.listen());

/*----------------------------------------------------------------------------------------
* Bot Storage: This is a great spot to register the private state storage for your bot. 
* We provide adapters for Azure Table, CosmosDb, SQL Azure, or you can implement your own!
* For samples and documentation, see: https://github.com/Microsoft/BotBuilder-Azure
* ---------------------------------------------------------------------------------------- */

var tableName = 'botdata';
var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

// Create your bot with a function to receive messages from the user
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send('You reached the default message handler. You said \'%s\'.', session.message.text);
});

bot.set('storage', tableStorage);

// Make sure you add code to validate these fields
var luisAppId = process.env.LuisAppId;
var luisAPIKey = process.env.LuisAPIKey;
var luisAPIHostName = process.env.LuisAPIHostName || 'westus.api.cognitive.microsoft.com';

const LuisModelUrl = 'https://' + luisAPIHostName + '/luis/v2.0/apps/' + luisAppId + '?subscription-key=' + luisAPIKey;

// Create a recognizer that gets intents from LUIS, and add it to the bot
var recognizer = new builder.LuisRecognizer(LuisModelUrl);
bot.recognizer(recognizer);

// Add a dialog for each intent that the LUIS app recognizes.
// See https://docs.microsoft.com/en-us/bot-framework/nodejs/bot-builder-nodejs-recognize-intent-luis 
bot.dialog('GreetingDialog',
    (session) => {
        session.send('You reached the Greeting intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Greeting'
})

bot.dialog('HelpDialog',
    (session) => {
        session.send('You reached the Help intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Help'
})

bot.dialog('CancelDialog',
    (session) => {
        session.send('You reached the Cancel intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Cancel'
})

bot.dialog('NoneDialog',
    (session) => {
        session.send("Hi... I'm the note bot sample. I can create new notes, read saved notes to you and delete notes.");

       // If the object for storing notes in session.userData doesn't exist yet, initialize it
       if (!session.userData.notes) {
           session.userData.notes = {};
           console.log("initializing userData.notes in default message handler");
       }
        session.endDialog();
    }
).triggerAction({
    matches: 'None'
});

bot.dialog('NoteCreateDialog', [(session, args, next) => {
        // Resolve and store any Note.Title entity passed from LUIS.
        var intent = args.intent;
        var title = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');

        var note = session.dialogData.note = {
          title: title ? title.entity : null,
        };
        
        // Prompt for title
        if (!note.title) {
            builder.Prompts.text(session, 'What would you like to call your note?');
        } else {
            next();
        }
    },
    (session, results, next) => {
        var note = session.dialogData.note;
        if (results.response) {
            note.title = results.response;
        }

        // Prompt for the text of the note
        if (!note.text) {
            builder.Prompts.text(session, 'What would you like to say in your note?');
        } else {
            next();
        }
    },
    (session, results) => {
        var note = session.dialogData.note;
        if (results.response) {
            note.text = results.response;
        }
        
        // If the object for storing notes in session.userData doesn't exist yet, initialize it
        if (!session.userData.notes) {
            session.userData.notes = {};
            console.log("initializing session.userData.notes in CreateNote dialog");
        }
        // Save notes in the notes object
        session.userData.notes[note.title] = note;

        // Send confirmation to user
        session.endDialog('Creating note named "%s" with text "%s"',
            note.title, note.text);
    }]
).triggerAction({
    matches: 'Note.Create'
});

bot.dialog('NoteDeleteDialog', [(session, args, next) => {
        if (noteCount(session.userData.notes) > 0) {
            // Resolve and store any Note.Title entity passed from LUIS.
            var title;
            var intent = args.intent;
            var entity = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');
            if (entity) {
                // Verify that the title is in our set of notes.
                title = builder.EntityRecognizer.findBestMatch(session.userData.notes, entity.entity);
            }
            
            // Prompt for note name
            if (!title) {
                builder.Prompts.choice(session, 'Which note would you like to delete?', session.userData.notes);
            } else {
                next({ response: title });
            }
        } else {
            session.endDialog("No notes to delete.");
        }
    },
    (session, results) => {
        delete session.userData.notes[results.response.entity];        
        session.endDialog("Deleted the '%s' note.", results.response.entity);
    }]
    ).triggerAction({
    matches: 'Note.Delete'
});

bot.dialog('NoteReadAloudDialog', [(session, args, next) => {
        if (noteCount(session.userData.notes) > 0) {
           
            // Resolve and store any Note.Title entity passed from LUIS.
            var title;
            var intent = args.intent;
            var entity = builder.EntityRecognizer.findEntity(intent.entities, 'Note.Title');
            if (entity) {
                // Verify it's in our set of notes.
                title = builder.EntityRecognizer.findBestMatch(session.userData.notes, entity.entity);
            }
            
            // Prompt for note name
            if (!title) {
                builder.Prompts.choice(session, 'Which note would you like to read?', session.userData.notes);
            } else {
                next({ response: title });
            }
        } else {
            session.endDialog("No notes to read.");
        }
    },
    (session, results) => {        
        session.endDialog("Here's the '%s' note: '%s'.", results.response.entity, session.userData.notes[results.response.entity].text);
    }]
).triggerAction({
    matches: 'Note.ReadAloud'
});

function noteCount(notes) {

    var i = 0;
    for (var name in notes) {
        i++;
    }
    return i;
}

bot.dialog('DropoutDialog', (session) => {
        var possibleResponses = [
          "Oh no! Don't drop out of school, it's worth it!",
          "Dropping out of school is a terrible decision.",
          "Don't worry! I'll support you!",
          "If Bill Gates dropped out of school, you can too!"
        ];
        var response = possibleResponses[Math.floor(Math.random() * possibleResponses.length)];
        if (noteCount(session.userData.notes) > 0) {
            response += " You even have " + noteCount(session.userData.notes) + "notes!";
        }
        session.endDialog(response);
    }
).triggerAction({
    matches: 'Dropout'
});

```

## Test the bot

In the Azure Portal, click on **Test in Web Chat** to test the bot. Try type messages like "Create a note", "read my notes", and "delete notes" to invoke the intents that you added to it.
   ![Test notes bot in Web Chat](https://user-images.githubusercontent.com/1689183/38473982-f6483fa6-3b4c-11e8-810b-e809abfac64e.png)

And, you can also try your custom intent that you had created before:

![image](https://user-images.githubusercontent.com/1689183/38038133-58631fe0-3278-11e8-83b5-67c2e10ae20f.png)

> [!TIP]
> If you find that your bot doesn't always recognize the correct intent or entities, improve your LUIS app's performance by giving it more example utterances to train it. You can retrain your LUIS app without any modification to your bot's code. See [Add example utterances](/azure/cognitive-services/LUIS/add-example-utterances) and [train and test your LUIS app](/azure/cognitive-services/LUIS/train-test).

## Share and deploy your bot!
### Deploy by web channels

Get your bot secret key
Open your bot in the Azure Portal and click Channels blade.

1. Click Edit for the Web Chat channel.
![image](https://user-images.githubusercontent.com/1689183/38038698-932e53fa-3279-11e8-8d5f-546022598337.png)

1. Under Secret keys, click Show for the first key.
![image](https://user-images.githubusercontent.com/1689183/38038702-973e0bca-3279-11e8-9b60-cbf70f207e3a.png)


1. Copy the Secret key and the Embed code.

1. Click Done.

### Embed your bot in a website

To embed your bot in your website by specifying the secret within the iframe tag:

1. Copy the iframe Embed code from the Web Chat channel within the Bot Framework Portal (as described in the steps above).

1. Within that Embed code, replace "YOUR_SECRET_HERE" with the Secret key value that you copied from the same page.

```
<iframe src="https://webchat.botframework.com/embed/YOUR_BOT_ID?s=YOUR_SECRET_HERE"></iframe>
```

1. You can now add the above HTML code to any website (such as your personal website) to embed the bot. You can also just go to the URL `https://webchat.botframework.com/embed/YOUR_BOT_ID?s=YOUR_SECRET_HERE` in your web browser to interact with your bot -- share this link with others so they can use your bot as well!

Here's a finished example: **https://webchat.botframework.com/embed/arvind-note-bot?s=uvrjVpZN9OI.cwA.qNg.bKim9V6FrD-I1d3fz1b2WIjfhTiTtCFSD819e-QBvfM**

### Integrating your bot with Facebook, Slack, Skype, email, and more
Each platform requires a specific setup and registration with developer accounts. Read this tutorial for more information about how to integrate your bot with other platforms: **https://docs.microsoft.com/en-us/azure/bot-service/bot-service-channel-connect-slack**

Note: as of now, Facebook is not allowing new Messenger bots to be created while they update their policies.


## Next steps

From trying the bot, you can see that the recognizer can trigger interruption of the currently active dialog. Allowing and handling interruptions is a flexible design that accounts for what users really do. Learn more about the various actions you can associate with a recognized intent.

> [!div class="nextstepaction"]
> [Handle user actions](bot-builder-nodejs-dialog-actions.md)


[LUIS]: https://www.luis.ai/

[intentDialog]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html

[intentDialog_matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html#matches 

[NotesSample]: https://github.com/Microsoft/BotFramework-Samples/tree/master/docs-samples/Node/basics-naturalLanguage

[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[confirmPrompt]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#confirmprompt

[waterfall]: bot-builder-nodejs-dialog-manage-conversation-flow.md#manage-conversation-flow-with-a-waterfall

[session_userData]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#userdata

[EntityRecognizer_findEntity]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.entityrecognizer.html#findentity

[matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#matches

[LUISAzureDocs]: /azure/cognitive-services/LUIS/Home

[Dialog]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html

[IntentRecognizerSetOptions]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iintentrecognizersetoptions.html

[LuisRecognizer]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.luisrecognizer

[LUISConcepts]: https://docs.botframework.com/en-us/node/builder/guides/understanding-natural-language/

[DisambiguationSample]: https://github.com/Microsoft/BotBuilder/tree/master/Node/examples/feature-onDisambiguateRoute

[IDisambiguateRouteHandler]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.idisambiguateroutehandler.html

[RegExpRecognizer]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.regexprecognizer.html

[AlarmBot]: https://github.com/Microsoft/BotBuilder/blob/master/Node/examples/basics-naturalLanguage/app.js

[LUISBotSample]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-LUIS

[UniversalBot]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.universalbot.html
