---
layout: post
title:      "Sinatra Portfolio Project - Scuba Logbook"
date:       2017-10-03 03:17:37 +0000
permalink:  sinatra_portfolio_project_-_scuba_logbook
---


Created a Scuba Diver's logbook. 

Create a new table in the database to store the scuba dives.

Scuba Dives should have:
A dive number, date, visibility, location, total bottom time to date, bottom time this dive, accumulated time, PSI/BAR start, PSI/BAR end and dive comments (which can be written as one string containing all the notes).

Make sure you have a corresponding model for your dives.

In the dives_controller.rb, set up a controller action that will render a form to create a new dive. This controller action should create and save this new dive to the database.

Again in the dives_controller.rb, create a controller action that uses RESTful routes to display a single dive.

Create a third controller action that uses RESTful routes and renders a form to edit a single dive. This controller action should update the entry in the database with the changes, and then redirect to the dive show page.

Create a controller action (show dives) that displays all the dives for an individual user in the database.

Add to the dive show page a form that allows a user to delete a dive. This form should submit to a controller action that deletes the entry from the database and redirects to the show dives page.

==========================================================================================================

GEMFILE AND ENVIRONMENT.RB
This project is supported by Bundler and includes a Gemfile.

Run bundle install before getting started on the project.
As this project has quite a few files, an environment.rb is included that loads all the code in your project along with Bundler. You do not ever need to edit this file. When you see require_relative ../config/environment, that is how your environment and code are loaded.

MODELS
You'll need to create two models in app/models, one User model and one Dive. Both classes should inherit from ActiveRecord::Base.

MIGRATIONS
You'll need to create two migrations to create the users and the dives table.
Users should have a username, email, has a secure password, and has many dives.
Dives should have a dive number, date, visibility, location, bottom_time_to_date, bottom_time_this_dive, accumulated_time, dive_start, dive end, dive comments, user id and belong to a user.

ASSOCIATIONS
You'll need to set up the relationship between users and dives. Think about how the user interacts with the dives, what belongs to who?

INDEX PAGE
You'll need a controller action to load the index page. You'll want to create a view that will eventually link to both a login page and signup page. The  index page should respond to a GET request to /.

CREATE DIVE
You'll need to create two controller actions, one to load the create dive form, and one to process the form submission. The dive should be created and saved to the database. The form should be loaded via a GET request to dives/create_dive and submitted via a POST to /dives.

SHOW DIVE
You'll need to create a controller action that displays the information for a single dive. You'll want the controller action respond to a GET request to /dives/:id.

EDIT DIVE
You'll need to create two controller actions to edit a dive: one to load the form to edit, and one to actually update the dive entry in the database. The form to edit a dive should be loaded via a GET request to /dives/:id/edit. The form should be submitted via a POST request to /dives/:id.
You'll want to create an edit link on the dive show page.

DELETE DIVE
You'll only need one controller action to delete a dive. The form to delete a dive should be found on the dive show page.
The delete form doesn't need to have any input fields, just a submit button.
The form to delete a dive should be submitted via a POST request to dives/:id/delete.

SIGN UP
You'll need to create two controller actions, one to display the user signup and one to process the form submission. The controller action that processes the form submission should create the user and save it to the database.
The form to sign up should be loaded via a GET request to /signup and submitted via a POST request to /signup.
The signup action should also log the user in and add the user_id to the sessions hash.
Make sure you add the Signup link to the login page.

LOG IN
You'll need two more controller actions to process logging in: one to display the form to log in and one to log add the user_id to the sessions hash.
The form to login should be loaded via a GET request to /login and submitted via a POST request to /login.

LOG OUT
You'll need to create a controller action to process a GET request to /logout to log out. The controller action should clear the session hash

PROTECTING THE VIEWS
You'll need to make sure that no one can create, read, edit or delete any dives.
You'll want to create two helper methods current_user and logged_in?. You'll want to use these helper methods to block content if a user is not logged in.
It's especially important that a user should not be able to edit or delete the dives created by a different user. A user can only modify their own dives.

==========================================================================================================

FILE STRUCTURE
├── CONTRIBUTING.md
├── Gemfile
├── Gemfile.lock
├── LICENSE.md
├── README.md
├── Rakefile
├── app
│   ├── controllers
│   │   └── application_controller.rb
user_controller.rb
dive_controller.rb
│   ├── models
│   │   ├── dive.rb
│   │   └── user.rb
│   └── views
│       ├── index.erb
│       ├── layout.erb
│       ├── dives
│       │   ├── dive_sheet.erb
│       │   ├── edit_dive.erb
│       │   ├── show_dive.erb
       │      │     ── show_dives.erb
│       │   └── welcome.erb
│       └── users
│           ├── signup.erb
│           └── login.erb
│           └── show.erb
├── config
│   └── environment.rb
├── config.ru
├── db
│   ├── development.sqlite
│   ├── migrate
│   │   ├── 20151124191332_create_users.rb
│   │   └── 20151124191334_create_dives.rb
│   ├── schema.rb
│   └── test.sqlite
└── spec
   ├── controllers
   │   └── application_controller_spec.rb
   └── spec_helper.rb

==========================================================================================================

Scuba Logbook Notes:

Used the following link https://github.com/thebrianemory/corneal to establish the project.

Downloaded and installed Oracle VM VirtualBox and Ubuntu.

Create a file `spec.md` in the root directory of your project
touch spec.md
Git commit frequently and often
Git commit -m “...” 
// ♥ rake db:create_migration NAME=add_email_to_user
db/migrate/20170908232555_add_email_to_user.rb
[23:25:55] (master) scuba-logbook
// ♥ rake db:migrate
== 20170908232555 AddEmailToUser: migrating ===================================
== 20170908232555 AddEmailToUser: migrated (0.0000s) ==========================
[23:26:06] (master) scuba-logbook
// ♥ rake db:migrate SINATRA_ENV=test
== 20170908232555 AddEmailToUser: migrating ===================================
== 20170908232555 AddEmailToUser: migrated (0.0000s) ==========================


SHOTGUN ISSUE
scott@scott-VirtualBox:~/Development/code/Sinatra/Project/scuba-logbook$ shotgun== Shotgun/Thin on http://127.0.0.1:9393/
Thin web server (v1.7.2 codename Bachmanity)
Maximum connections set to 1024
Listening on 127.0.0.1:9393, CTRL+C to stop
/home/scott/.rvm/gems/ruby-2.3.3/gems/eventmachine-1.2.5/lib/eventmachine.rb:530:in `start_tcp_server': no acceptor (port is in use or requires root privileges)
SHOTGUN RESOLUTION
scott@scott-VirtualBox:~/Development/code/Sinatra/Project/scuba-logbook$ ps aux | grep shotgun
scott    28102  0.2  0.6 101308 25928 ?        Sl   13:59   0:00 ruby /home/scott/.rvm/gems/ruby-2.3.3/bin/shotgun
scott    28624  0.0  0.0  21296   972 pts/1    S+   14:03   0:00 grep --color=auto shotgun
scott@scott-VirtualBox:~/Development/code/Sinatra/Project/scuba-logbook$ kill -9 28102


VERIFY SHOTGUN PORT IS CLOSED
scott@scott-VirtualBox:~/Development/code/Sinatra/Project/scuba-logbook$ kill -9 28102
scott@scott-VirtualBox:~/Development/code/Sinatra/Project/scuba-logbook$ ps aux | grep shotgun
scott    28634  0.0  0.0  21296   912 pts/1    S+   14:06   0:00 grep --color=auto shotgun

SHOTGUN PORT ISSUE CONCLUSION
Ctrl + C to EXIT SHOTGUN PROPERLY!!!!

Active Record Issue with dive and dives vs dife and difes 
Found the solution to the issue at:
http://michaelries.info/2017/04/27/diving_into_activerecord/

SOLUTION:

Add the following to the environment.rb file and the application operates as intended.:

ActiveSupport::Inflector.inflections do |inflect|
  inflect.irregular 'dive', 'dives'
end

Thanks to the help and explanations I received from Enoch Griffith, the Technical Coach who I was able to schedule meetings with and do a mock assessment.
