# Action Cable
by Darius Sadighi, May Zhang, Reggie Kim

### What is Action Cable
Action Cable is one of the key additions to Rails 5, allowing a real-time, open connection with the server using WebSockets.

WebSockets?? In the typical MVC model, the user enters the path into the browser, which hits the router. The router directs to the appropriate controller. The controller queries the database, gets the data, then passes that data to the view.  Finally the view is rendered on the browser. The connection with the database is opened briefly by the controller and then closed as soon as the controller gets data. Any additional connections with the database would have to be initiated by the browser (event listeners, set intervals, etc) or when the user hits the server with another GET request. 

WebSockets is a protocol that allows the browser and server to keep an open two-way connection. When the database gets new data, the server automatically broadcasts that data and sends it to browsers connected to that channel.

### Making an app with Action Cable
#### Getting Started
We'll create a chat app called ShortSlack-Chat

1. In the terminal create a new Rails app, specifying the latest version of Rails 5 and postgres as the database (As of May 30, 2016 the latest is 5.0.0rc1): 
    * ```rails \_5.0.0rc1\_ new <Your app name> -d postgresql```
    * in this example: ```rails \_5.0.0rc1\_ new ShortSlack-Chat -d postgresql```
1. cd into your app
1. ```rails db:create```
(note that in Rails 5 for your db creations and migrations, you can use the command "rails" instead of "rake")

#### Setting up Rails
1. Create a model in the terminal, called Message it will have one attribute, content
    * ```rails g model message content:text```
1. Create a controller for the chat room
    * ```rails g controller rooms show```
1. Migrate the model
    * ```rails db:migrate```
1. Set up the routes so that the root directs you to chat room's show page
    * config/routes.rb    
    ```ruby
    Rails.application.routes.draw do
        root to: 'rooms#show'
    end
    ```
1. Set up the rooms controller to show all messages
    * app/controllers/rooms_controller.rb
    ```ruby
    class RoomsController < ApplicationController 
        def show 
            @messages = Message.all
        end 
    end
    ```
1. Finally, set up the views and include a form to send a chat message
    * app/views/rooms/show.html.erb
    ```html
    <h1>Chat room</h1> 
    <div id="messages"> 
        <%= render @messages %> 
    </div> 

    <form> 
        <label>Say something:</label><br> 
        <input type="text" data-behavior="room_speaker"> 
    </form>
    ```
    * create file: app/view/messages/_message.html.erb
    * note that the file is a partial, which will be used to render the message
    ```html
    <div class=“message”>
        <p><%= message.content %></p>
    </div>
    ```

So far, everything has been standard Rails stuff. We've set up the Model, View, and Controller

#### Create a channel
Now to set up Action Cable. To enable Action Cable in our app there are two steps:
1. Go to the routes and mount Action Cable
    * config/routes.rb
    ```ruby
    mount ActionCable.server => '/cable'
    ```
1. Initialize in cable.coffee or cable.js
    * app/assets/javascripts/cable.coffee or /cable.js
    * Rails 5 will either create a cable.coffee or cable.js for you
    * in cable.coffee
    ```
    #= require action_cable 
    #= require_self 
    #= require_tree ./channels 
    # 
    @App ||= {} 
    App.cable = ActionCable.createConsumer()
    ```
    * in cable.js
    ```javascript
    //= require action_cable
    //= require_self
    //= require_tree ./channels

    (function() {
        this.App || (this.App = {});
        App.cable = ActionCable.createConsumer();
    }).call(this);
    ```
