# Language-Style-Transfer

Recieve input of either an English or Spanish phrase and translate the phrase into the grammar stylings of the other language. Used Natural Language Toolkit (NLTK) to detect the POS structure of English sentences and Spaghetti Tagger github repository to detect POS structure of Spanish Sentences. Wrote code to manually rearrange POS of sentences to resemble that of the other language. Used Pygame to display the results of the language style transfer. When the code is run in Pycharm, the user is asked to input an English sentence of up to or around 13 words. Then a display window pops up of, made with Pygame, which displays the original sentence and its syntax tree (with chunks that display when certain parts of speech are in a certain order, based on English grammar rules we switched to Spanish in the main function). The pygame also displays the Spanglish sentence and its syntax tree (with chunks based on the rules we implemented in our code. 

# Must install or already have these packages on your computer (based on mac) and have python 3.9 installed on computer. use pip3 install in order to install necessary packages

* PyCharm CC
* os
* sys
* numpy
* IPython
* nltk
* pygame
* googletrans==3.1.0a0 --> MUST BE THIS VERSION (NOT googletrans) OR TRANSLATOR AND OTHER CODE WILL NOT WORK
* spaghetti
* ghostscript --> MUST HAVE THE LASTEST VERSION OF PYTHON INSTALLED OR YOU WILL GET AN ERROR WHEN TRYING TO CONVERT .ps FILES OF SYNTAX TREES TO .png

# TO RUN THE CODE

To see syntax trees for English --> Spanish:

* Copy and Paste the entire PyCharm cell from the COLAB document into a PyCharm file
* Run the code
* Wait for prompting
* Type in an English sentence

To see what's been done with Spanish --> English:

* Type a spanish string into spanglishMachine() the cell at the top of the colab and run it
* You can also go to both the "Testing" folders to try out my sentences I used to develop the rules code :)



# spanglishMaker()

Main function. Recieves a string and eventually returns an array of tuples. Each tuple's index 0 contains a string of the corresponding word in the sentence and index 1 contains a string code corresponding to the part of speech.

* Uses google translate to determine the source language
* If src = English --> tag using NLTK --> toSpanishStyle(tagged sentence)
* If src = Spanish --> tag using Spaghetti Tagger --> toEnglishStyle(tagged sentence)

# English --> Spanish

## toSpanishStyle()

Take in a tagged array and returns a tagged array. More details in the comments of the code, but in general focusses on variations between English and Spanish ordering of pronouns, nouns, possessives, and objects. Uses getStartNoun() to find the beginning index of any noun clump (the largest possible being determiner + adjective + noun + noun) for use in moving nouns arouund verbs and each other.

Here are the 10 rules in some, with some of the nicher ones explained:

* Rule 1: Move possessive nouns after what they possess ------ Alvin's book --> libro de Alvin --> book of Alvin
* Rule 2: Put noun after Verb When It's a Verb applied to the second noun in the Sentence (because switch more common with long sentences) ------ although not a requirement in Spanish, this is not a uncommon practice in spanishto switch up sentence structure ------ I don't remember the moment in which Sarah wrote --> No recuerdo el momento en que escribió Sarah --> I don't remember the moment in which wrote Sarah
* Rule 3: verb + pronoun1 + preposition + pronoun2 --> pronoun 2 + pronoun 1 + verb ------ Sarah wrote it for me --> Sara me lo escribió --> sarah me it wrote
* Rule 4: Remove Pronouns Directly Before Conjugated Verbs If not 2 or 3 pronoun group before verb ------ because of how verbs are conjugated, pronoun subjects are rarely needed. However, when there are pronouns at the beginning of the verb denoting objects, this can become confusing, so pronouns are kept as subjects in this case.
* Rule 5: Noun + Verb + Pronoun --> Pronoun + Verb + Noun ------ Another not required rule in Spanish but appropriate for the project, this is a convention in Spanish poetry so that sentences end on the most salient word. ------ Bridget wrote it --> It escribió Bridget --> It wrote Bridget
* Rule 6: Adjective + Noun --> Noun + Adjective ------ blue eyes --> ojos azules --> eyes blue
* Rule 7: "The" Before Every Non-Proper Noun ------ this is just a thing in Spanish
* Rule 8: remove Attributive Nouns ------ coffee cup --> taza de café --> cup of coffee
* Rule 9: If "not" or "n't" after verb, move "not" to before verb ------- I'm not happy --> no soy feliz --> Not am happy
* Rule 10: VB + IN/TO + PRP --> PRP + VB ----- along the same lines as the other pronoun rules, but had to add at end to avoid messing with the other steps

Sources:
* https://www.thoughtco.com/grammatical-differences-between-spanish-and-english-4119326#:~:text=Word%20order%20is%20less%20fixed,subjunctive%20mood%20than%20English%20does.
* my intimidatingly powerful knowledge (high school AP Spanish)

## getStartNoun()

Just explained this

# Spanish --> English

This code is much less involved than the other, largely because Spaghetti Tagger pulls from a lot fewer words than NLTK. For example, while Spaghetti tagger correctly detects "bonita" as a singular adjective, it doesn't detect "bonita**_s_** as an adjective at all. This proved frustrating. We eventually decided that it wasn't worth putting too much time into this part of the code if Spaghetti Tagger was only going to detect grammatically correct Spanish sentences half the time, but the code still has some functionality. Namely, it:
* Rule 1: switches adjectives to the beginning of verbs
* Rule 2 pt. 1: creates attributive nouns by switching noun formats from noun1 + "de" + noun2 to noun2 + noun1
* Rule 2 pt. 2: models English possessive nouns by switching possessed noun + "de" + possessor noun to possessor noun + "'de" (a distinct tuple we made with an index 1 of "POS," corresponding to the NLTK code for possessive nouns
* Rule 3: uses conjugation of verbs to add pronoun subjects before verbs that lack subjects


# Illegal chuck pattern
* VB + PRP + TO + PRP

# syntaxTreeGenerator()
This function creates a syntax tree with chunks of various parts of speech defined from English grammar rules. The input is the original sentence inputed by the user, which is tokenized and tagged in the funciton, and the output is a syntax tree image using NLTK toolkit, which is saved to a .ps file and converted to a .png file. Can only be run in pycharm, not colab (which cannot make display windows)

# spanglishSyntaxTreeGenerator()
This function creates a syntax tree like the original sentence one, but the input it the array made by the spanglishMachine function, and the chunks are based on Spanish grammar rules from the above functions. It is also saved to a .ps file and converted to a .png file. Can only be run in pycharm, not colab (which cannot make display windows)

# Pygame Display
We used the pygame toolkit to display the various aspects of our project! We run this in pycharm, since colab can't make display windows. The pygame shows an image of all of the NLTK POS tags on the left. On the right, the original sentence (labelled) is read in from the text files created within the spanglishMachine function. Below it is the image of the syntax tree for the original sentence (labelled). Then the same two aspects are shown below that for the Spanglish sentence and syntax tree. The longer a user-inputed sentence is, the farther to the left it is displayed. 


<i>Hour breakdown:</i>
  
    3/28: Brainstorm possible projects to pitch at Monday office hours (Catherine & Megan) - 0.5 hours
    3/29 (10:30am): Discussed ideas with Lakshmi in morning office hours (Megan) - 0.5 hours
    3/29 (6:00pm): Touched base on what Lakshmi told Megan in office hours, chose project and brainstormed new directions (Catherine & Megan) - 1 hour
    3/29 (7:20pm): Talked to Lakshi in office hours about new idea. Got idea of feasibility and what to research. Cut short because I had a Grubhub order (tacos). (Megan) - 0.5 hours
    3/30: Created this Github and wrote up project proposal rought draft (Megan) - 1.5 hour
    4/3: Researched POS models (Megan) - 1 hour
    4/4: Researched Natural Language Toolkit, POS models, and chunking (Catherine) - 1 hr
    4/4 (4:00pm): Pair Programmed downloading NLTK and beginning POS Models in Spanish and Sanskrit, Catherine drove, couldn't push to github will figure out in office hours how to commit from colab (Catherine & Megan) - 3 hours each
    4/5 (6:30pm): Tried to download github repo for use in Colab (didn't figure it out yet). Also researched how to train NLTK corpuses. (Megan) - 1 hour
    4/6 (6-7pm): waiting in line for office hours (Catherine) - 1hr
    4/6 (7-8pm): waited in line for office hours. Asked a question (Megan) - 1hr
    4/9 (10-11pm): researching sanskrit computational linguisticss (Catherine) - 1hr
    4/10 (4:30-5:30): figured out how to push to github!! (Megan and Catherine) - 1hr each
    4/11 (7-8:30): office hours to figure out spaghetti/spanish tagger (Megan and Catherine) - 1.5 hrs each
    4/12 (9-10pm): experimented with now-functioning spaghetti tagger and different spanish sentences (Megan) - 1 hr
    4/14 (3:30-4:30): trying out sanskrit tagging (Catherine) - 1hr
    4/14 (8-9:30): pair programming, working on Spanish tagger ideas and figuring out googletrans (Megan and Catherine) - 1.5 hrs each
    4/15 (1:45-2:45): figured out how to translate from Spanish to English and resolved translator problems (Catherine) - 1hr
    4/17 (7-9:30pm): wrote chunk parsing syntax tree code and starting trying to create a virtual display in colab (Catherine) - 2.5hrs
    4/17 (7-9:30pm): wrote spanglishMachine() and experimented with indexing through nltk and spaghetti list outputs to determine POS. Pushed code to wrong repository. (Megan) - 2.5 hrs
    4/19 (7-8:30pm): Office hours to try to display syntax trees and work on main function (Megan and Catherine) - 1.5 hrs
    4/19 (9-11pm): Figured out PyCharm and made function that can display syntax trees with chunks in PyCharm!! (Catherine) - 2 hrs
    4/19 (9-11pm): Modified spanglishMachine() and created toSpanishStyle() and toEnglishStyle(). Added 2 grammar rules to toSpanishStyle() (Megan) - 2 hrs
    4/20 (12-2:30am): Continued work on toSpanishStyle(), adding 3 new grammar rules and brainstorming a 4th (Megan) - 2.5 hrs
    4/20 (3-4:30am): toSpanishStyle(): added step 3.5 for longer sentences. Looked carefully at Spanish POS tags and tried to discern different letter meanings. Added a step for moving adjectives before nouns in toEnglishStyle() (Megan) - 1.5 hours
    4/20 (11am-1:30pm): added another rule to toSpanishStyle() to account for possessive nouns. Also created getStartNoun() to streamline some code. (Megan) - 2.5 hrs
    4/20(1:30-3pm): added bunch of pronoun stuff to toSpanishStyle() (Megan) - 1.5 hrs
    4/20 (3:45-4:15): talking through project plans, brainstorming next steps of project, and how to do user input (Megan and Catherine) - .5 hrs
    4/20 (4:30-5:30pm): user input research (Catherine) - 1 hr
    4/20 (7:00-8:30): fixed bugs in toSpanishStyle() and worked with Catherine a little bit to figure out a display tree typo (Megan) - 1.5 hours
    4/20 (6pm-12:30am): worked in pycharm (copied code to bottom of colab) to do user input of sentence in syntax tree function and began making pycharm game to pisplay spanglish generator; initialized the game and made text in it, then began working on converting .ps file of syntax tree to .png file to display in pygame (Catherine) - 6.5 hrs
    4/21 (11:30am-1pm): researched how pycharm works (Megan) - 1.5 hrs
    4/21 (8-9:30am): Researched more about how to use pygame and different tools and things you can implement in it (Catherine) - 1.5 hrs
    4/21 (9:30am-1:30pm): figured out how to convert .ps to .png (took so long oh my goodness had to figure out ghostscript and reinstall python), got syntax tree image onto pygame, and then spent rest of time figuring out how to center images and and place them in different locations on the screen (Catherine) - 4 hrs
    4/21 (2:30-4:30pm): started toEnglishStyle() (Megan) - 2 hrs
    4/21 (5:00-5:30pm): called Catherine to find an error in her code (Megan) - 0.5 hours
    4/21 (5:30-8:30): Created changeSpanTags() and added to toEnglishStyle() (Megan) - 3 hours
    4/21 (8:30-9:00pm): Called Catherine to discuss POS groups she needs to chunk to create syntax trees (Megan) - 0.5 hrs
    4/21 (9:00-10:00pm): Last touches on toEnglishStyle() and Cleaning up Collab (Megan) - 1 hr
    4/21 (2pm-2am): Oh my goodness so much: first figured out how to download the original and Spanglish sentences as text files, then how to read those into the pygame display, made a new syntax tree function for the spanglish sentence and Megan helped pair program that while I drove, made chunks in the chunk parsers in the syntax tree functions based on all of the syntax rules for English and Spanish, debugged code, moved everything around a lot on the pygame display window, tested out sentences, made it so the stuff on the right of the display window moves to the left the longer the sentence inputted is, the phone calls that Megan descibed above, and commenting all of my code (Catherine) - 12 hours
    4/21 (10:00pm-3:30am): fixing all the mistakes I made in my tiredness. Then added to ReadM. Then added more rules to toSpanishStyle() cause I felt like it. Probably the new meds lol (Megan) - 5.5 hrs
  
<i>Running Total (for our reference only, will delete @ end)
      
    Megan: 45.5 hours
      
    Catherine: 45 hours
