---
layout: post
---
The idea of chatbot is nothing new, but has become the commercial focus of many messaging services. Take Facebook Messenger as an example, they recently launched their own [bot platform](http://www.theverge.com/2016/4/12/11395806/facebook-messenger-bot-platform-announced-f8-conference) to rival the dominating Slackbot in the market. The trend shows high interest in chatbots by the general public nowadays, and there is a reason for it - creating a customized chatbot specific to the need an individual or organization can greatly improve productivity.

I did not see the merits of a chatbot until I built one for [UCLA ACM](http://acm.cs.ucla.edu), the largest computer science student group on campus. As an officer, I sometimes find it difficult to keep up with all the events due to our large group size (40~50 people). Even though we use Slack as the communication channel (which has many nice features for grouping), information can still be easily lost and many repetitive chores are often distributed among the officers. These are the problems that a chatbot can solve: keep track of information and manage repetitive works.

Features Planning
-----------------
It is important to understand the use case of the chatbot early. At UCLA ACM, we had to manually perform the following tasks:

- create sign-in sheets for events
- remind committee chairs to submit newsletter events
- check whether our student lounge is open
- search for resource links
- and more...

These are the sort of tasks that can be resolved with a combination of software and hardware approaches. Our Slackbot is, therefore, designed to eliminate human involvement in tedious activities. 

Planning features before the construction phase allows me to develop a robust dispatcher for our Slackbot. By sorting distinct features into independent sets, the dispatcher can handle user queries effectively and allows me to achieve better modularization accordingly. Here is part of our Slackbot directory structure:
	
	dispatch.js
	scripts/
	  signin.js
	  reminder.js
	  ai.js
	  sms.js
	  util.js

Personification
---------------
The chatbot has to act like a person, even though its main goal is to achieve automation. Since I developed the bot in JavaScript, the easiest way to acquire a conversation program is to integrate [Elizabot](http://www.masswerk.at/elizabot/). This program is not as "witty" as some people might expect, but is somewhat capable of replying to colloquial sentences (of course you can always hack the original implementation for more features). One thing to note is that Elizabot is used as the *last gate of defense* - which means that the dispatcher will only route the query to it if all other cases fail to satisfy the user's intention. 

![img]({{ localhost.url }}/images/blogs/425/1.png)

(yes, our bot is called Senpai)

Wit.ai
------
A chatbot should understand sentences besides static query (i.e. man \<command name>). To achieve this I had to incorporate a NLP library into the program. I personally prefer [Wit.ai](https://wit.ai/) - an easy-to-use NLP platform that lets developers parse the intents of user input. Since features had been planned out in earlier stage, I was able to categorize the intents in similar fashion as the modules.

![img]({{ localhost.url }}/images/blogs/425/2.png)

Wit.ai should act as a layer on top of more basic functionalities, such as searching for a gif. With the help of this API, senpai can understand static query

![img]({{ localhost.url }}/images/blogs/425/3.png)

and more human-friendly sentences.

![img]({{ localhost.url }}/images/blogs/425/4.png)

Reminder
--------
Having a reminder in the chat is helpful for a student group like us (so people cannot make excuses for forgetting about the meetings). Once again, using Wit.ai intent parser, Senpai can remind a certain group of people about a certain event at a specific time when it receives a sentence like this: 

![img]({{ localhost.url }}/images/blogs/425/12.png)

In order to prevent errors like HTTP connection timeouts, the reminder has to handle two types of requests separately:

1. remind within 60 seconds 
	- This type of request can be handled by a simple HTTP response by waiting for the amount of time requested. The logic is straight-forward and does not require additional modules.
2. remind after more than 60 seconds
	- This type of request is more tricky, as it requires a database and a forever-running script. When the request is received by the server, Wit.ai will first parse the sentense into the defined data structure. The reminder app will then store the data into the database. Using [node-cron](https://github.com/ncb000gt/node-cron), the forever-running script checks for existing reminders in the database every minute, then sends the message back to the Slack channel via [incoming webhook](https://api.slack.com/incoming-webhooks) if a reminder is due.

Automate Tasks
--------------
We use Google Sheet for event sign-in purpose, but all the work was done manually before. Now whenever we need to create a new sign-in sheet, we can just ask our chatbot to do it:

![img]({{ localhost.url }}/images/blogs/425/6.png)

Senpai can also help us broadcast a message via SMS:

![img]({{ localhost.url }}/images/blogs/425/7.png)
![img]({{ localhost.url }}/images/blogs/425/8.png)

Interfacing with the Physical World
-----------------------------------
ACM has a student lounge (or clubhouse) for students to work on projects outside of classes. It is often locked because only a few officers have the key to the door at the moment. To ask an officer to open the door every time is just too much of an hassle. Can we ask Senpai to check it for us? Maybe...

![img]({{ localhost.url }}/images/blogs/425/9.png)

The idea here is to have a [Tessel 2](https://tessel.io) microcontroller running as a server at all time to check the clubhouse status. Fortunately, Tessel has developed its own version of [ambient module](http://start.tessel.io/modules/ambient), which can detect ambient light and sound levels. The Tessel server will assume that the clubhouse open if the light and sound levels are above a certain threshold.

![img]({{ localhost.url }}/images/blogs/425/10.png)
![img]({{ localhost.url }}/images/blogs/425/11.png)

This feature is currently under "alpha-testing", and I hope to refine the program and make it accessible to the general members in ACM soon. 

What's next?
------------
A well-designed chatbot can facilitate the planning process and the overall throughput of an organization. Since our chatbot architecture follows the design paradigm of modularization, everyone in ACM can contribute to the chatbot by simply writing a script for a certain purpose. Some of the features we want to add on later include reimbursement, financial statistics, IoT integration, etc. We aim to build more interesting features, and hopefully involve other student organizations in the new era of chatbot automation.