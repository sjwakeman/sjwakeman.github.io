---
layout: post
title:  "CLI data gem project"
date:   2017-04-29 19:39:00 +0000
---

I chose the https://seekingalpha.com webpage of stock ideas incorporating both Long and Short articles.

I followed the "video walkthrough" (https://www.youtube.com/watch?v=lDExWIhYKI) of building a CLI Gem from scratch called Daily Deal by Avi. I followed along and watched and rewatched multiple times as Avi explained the steps involved. I created screenshots of the process. I had several miss steps with the naming of folders and file structure that caused me to delete and restart several times. Near the end of this process it was brought to my attention to not use the "video walkthrough" as a template so I decided to incorporate the structure already developed with the video and use the programming and files used in the "50 Best Restaurants in the World" CLI gem.

I used the make a gem using bundler stubbing out the structure for me in advance.

<Gem bundler 1-4>
<Version rb 1>

Had to delete and regenerate with underscores instead of using hyphen. SeekingAlpha-Articles to Sa_Articles.

Created the Sa_Articles in bin directory.
Tested with puts "Hello World"
Ran bin file ruby bin/sa-articles through Ruby interpreter.

<First run of CLI>

Ran bin file as a user would through bash and the Ruby interpreter using ./bin/sa-articles.
Permission denied message.

<Bash run CLI 1>

Needing to ls -lah permissions for file had read and write permissions not executable permissions.

<read and write permissions no executable permissions>
<chmod>
<chmod operation not permitted>

Contacted online support to troubleshoot why this error message was occuring.

<Granted executable permissions>

Added executable permissions and confirmed with ./bin/sa-articles outputting the "Hello World."

<Second run of CLI>

Create the Logic of the CLI.
SaArticles::CLI.new.call in bin folder>sa-articles file.
Create the CLI class in lib folder>CLI file.
Load interpreter #!/usr/bin/env ruby
class SaArticles::CLI 
Created a def call 
   puts "Today's seekingalpha articles"

<cli rb-1>

<cli rb -2>

Try to get ./sa-articles to run.
uninitialized constant SeekingalphaArticles.
'require seekingalpha_articles'
no such file exists.
Environment file: sa_articles.rb
'require ./lib/seekingalpha_articles'
cannot load file version (Load Error)
require_relative "./sa_article/version"
require other files:
require_relative './sa_article/cli'
Delete module SaArticles
          #your code goes here...
          end
Run ./bin/sa-articles
   outputs "Today's trading idea articles"
	 
ALWAYS LOOKING FOR NEW ERRORS

Stub some things out. Get interface working without real data. Eventually will change to scape long and short articles.

def call
Puts “Today’s trading idea articles.”
Run program and make sure it works.

def list_articles 
Here doc ruby.
http://blog.jayfields.com/2006/12/ruby-multiline-strings-here-doc-or.html
Run program and make sure the here doc information displays.

<cli rb -3>

How to get rid of indentation in here doc ruby.
Puts <<-DOC..gsub /^\s*/, ‘’
Run program make sure it works.

<remove indentation in here doc>

def menu
puts”Enter the number of the article you would like to read:”

<cli rb -4>

Run program make sure it works.

Move Puts “Today’s trading idea articles.” from def call to def list_articles.
The interface is starting to come together, discover what I need to program to move on.

Input = gets.strip
Case input
   When “1”
      Puts “Article 1 displayed”
   When “2”
      Puts “article 2 displayed”

<cli rb -5 input 1 and 2 working>

Run program make sure when 1 is entered it displays “Article 1 displayed” and when  2 is entered it displays “Article 2 displayed”

Program exited after 1 or 2. Make change to include exit in menu method interface.
puts”Enter the number to the article you would like to read or type exit.”
Loop menu
While input != “exit”
Input = gets.strip.downcase

<cli rb -6 input exit message>

Run program to verify it is displaying properly.
Goodbye method in def call.

def goodbye
Puts “Check back later for more articles.”
Verify whole fake interface is running now.
Undefined local variable input.
Need to insert input = nil.
Run program again to verify “exit”, “1” and “2” work correctly.
Did not reprint menu after entering “1” or “2”.
Move down puts”Enter the number to the article you would like to read or type exit.”            beneath While input != “exit”
Run program again to verify the menu displays after entering “1” or “2”.

Add an option to interface with “list”
puts”Enter the number to the article you would like to read or type list to see the articles again or type exit.”  
when “list” recall list_articles menu

Run program again to verify “list”,”1”, “2” and “exit” works.
Determined no error message when a number other than 1 or 2 was entered.

Create an else statement puts “Not sure what you want. Type list or exit.”
Run again to verify else statement displays when typing in random letters or numbers that are not “1” or “2”.

<cli rb -7inputs list and not sure what you want message>
<Console run with 2 article list test passed>
<Cli-9 the_article working properly>

THE INTERFACE FOR THE PROGRAM IS DONE BUT NONE OF THE DATA IS REAL.
START MAKING THE DATA REAL

Recap:
Defined execution point. 
./bin/sa-articles
Stubbed out an environment
lib/sa_articles.rb
Anything that needs environment can load it through
require ‘./lib/sa_articles
The bin file creates a new instance of the CLI class.
SaArticles::CLI.new.call
Runs the def call in CLI that in turn runs:
List_articles, menu and goodbye methods.

Time to make the data real. Substitute fake data for real data.
Look back at notes.md

<Notes.md>

What is an article? An article has a title, author, stock symbol and url.

I want objects to have a collection of these properties that the CLI can use.

PROGRAM THE CODE YOU WISH YOU HAD

@article = SaArticles::Articles.ideas
Articles object has a class method ideas that returns these articles.
Have a new object in domain so create a new file called Articles in lib folder
Articles is going to have a class SaArticles::Articles with a class method ideas

def self.ideas
#should return a bunch of instances of articles.
To make sure the method call is working correctly will use fake data here doc cut from cli.rb to articles.rb
Type in ./bin/sa-articles
Uninitialized constant SaArticles::Articles (NameError)
Never required this new file in the environment file sa_articles.rb
Add require_relative ‘./sa_articles/article

<sa_articles rb -2>

Now it is working again.

Want objects to have one responsibility
article_1=self.new
article_1.title = “Article 1”
article_1.author = “Author 1”
article_1.stock_symbol = “Stock symbol 1”
article_1.url = “https://seekingalpha.com”

article_2=self.new
article_2.title = “Article 2”
article_2.author = “Author 2”
article_2.stock_symbol = “Stock symbol 2”
article_2.url = “https://seekingalpha.com”

These are my objects.
Increasing the abstraction of everything I have hard coded until it actually begins talking to seekingalpha.com
At the end of the ideas method I should return these two articles an array of article_1 and article_2.

[article_1, article_2]

Stubbed out the data, it is totally fake but allowing me to move forward.
Instances of articles is title, author, stock_symbol and url.

attr_accessor :title, :author, :stock_symbol, :url
Play with SaArticles::Articles in console without having to run program.
Console file in bin 
./bin/console
>SaArticles::Articles.ideas
Printed out stubbed information.

<bin console 1-4>
<Console run Sa-Articles.today>

Take the stubbed here doc out now from the articles.rb file def self.ideas

Better way to require environment 
Cut from bin/console file in lib folder and move to bin/sa-articles:
   require “bundler/setup”
   require “sa_articles” 
Run ./bin/sa-articles running very well.
Now with ./bin/console can play with this method SaArticles::Articles.ideas and displays the articles instances. 

Update CLI instead of making it hard coded, need to change case statement as it does not feel as resilient. 
Change to an if statement

if input.to_i  > 0
   puts @articles[iput.to_i-1]
elsif input == “list”
   list_articles
else
   puts “Not sure what you want. Type list or exit.”

Verify it works. Run ./bin/sa-articles
Lost the list Article 1 and Article 2.
Need to improve add to lib/cli.rb file

@articles.each.with_index(1) do |article, i|
   Puts “#{i}. #{article}”
Run ./bin/sa-article to verify it works correctly.

Improve on Ruby object orientation. 
    Puts “#{i}. #{article.title} - #{article.author} -#{article.url}”
Run ./bin/sa-article to verify the fake data is displaying properly.
If return articles objects everything else would just work.
Copy article logic 
the_article = @articles[input.to_i-1] 
   puts “#{article.title} - #{article.author} -#{article.url}”

CLI is still using fake data but is using a higher level of fake data.
Instead of hardcoded data strings it is reading out of objects.
As soon as this reads data from seekingalpha.com everything else will just work.

Make article_1 and article_2 into real data.
Need to replace with scrape seekingalpha long and short articles. Return articles based on that data.

Make another object scrape_longs and scrape_shorts
Different modes in programming. Do you like this pattern and how do you feel about it?

def self.ideas
SaArticles::Articles.scrape_longs.new(“https://seekingalpha.com/stock-ideas/long-ideas”)
SaArticles::Articles.scrape_shorts.new(“https://seekingalpha.com/stock-ideas/short-ideas”)
What is the code you want or wish you had?
Create self.scrape_articles method

def self.scrape_articles
   article_1=self.new
   article_1.title = “Article 1”
   article_1.author = “Author 1”
   article_1.stock_symbol = “Stock symbol 1”
   article_1.url = “https://seekingalpha.com”

   article_2=self.new
   article_2.title = “Article 2”
   article_2.author = “Author 2”
   article_2.stock_symbol = “Stock symbol 2”
   article_2.url = “https://seekingalpha.com”

[article_1, article_2]

Ready to scrape articles now!
Will move into its own class later.

def scrape_articles
#Go to https://seekingalpha.com/stock-ideas/long-ideas for long articles
#extract the properties
#instantiate an article

#Go to https://seekingalpha.com/stock-ideas/short-ideas for short articles
#extract the properties
#instantiate an article

Change [article_1, article_2] to [long_article] and [short_article]

def scrape_articles
   articles = []
   [articles]

Deal with scrapping. Need dependencies.
Go to gemspec.
Spec folder>sa_articles.gemspec file add:
spec.add_development_dependency “pry”
spec.add_dependency “nokogiri”
Run bundle = works!
Run ./bin/console
   >Nokogiri
Name Error:uninitialized constant Nokogiri
Need to require ‘nokogiri’ in lib/sa_articles.rb file to work.
Included require ‘pry’ in lib/sa_articles.rb file as well.
Take out require ‘pry” when I publish gem.

<created pry and nokogiri dependencies and bundled>

Ready to scrape.
Build another scrape method
articles <<self.scrape_longs
articles <<self.scrape_shorts

def self.scrape_longs
   doc = Nokogiri::HTML(open(“https://seekingalpha.com/stock-ideas/long-ideas”))

Because using open will need to require ‘open-uri’ in lib/sa_articles.rb
Use binding.pry to figure out how to scrape webpage.
Run ./bin/console

Run SaArticles::Articles.scrape_longs
Gets to pry 
>doc.to_html

<doc to html to begin scrape longs>

Displays html from https://seekingalpha.com/stock-ideas/long-ideas
Right click on webpage and left click on Inspect Element

<long scrape title>

Select the article title = doc.search("a.a-title").text.strip

<long scrape url>

Select the article url = doc.search('div.a-info a').last.text.strip

<long scrape author>

Select the article author. This was extremely difficult to resolve. As the title was the first href followed by varying number of horizontal bullet points. 

Articles had a maximum of 6 bullet points:
Editors’ Pick is the first bullet point if chosen.
Stock symbol the second bullet point.
Timestamp the article was created the third bullet point.
Author of the article is the fourth bullet point.
Comments link being the last bullet point.   

The issue was there was no uniformity of thes horizontal bullet points. 
Few articles were designated as Editors’ pick and sometimes articles did not have comments. 
The only constant attributes is stock symbol, timestamp and author.

Regrettably the patterns that allowed me to pull the title and url for the articles did not work with authors. After a week of not making any progress I scheduled a meeting with Corinna Brock Moore to troubleshoot to resolve.

<Scrape author attempts>

Copy and update scrape_longs selectors to create scrape_shorts selectors

During a CLI gem project study group with Corinna Brock Moore it was suggested to use https://github.com/dannyd4315/worlds-best-restaurants-cli-gem/tree/master/lib/worlds_best_restaurants as the template instead of the video walkthrough.
<Restaurant to SA templates>

So while keeping the folder and file structures I started from scratch. Using the “Worlds Best Restaurants” CLI as the template.

I copied out the cli.rb, restaurant.rb and scraper.rb.

Then I replaced the restaurant information with the seekingalpha article information cli.rb, articles.rb, long_scrape.rb, short_scrape.rb and version.rb.

Next I had to merge the cli format with the walkthrough video with the restaurant. As I was asking the user which type of trading idea they were interested in reading long or short articles.

Then needed to scrape the pertinent long articles or short articles depending upon the user input.
I had the user decide the number of the article to begin with and listed 10 articles in a list. 
Next the user would decide which article they were interested in reading more about.
Lastly ask the user if they would like to read another article? If yes repeated process over if no Check back later for more articles.


DO ONE THING AT A TIME!
THINK IN VERY SMALL STEPS!

THINK ABOUT BUILDING A PROGRAM FROM SCRATCH, ONE OBJECT ONE METHOD AND ONE REQUIREMENT AT A TIME!

FIGURE OUT A WAY TO MAKE SOME PROGRESS WHEN PROGRAMMING!

BETTER WAY TO PROGRAM THINK ONLY ABOUT TWO THINGS NEED TO DO,  AS YOU DO THOSE TWO THINGS THE NEXT THINGS WILL REVEAL THEMSELVES!
