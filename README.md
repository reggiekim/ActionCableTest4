# Action Cable
by Darius Sadighi, May Zhang, Reggie Kim

### What is Action Cable
Action Cable is one of the key additions to Rails 5, allowing a real-time, open connection with the server using WebSockets.

WebSockets?? In the typical MVC model, the user enters the path into the browser, which hits the router. The router directs to the appropriate controller. The controller queries the database, gets the data, then passes that data to the view.  Finally the view is rendered on the browser. The connection with the database is opened briefly by the controller and then closed as soon as the controller gets data. Any additional connections with the database would have to be initiated by the browser (event listeners, set intervals, etc) or when the user hits the server with another GET request. 

WebSockets is a protocol that allows the browser and server to keep an open two-way connection. When the database gets new data, the server automatically broadcasts that data and sends it to browsers connected to that channel.

### Getting Started
We'll create a chat app called ShortSlack-Chat
1. In the terminal create a new Rails app, specifying the latest version of Rails 5 and postgres as the database (As of May 30, 2016 the latest is 5.0.0rc1): 
rails _5.0.0rc1_ new <Your app name> -d postgresql
rails _5.0.0rc1_ new ShortSlack-Chat -d postgresql
1. cd into your app
1. rails db:create
(note that in Rails 5 for your db creations and migrations, you can use the command "rails" instead of "rake")
1. 
