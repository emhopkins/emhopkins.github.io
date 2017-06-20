---
layout: post
title:  "My First Gem - The Office Quote Generator"
date:   2017-06-20 06:58:24 +0000
---

 
**"I'm sort of a master of distraction. When I was a kid, my mom received complaints left and right from my teachers that I would distract everyone around me." - Michael**
 
 
For my first gem I decided to build a CLI app that allows a user to view different quotes from the TV show, "The Office".  And I have to admit, I spent more time than necessary testing the CLI because I love this show so much.  I may have created a monster.  I scraped my data from the site https://www.tvfanatic.com/quotes/shows/the-office/, which was the best selection of quotes from The Office I could find.  
 
**"Speaking as a former baby, don't get too hung up on baby names." -Andy**
 
I started by creating my basic project structure, which changed throughout the project.  I ended up with this structure:
 
```
├── CODE_OF_CONDUCT.md
├── Gemfile
├── Gemfile.lock
├── LICENSE.md
├── README.md
├── bin
│   └── the-office-quote-generator
├── lib
│   ├── character.rb
│   ├── office_quote_controller.rb
│   ├── quote.rb
│   ├── scraper.rb
│   ├── the_office_quote_generator
│   │   └── version.rb
│   └── the_office_quote_generator.rb
├── spec.md
└── the-office-quote-generator.gemspec
 
```
 
I started by creating my scraper class in lib/scraper.rb.  Prior to actually creating objects, I created hashes of quotes and characters in order to test that I was scraping my data correctly.  I also used pry to test out the different css selectors that I used when accessing elements of the Nokogiri document.  To get the data exactly the way I wanted was actually a bit of a challenge.  I used Ruby's .strip function to get rid of extra leading and trailing spaces, but I had an issue with the way all of my dialogue quotes were formatted when using Nokogiri's .text function for the css element containing the quote's content.  In some cases (quotes with characters), the quote would take up one paragraph.  However, with dialogue quotes, each character's line would be separated by a `<br>` tag.  When using .text, these would be taken out and each line of dialogue would be pushed against the next with no space.  I needed a solution that would fix these issues, but still work for situations with normal one paragraph quotes.  So, rather than .text, I used .to_s for these elements.  This converted the whole element (including the html tags) to a string.  Once I had the data like this, I could .gsub! to sub out the tags with space.  
 
**Michael: There are four kinds of business: tourism, food service, railroads, and sales. And hospitals/manufacturing. And air travel.**
 
Once I had the data getting parsed correctly, I began creating my Quote and Character classes.  I started by basing a lot of my functionality on the Song and Artist classes we built in previous labs.  Like Songs and Artists, Quotes belong to a specific Character and a Character can have many quotes.  Since I knew I some quotes were dialouge, I handled those separately.  If there was no single character assigned to a quote, I initialized the Quote object without a Character.  In initialization in the Quote class I stored all dialogue quotes in the class variable array, `@@dialouge_quotes` and all quotes in the class variable array `@@all`.  When a new Quote is initialized, I check to see if a character object exists for the new quote.  If it does, the quote get's added to the Character object's quotes.  If it does not exist, a new Character object is created. Once all the Quote and Character objects have been created, the CLI begins.  
 
**Plan a party, Angela. Oh! And the entire world will see it. Oh! And here's $65.00 for your budget. Oh and here are four idiots who'll do nothing but weigh you down. Oh. And your cat's still dead. -Angela**
 
For the CLI, I gave the user 3 basic options to hear quotes: 
 
Please select from the following options: 
1. Choose a quote from a specific character
2. Hear a dialogue quote (between multiple characters)
3. Hear a random quote
 
Each option will invoke a different class method from the Quote and Character classes.  Option 1 allows the user to get a little deeper and chose between either one random quote for a character or see a list of all of their quotes.  I also considered listing all the quotes as an option, but when I tested all quotes, I couldn't make it to the end.  There's just too much data for them to display in a reasonable time.  I ended up deciding to display random quotes and I think that resulted in a much more manageable user experience.  After testing the CLI, I decided I wanted to publish my gem on rubygems.org.   
 
**I resent the implication that I would keep that secret. I can't and I won't. -Andy**
 
My gem can be found here: https://rubygems.org/gems/the-office-quote-generator.  This part was challenging, because I had to restructure my project a bit to work as a gem.  I also had to do a bit of troubleshooting with my gemspec until I got everything working correctly.  And many versions later, I had my gem working as I'd hoped!  The biggest struggle for me was figuring out the correct way to require my gem dependencies as well as the proper way to include all my files.  I ended up including all my lib files in the executable, the-office-quote-generator.  I also found that I needed to use `s.add_runtime_dependency` in my gemspec (rather than `s.add_development_dependency`) in order for someone to install my gem (without `--dev`) and run my executable successfully.  After getting all of these final things worked out, I was so excited to install my gem (in a new directory) and run the executable.  
 
I think I deserve to watch an episode of the office. 
 
**[singing] Come tomorrow, feel no pain! Feel no pain! Toby! Toby! Tobee-yy! Toby's goin' away! See ya! He's outta here! See ya! He's outta here! Ohh! Goodbye Toby! Goodbye Toby! Goodbye Toby! Goodbye Tooo-by! -Michael**
 

