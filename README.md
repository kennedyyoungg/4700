What's in the submission files?

Makefile : An Null Makefile
client : An sourcecode. Written in Python.
README.md : This file. It contains all explanation regarding the source code.
secret_flags : A file with secret_flags. TCP, TLS respectively.
Debug Mode

current_log_level = logging.DEBUG
Use logging.DEBUG for the debugging mode. This will show every single step.
Use logging.NONE for the production mode. All messages will be suppressed.
Challenges

1. Data Structure Selection

I started with Trie initially. However, I realized that Trie finds data filtering and frequency sorting difficult. I simply switched to the simple list and let my code inspect every entity in it (linear search). Surprisingly, the performance is still good!

2. Python only issues

The variable name Northeastern-usernamecould not be implemented in Python due to the programming language's semantics. I used username for the alternative solution.

3. TLS Issues

The SSL library uses the local machine's CA database, which causes a connection error on a different system. As a result, the CA verification was disabled (NOT SAFE). However, it will work on any computer.

#ADDED : SSL CA Check Bypass
context.check_hostname = False
context.verify_mode = ssl.CERT_NONE
4. Guessing Algorithm

I used a simple linear search without sorting for the guessing algorithm. I realize that the current simple algorithm yields acceptable performance. So, I decided not to implement any efficiency optimization!

In addition, I used simple pop. There's almost no randomness in guessing logic. I was initially trying to implement randomness in word guessing, but I noticed that simple pop from the list It still yields good performance, so I will just keep using the simple method!

Approach

Prolog Logic

Init the logger
Argument Parsing
Port Selection Logic 4. If provided = Use that 5. Otherwise, Use the default port!
Wordlist Loading : File read and creating a word list
Socket Creation 8. If TLS? Implement the wrapper
Start the Game!
Game Logic

Connection Init

Send HELLO Message (HELLO, Username) -> JSON and SEND
Receive START Message (START, GameID) -> Use the GameID for the entire game
Send first Guess -> Pop the word from the wordlist. Adding in the used word list.
Guessing Logic

BYE with FLAG -> DONE! 2. Print flag and exit the program
BYE without FLAG -> Exception!
Retry 4. Get the most recent guess + marks 5. Filter the wordlist based on the previous result. 6. RETRY!
Other cases -> Exception!
Word Filtering Logic

There are 3 cases

[2] : Exist, Position is correct
[1] : Exist, Position is incorrect
[0] : Not exist
There are occasions when the same character can exist in different positions. To guarantee the total number of characters, I made a counter using the collection library to check the number of letters.

First, the program will review every word in the wordlist (candidates). Iterating every letter, the program will compare the pre-defined rule.

   [First Ittr]  : APPLE, AMPLE, {0, 1, 2, 0 ,0}
   [Second Ittr] : A vs {0, 1, 2, 0 ,0}, P vs {0, 1, 2, 0 ,0}, P vs {0, 1, 2, 0 ,0} ...
   
   When the Letter matches the pre-defined condition, it will return TRUE. Otherwise, False.
When the mark has [2], the exact same letter has to be located in the exact same order
When the mark has [1], it could exist somewhere in the word. So, less intensely, search for the string to decide.
When the mark has [0], that letter should not exist. Therefore, the function will return 0.
There are some side cases that I have added to the program comment.

In any case, when the counter becomes negative, the function will yield zero because it's not possible.

Also, the program compares the filtered word list to the used word, and if the word exists in the used word list, simply avoid it.

How to run?

./client <-p [int:Port Number]> <-s> <[string:hostname]> <[string:Northeastern-username]>
Parameters

Required

string:hostname : FQDN or IP address
string:Northeastern-username : NEU ID not EMAIL
Optional

-p [int:Port Number] : Port number. Default value 27993,27994(Secure).
-s : TLS Flag
Testing
