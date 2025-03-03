## observations

- a lot of the methods that I'm using are async await, probably because the front end wants to be non-blocking with respect to backend calls and the backend functions want ot be non-blocking with respect to database I/O
- I used an llm to help me plan the design, it suggested a layered architecture for the front and backend, there are lots of layers of abstraction. 


### youtube visualization project
finding the right data discovery doc url was hard, 
docs on google are comprehensive but hard to wade throug hif you are lookign to just get things working quickly
I did get help from the LLM on figuring out which api had the data I was looking for it was analytics api
there was a github page for all google api clients a javascript library to make it easier
had to check the error message on my api request in the browser to see that it was saying I needed to use oauth 2.0 with a client id to get things worknig
for google you ahve to set it up in your develoer console there are a lot of little steps to go through but it isn't that bad. 
I hadn't had to hide a secret with js in the browser before that was a lesson, got help from llm on that too.


### oauth 2.0
the main idea is to have the application redirect to an auth server where the user is told their account data will be accessed by the application if hty approve acting on their behalf as the authenticated user. if they accept then they get an authorization token back which the application may either validate or exchange. 


### html knowledge bombs
in html5 there are two attributes in the script tag you can add, 'async' and 'defer'. if you use async, then instead of blocking the html parsing to download and run the script, it will download the script in a separate thread concurrent to the HTML parsing. however if you just use async, it will still pause the html parsing to execute the script when it is done downloading. Also, it could be that scripts execute in a different order than they were ordered in the html file because it might take longer for one to download than another. with defer, scripts are downloaded concurrently to html but they are not executed until after html parsing is done. they also are executed in the same order that they would have been encountered going top to bottom down the page, the declaration order.