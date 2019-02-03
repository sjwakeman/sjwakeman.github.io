---
layout: post
title:      "Rails app project blog"
date:       2019-02-03 04:39:43 +0000
permalink:  rails_app_project_blog
---

TRAINER APPLICATION
===================
The Trainer application is my Rails Assessment project. 
The Trainer application was developed for trainers to schedule their training sessions with clients and to keep their client's contact information.

* Outlined controllers and methods.
* Created the models with associations, validations and attributes
* Outlined the views to be used.
* Created the routes.
* Table migrations

**Troubleshooting process**:  
1. searching my electronic based notes using CTRL+F to search for a unique term/error message. Keeping copious notes helps resolve simple errors. 
2. When errors become more complex my second step was google searching the unique term/error message.
3. Lastly contacting an instructor with the steps that I have attempted to resolve the issue via notes and google search results.
 
**How to Delete all Active Records to start with a clean slate?**

Throughout the app building process there were several times which I needed to delete all active records to start with a clean slate in order to troubleshoot a particular issue.

In Terminal ran the following two commands:
`rake db:drop` (Deletes the database)
`rake db:migrate` (Runs schema migrations)

Also needed to delete the session cookies for the webpage:
1. Right click on webpage.
2. Left click on the >> symbol
3. Left click on the Storage
4. Left click on Cookies
5. Right click on http://localhost:3000 to highlight
6. Left click on Delete All Session Cookies


Below are several of my trials and tribulations I encountered throughout this process. Hopefully it will help others to avoid the mistakes I made.

**Issue:** Foreign Key placement in database.

Belongs_to Relationship

Client Model:
```
client.rb
belongs_to :user
```
**Learned:** The Foreign Key is always placed with the Model Class that has the belongs_to association.
```
client.rb
user_id
```
**Resolution:** 
The Foreign Key is ALWAYS placed in Model of Belongs_To Relationship. 



**Issue:** Telephone number migration

**Source: **

https://stackoverflow.com/questions/23637057/why-is-it-best-to-store-a-telephone-number-as-a-string-vs-integer

**Learned:**
Telephone numbers are strings of digit characters, they are not integers.
Consider for example:
Expressing a telephone number in a different base would render it meaningless
Adding or multiplying two telephone numbers together, or any math operation on a phone number, is meaningless. 
The result is not another telephone number (except by conicidence)
Telephone numbers are intended to be entered "as-is" into a connected device.
Telephone numbers may have leading zeroes.
Manipulations of telephone numbers, such as adding an area code, are String operations.
Storing the string version of the telephone number makes this clear and unambiguous.



**Issue:** Client link not working

**Resolution:** 
Routing issue `redirect_to client_path(@client)`



**Issue:** Edit training_session link 
`First argument in form cannot contain nil or be empty`

**Resolution:** 
Changed the edit method in training_session_controller to the edit method in the the client controller as the template:
Changed code from:
```
def edit
end
```
Changed code to:
```
def edit
 @training_session = TrainingSession.find(params[:id])
end
```



**Issue:** New Client button error
```
NoMethodError at /clients
	undefined method `user=' for #<Client:0x0c503398>
	Did you mean?  users=
               users
```

**Source:**

https://stackoverflow.com/questions/28377261/nomethoderror-undefined-method-user-for-has-many-through-association

You defined that a Group has_many :users. Therefore a group does not have an user method, but an users method:


**Resolution:**
Translate to my code
You defined that a Client has_many :users. Therefore a client does not have an user method, but an users method:
Changed code from:
    `@client.user = current_user #ties current_user to @client.user`
Changed code to:
    `@client.users = current_user #ties current_user to @client.users`



**Issue:** Cannot create a new client for a different user.

Example:User1 has client1 with training_session.
Logout User1.
Signup User2.
User2 clicks on New Client link and enters information.
Click on Create Client Button
```
ActiveRecord::RecordNotFound at /clients
Couldn't find Client with 'id'=
 app/controllers/clients_controller.rb, line 56 
  def set_client
56    @client = Client.find(params[:id])
  end
```
**Resolution:** 
I was using .find instead of .find_by method. 
Can create a new client.


**Issue:** It appears when I establish a new client it is saved. However when I Cancel a 
Training_Session that Client is deleted from the Client Index page. 
Yet when I enter a Training Session with that same client. The client 
information appears when I click on the Client link.


**Issue:** Two relationships for a client association

**Learned:** 
Having these two relationships doesn’t mean that we get both lists. They actually represent different sets of data. Whichever relationship is defined last is the one that will be called upon when we actually call on a user’s #clients method. 

However, what we can do is alias one of these relationships. We can do this by calling that relationship whatever we’d like (as long as it is a different name) and specifying the class name. 
 
 **Resolution:**
Association changed from:
```
has_many :clients #FIRST RELATIONSHIP
has_many :clients, through: :training_sessions #SECOND RELATIONSHIP
```

Association changed to:
```
has_many :created_clients, class_name: “Client” 
has_many :clients, through: :training_sessions 
```
 
**Learned:** 
“created_clients” is just a name I made up, but because we specify the class 
name where ActiveRecord should fetch the objects from, we can set up the 
relationship and call “user.created_clients” to see the collection of the 
clients a user created. 

While the clients that belong to a user and the ones that the user has through 
training sessions are —to us—all members of the same dataset in the sense that 
they are all a user’s clients, Rails has no way to know that. It only knows 
which clients to grab when we define a relationship, and we can only define one 
relationship with a given method name, ex: user.clients 


```
 has_many :created_clients, class_name: “Client” 
  has_many :clients, through: :training_sessions 
```
 
Each user will end up with a method #clients, which will show the clients they 
have through training_sessions, and a method #created_clients, which will show 
the clients that belong to that user. 
 
When we call #clients, we will still see that list of clients the user has 
through their training sessions. The difference is that we now have an 
additional method, #created_clients, that will show the clients that belong to 
the user. 
 
**Resolution:**
I looked back through the code and noticed the following worked for client's with training_sessions on the clients/index.html.erb page:

  ```
<%= @clients.each do |client| %>
  <%= link_to " #{client.name}", client_path(client) %>
```

I used the above as a template for the next list of client's without training_sessions on the clients/index.html.erb page:

  ```
<%= @created_clients.each do |created_client| %>
  <%= link_to " #{created_client.name}", client_path(created_client) %>
```
  

**Issue:** Duplicate Validation errors for password field on sign up page.

Sign Up Page 

Errors 

Password can't be blank 

Password can't be blank 

Name can't be blank 

Email can't be blank 


**Google search** validation error message format 

**Resolution:** 
Changed the code from:
 `<%= object.errors.full_messages.each do |message|%> `
 
Changed the code to: 
 `<% object.errors.full_messages.each do |message|%>` 
 
**Learned:** 
Note the <% at the beginning instead of <%= 

 **Explanation:** <%= is for making the output from the evaluated expression be printed out. Using 
<% is for indicating that the expression should only be evaluated and output not 
be printed. 


**Google search** Ruby Validation Password can't be blank Password can't be blank. 

**Source:** 

https://stackoverflow.com/questions/26751077/password-cant-be-blank

**Resolution:** 
Secondly, you need to remove validation for encrypted_password field, Remove this line from your model

`validates :password, presence: true`
So I removed the :password from the validation line in user.rb model
Resolved to a single password validation error message.


**Issue:** I created Client2 originally without a training session. Clicked on New Client link and added the information for Client2. 
Which displayed correctly in the Client Index Page under the Clients without Training Sessions heading.
I then created a training session for Client2 however now the Client Index Page has two listings of Client2. One under the Clients with Training Sessions heading and another under Clients without Training Sessions.

**Learned:** 
So it seems now the outstanding issue is to weed out duplicates from the clients that are connected to the user through the belongs_to relationship and clients that are connected through the has_many through relationship.


**Google search** Ruby remove  objects listed in another list

**Source:** 

https://stackoverflow.com/questions/30052095/remove-array-elements-that-are-present-in-another-array

**Learned:** 
You can use .reject to exclude all banned words that are present in the redacted array:
`words.reject {|w| redacted.include? w}`

Translate to:
`@created_clients = current_user.created_clients.reject { |w| clients.include? w}`


Client2 no longer appears under Clients without Training Sessions heading.



**Issue:** Need training_sessions to be sorted by date and start time in descending order instead of id.

**Google search** How to parse the date and time field in the edit.html.erb page.

**Source:** 

https://stackoverflow.com/questions/1449955/rails-date-format-in-a-text-field

**Resolution:** 
Simple one time solution is to use :value option within text_field call instead of adding something to ActiveRecord::Base or CoreExtensions.
For example:
`<%= f.text_field :some_date, value: @model_instance.some_date.strftime("%d-%m-%Y") %>`

Simple and direct. In my experience it's usually not worth fretting over adding some fancy code elsewhere - this has always been good enough for me. – dmonopoly Mar 16 '12 at 8:04


**Issue:** Helped add menu links to main menu bar for two classes

**Source:** 

https://gist.github.com/mynameispj/5692162

**Resolution:** 
Application_helper.rb
module ApplicationHelper
```
def current_class?(test_path)
    return 'active' if request.path == test_path
    ''
  end
```
Users.show.html.erb
```
<table>
  <tr><td><h3><%= @user.name %> - Main Menu</h3></td></tr>
  <tr><td><%= link_to "Training Sessions Index", training_sessions_path %></td>
  <td colspan="2"> </td>
  <td><%= link_to "Client Index", clients_path %></td>
  </tr>
  <tr><td> </td></tr>
  <tr><td><%= link_to "New Training Session", new_training_session_path %></td>
  <td colspan="2"> </td>
  <td><%= link_to "New Client", new_client_path %></td>
  </tr>
  <tr><td> </td></tr>
  <tr><td><%= link_to "Edit Training Session", edit_training_session_path %></td>
  <td colspan="2"> </td>
  <td><%= link_to "Edit Client", edit_client_path %></td>
  </tr>
  <tr><td> </td></tr>
  <tr><td><%= button_to "Log Out", logout_path, method: :delete %></td></tr>
  <tr><td> </td></tr>
</table>
```


**Issue:** Form_for vs Form_tag usage

**Google Search** Form_For VS. Form_Tag

**Source:** 

https://stackoverflow.com/questions/14463946/form-for-and-form-tag-when-to-use-which-one

**Learned:** 
form_tag simply creates a form. form_for creates a form for a model object. They are not interchangeable and you should use the one that is appropriate for a given case.
form_for is a bit easier to use for creating forms for a model object because it figures out what url to use and what http method to use depending on whether the object is a new record or a saved record.
In most cases when you are sending data to your database with a model, in methods like create, update, destroy. For this type of action you can use form_for.
Instead form_tag only create a simply form html, for example, a search:
`<%= form_tag("/search", :method => "get") do %> <%= label_tag(:q, "Search for:") %> <%= text_field_tag(:q) %> <%= submit_tag("Search") %> <% end %>`

I like this because your example hints at the fact that form_tag is often used for a "search" functionality, as it usually doesn't perfectly map to a Controller#action – mmcrae Feb 6 '15 at 15:30

I'd add that form_tag is good for creating session based forms. and as you say, when interacting with a database - form_for is the way to go. – BKSpurgeon May 10 '16 at 1:34

**Learned:**
Display only training_sessions for current_user in training_session index

`@training_sessions = current_user.training_sessions.sorted`

`@training_session.user = current_user #ties current_user to @training_session.user`

**Learned:**
Display only clients for current_user in the client index
  ```
#Displays clients of current_user
    @clients = current_user.clients.sorted
```


`@client.user = current_user #ties current_user to @client.user`


**Issue:** To weed out duplicates from the clients that are connected to the user through the belongs_to relationship and clients that**** are connected through the has_many through relationship.

**Google search** Ruby how to weed out duplicate records from app list

**Source:** 

https://stackoverflow.com/questions/26358774/remove-duplicate-entries-from-powershell-object-while-keeping-psobject-intact

That looks like the Format-Table output of a collection of PSObjects with the same properties.
To weed out duplicates, use Sort-Object -Unique:
So I included a validation in the client.rb model
validates :name, uniqueness: true
Refreshed the http://localhost:3000/clients webpage and the duplication is still present.

**Google search** Ruby remove  objects listed in another list

**Source:** 

https://stackoverflow.com/questions/30052095/remove-array-elements-that-are-present-in-another-array

**Learned:** 
You can use .reject to exclude all banned words that are present in the redacted array:
`words.reject {|w| redacted.include? w}`

**Resolution:**
Translated to the code in clients_controller:
```
@created_clients = current_user.created_clients.reject { |w| clients.include? W}
NameError at /clients
undefined local variable or method `clients' for #<ClientsController:0x0c65c8d4>
Did you mean?  @clients
```

Changed the code in the clients_controller:
`@created_clients = current_user.created_clients.reject { |w| @clients.include? w}`

Result: Appears to solved the user_id assignment issue through training_session
Client2 now appears under Clients without Training Sessions heading.


**Issue:** I was testing out my app and discovered when I cancelled a training_session the client no longer displayed in the client index page.
 I thought the client should appear under the Clients without Training Sessions heading.
I am curious as to why cancelling the training_session would delete the client. I thought cancelling the training_session would delete the training_session of the client?
I am only destroying the training_session not the client.

**Learned:**
Even if the client doesn’t show up in the list of Clients Without Training 
Sessions, it might not indicate that the object is being deleted. Remember: this 
list is REALLY showing us clients that were created by the user — filtering out 
those clients that have training sessions. Can you verify in the console that 
the client is actually being deleted? I think I might have an idea of what’s 
going on. 

Remember what we discussed before: the list of "Clients without a Training 
Session” isn’t actually just clients without a training session — it’s a list of 
clients that we created to belong to a user. Notice that this Client doesn’t 
actually have a user_id — which would be how we associate that client with a 
user.  

Clients created through Training Sessions are NOT given the user_id of the User.
Clients created through New Client link is given the user_id of the User.

I don't understand why this would be as both clients are being created under Trainer1 or user_id:1

Great question. So, the Clients we create don’t actually know anything about the user who is logged in. 
Our objects exist independently of our session and won’t be associated with any user automatically. 

The New Client link is using the create method under the clients_controller.rb 
and the client created through the training_sessions is using the create method under the training_sessions_controller.

Let’s talk about how: 
a) clients are created along with training sessions 
b) clients are created without training sessions 
 
There are actually two distinct processes happening here. 
 
a) When a client is created along with a training session, it happens through the 
nested form we built — not a client form, but rather the new training session 
form. Remember #client_name= on the training session model? THAT’S where that 
client gets created. 
 
b) When a client is created independently of a training session, THAT’S when the 
code in the #create method on the ClientsController comes into play. 

So when a client is created without a training session the user_id is associated with that client. However clients through a training session are not associated with the user_id because it is a different process.

Let’s break this down even further. Think about what a user_id does. User_id is a foreign key on a client that allows a client to “belong to” a user. 

When we create clients along with a training session (or create a training session with a client that already exists), those clients become associated with the user through the training session. 

When we create a client outside of that context — without a training session — the object will be created, but won’t be associated with the user unless it can belong to a user directly — without a training session. This is where user_id comes in, and if memory serves, this is set up correctly in your app.

We might be able to think of the user that a client belongs to as the user that created it — and it seems this is how you’d like to use it. Let’s go back to the original question:

So Clients created through Training Sessions are NOT given the user_id of the User.
Client created through New Client link is given the user_id of the User.

Our clients don’t care who is logged in because we haven’t told them whether we should do anything with that information. We’ve only told clients created without training sessions to do anything with the id of the user logged in. If you want a client created WITH a training session to have a user_id, you’ll need to set that when the client is created.

**Resolusion:**
So I do not have the current_user in the training_session.rb model. So I need to include the current_user and then save the current_user.

The good news is I can now create new clients again. So it appears line 24:
 `@created_client.user = current_user #TIE current_user to @created_client.user`
was the issue causing the prevention of the New Client and the error message.

**Learned:** 
I find it interesting that when I look at the Training Sessions Index Page the client does have the user_id:1

`[#<TrainingSession id: 1, date: "2018-01-01", start_time: "2000-01-01 09:00:00", end_time: "2000-01-01 10:00:00", location: "Gym", booked_status: true, client_id: 1, user_id: 1, created_at: "2018-09-18 19:10:51", updated_at: "2018-09-18 19:10:51">]`

So it appears the client with a training session has the user_id of 1 when in the Training Sessions Index Page however that attribute is lost when the same client is viewed Clients Index Page.

This means the Clients Index Page does not get the user_id value.

Notice that this object is not a client, but a training session. All training 
sessions will have a client id and a user id. There is no case where one page 
will display saved data, but another page won’t get that data. 


This I am a bit confused by for a few reasons. 
 
1. A model method should not contain controller logic 
2. I’m not quite sure where this method is called 
 
It may be helpful to outline the current flow of information when a client is 
created along with a training session. After the form is submitted, what methods 
get called and when? If you can pin that down, it may become a bit more clear 
what logic you will need to place and where you need to place it. 


**Issue:** I find it odd that the client has a user_id on the training_sessions index but does not have the user_id on the clients index page.
client1 has user_id of 1:

```
PRY
    18: def create
    19:   @training_session = TrainingSession.new(training_session_params)
    20:   @training_session.user = current_user #ties current_user to @training_session.user
    21:   @training_session.client.user_id = current_user.id #ties current_user id to @training_session.client_id
 => 22:   binding.pry
```

```
[1] pry(#<TrainingSessionsController>)> @training_session.client.user_id 
=> 1
```

Training Sessions Index Page
```
[#<TrainingSession id: 1, date: "2019-01-01", start_time: "2000-01-01 09:00:00", 
end_time: "2000-01-01 10:00:00", location: "Gym", booked_status: true, client_id: 1, user_id: 1, 
created_at: "2019-01-30 20:30:28", updated_at: "2019-01-30 20:30:28">]
2019-01-01
```

The same client1 however does not have a user_id on 
Clients Index Page
Clients with Training Sessions
```
[#<Client id: 1, name: "client1", email: nil, home_address: nil, work_address: nil, home_phone: nil, 
work_phone: nil, smart_phone: nil, user_id: nil, provider: nil, uid: nil>]
```

I created client1 through the training session.
training_sessions_controller.rb                                 
```
def create
@training_session = TrainingSession.new(training_session_params)
@training_session.user = current_user #ties current_user to @training_session.user
@training_session.client.user_id = current_user.id #ties current user id to client_id
if @training_session.save
     redirect_to training_session_path(@training_session)
 # new server request happens, so the previous controller
 #instance is destroyed and a new controller instance is created.
 else
  render 'new'
   #When you render, you remain in the same controller instance
 end
end
```

I am saving the @training_session object however the training_session client user_id is not being saved correctly?

The two objects you sent me are of different types - 
the first is a TrainingSession object, 
the second is a Client object

So it looks like the TrainingSession with an id of 1 does indeed have a `user_id`, 
but that the Client object with an id of 1 does not have a `user_id`

So, just to clear that up, they are two different objects, 
rather than the same object appearing with different qualities in different places

That is strange though that the client does not have a user_id, as it looks like it should based on your code

You might need to call `save` on the client separately

I have attempted the following in the training_sessions_controller.rb                             

```
PRY
    18:   def create
    19:     @training_session = TrainingSession.new(training_session_params)
    20:     @training_session.user = current_user #ties current_user to @training_session.user
    21:     @training_session.client.user_id = current_user.id #ties current_user id to @training_session.client_id
 => 22:     binding.pry
```

```
[1] pry(#<TrainingSessionsController>)> @training_session
=> #<TrainingSession:0x0cd3a098
 id: nil,
 date: Tue, 01 Jan 2019,
 start_time: 2000-01-01 12:00:00 UTC,
 end_time: 2000-01-01 13:00:00 UTC,
 location: "Work",
 booked_status: true,
 client_id: 2,
 user_id: 1,
 created_at: nil,
 updated_at: nil>
```

=>@training_session has user_id of 1

```
[2] pry(#<TrainingSessionsController>)> @training_session.client
=> #<Client:0x0cd3807c
 id: 2,
 name: "client100",
 email: nil,
 home_address: nil,
 work_address: nil,
 home_phone: nil,
 work_phone: nil,
 smart_phone: nil,
 user_id: 1,
 provider: nil,
 uid: nil>
```

=>@training_session.client has user_id of 1

```
[3] pry(#<TrainingSessionsController>)> @training_session.user
=> #<User:0x0cb9f774
 id: 1,
 name: "Trainer1",
 email: "Trainer1@gym.com",
 image: nil,
 uid: nil,
 password_digest: "$2a$10$3Uj.NG.mDvNg.81dZOOJZeYw6hV6Qn.UcsZbxDYl4D3Ib8Y08Bn3W",
 created_at: Thu, 31 Jan 2019 00:07:44 UTC +00:00,
 updated_at: Thu, 31 Jan 2019 00:07:44 UTC +00:00>
```

=>@training_session.user has user_id of 1

So the client is created through the training_sessions_controller.rb using the create method. 
Appears to have the correct user_id associated of 1. 
When I click on the Client Index link which uses the clients_controller.rb using the index method to display.

clients_controller.rb                                                                                                                            
```
def index
#Displays clients of current_user
@clients = current_user.clients.sorted
 #Displays created_clients of current_user
#@created_clients = current_user.created_clients.sorted
#Displays created_clients of current_user while reject @clients duplicates.
@created_clients = current_user.created_clients.reject { |w| @clients.include? w}
end 
```

```
#<Client id: 2, name: "client100", email: nil, home_address: nil, work_address: nil,
 home_phone: nil, work_phone: nil, smart_phone: nil, user_id: nil, provider: nil, uid: nil>]
```

The user_id for client100 becomes nil.

**Resolution:**
Need to save the client to the database.
`@training_session.client.save #save client to database`

> I would like to thank Luisa, Kevin and Ian for their help, patience and expertise throughout this process.


