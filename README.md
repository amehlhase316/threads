##### Author: Instructor team SE, ASU Polytechnic, CIDSE, SE


##### Purpose
This program shows a very simple client server implementation. The server
has 3 services, echo, add, addmany. Basic error handling on the server side
is implemented. Client does not have error handling and only has hard coded
calls to the server.

* Please run `gradle Server` and `gradle Client` together.
* Program runs on localhost
* Port is hard coded

## Protocol: ##

### Echo: ###

Request:
    {
        "type" : "echo", -- type of request
        "data" : <String>  -- String to be echoed 
    }
General response:
    {
        "type" : "echo", -- echoes the initial response
        "ok" : <bool>, -- true or false depending on request
        "echo" : <String>,  -- echoed String if ok true
        "message" : <String>,  -- error message if ok false
    }
Success response:

    {
        "type" : "echo",
        "ok" : true,
        "echo" : <String> -- the echoed string
    }

Error response:

    {
        "type" : "echo",
        "ok" : false,
        "message" : <String> -- what went wrong
    }

### Add: ### 
Request:
    {
        "type" : "add",
        "num1" : <String>, -- first number -- String needs to be an int number e.g. "3"
        "num2" : <String> -- second number -- String needs to be an int number e.g. "4" 
    }
General response
    {
        "type" : "add", -- echoes the initial request
        "ok" : <bool>, -- true or false depending on request
        "result" : <int>,  -- result if ok true
        "message" : <String>,  -- error message if ok false
    }
Success response:
    {
        "type" : "add",
        "ok" : true,
        "result" : <int> -- the result of add
    }
Error response:

    {
        "type" : "add",
        "ok" : false,
        "message" : <String> - error message about what went wrong
    }

### AddMany: ###
Another request, this one does not just get two numbers but gets an array of numbers.

Request:
    {
        "type" : "addmany",
        "nums" : [<String>], -- json array of ints but given as Strings, e.g. ["1", "2"]
    }
General response
    {
        "type" : "addmany", -- echoes the initial request
        "ok" : <bool>, -- true or false depending on request
        "result" : <int>,  -- result if ok true
        "message" : <String>,  -- error message if ok false
    }
Success response:

    {
        "type" : "addmany",
        "ok" : true,
        "result" : <int> -- the result of adding
    }

Error response:

    {
        "type" : "addmany",
        "ok" : false,
        "message" : <String> - error message about what went wrong
    }

### CharCount: ###
This one will count the number of characters in a given string, with the option to search for a specific character in the string. 
If the user specifies a character to search for, alter the request to include the character and return the number of times that character is found in the string.
If the user does not specify a character to search for, return the number of characters in the string.

Request:

    {
        "type" : "Charcount",
        "findchar" : false, -- value is false to denote general character counting
        "count" : <String> -- String to search through e.g. "sally sold seashells down by the seashore"
    }

Request for searching for specific char:

    {
        "type" : "charcount",
        "findchar" : true, -- value is true to denote specific character search
        "find" : <char>, -- if findchar is true -- character in String to search for e.g. "s"
        "count" : <String> -- String to search through e.g. "sally sold seashells down by the seashore"
    }

General response

    {
        "type" : "charcount", -- echoes the initial request
        "ok" : <bool>, -- true or false depending on request
        "result" : <int>,  -- result if ok true - number of the given character or overall characters in the String 
        "message" : <String>  -- error message if ok false
    }

Success response:

    {
        "type" : "charcount",
        "ok" : true,
        "result" : <int> -- number of the given character or overall characters in the String 
    }

Error response:

    {
        "type" : "charcount",
        "ok" : false,
        "message" : <String> -- error message about what went wrong
    }


### Storyboard: ###
This one will be to add to or view a storyboard kept on the server. 
The storyboard will be maintained on the server. Users can request to add a sentence with a username to the story, which should be added 
to the end of the server's existing story to extend it. 
The storyboard should persist across users and its order, as well as which user added which sentence, should remain consistent even if the server crashes. 
Each username should be unique and once a user adds their name and sentence to the story, that sentence should be immutable. A user who already added a sentence cannot add a new sentence using the same username. 

The idea is that each user can add onto a running storyboard on your server. It is up to the client side on how to display that
information back to the user, but the response must follow the protocol given below - with the first added sentence/user at the 0th index.

Request to add to the storyboard:

    {
        "type" : "storyboard",
        "view" : false, -- false to denote we are adding, not viewing
        "name" : <String>, -- if view is false -- name of the user adding
        "story" : <String> -- if view is false -- string the user is adding to the story (usually one sentence)
    }

Request to view the storyboard:

    {
        "type" : "storyboard",
        "view" : true -- true to denote we want to view the storyboard
    }

General response

    {
        "type" : "storyboard", -- echoes the initial request
        "ok" : <bool>, -- true or false depending on request
        "storyboard" : [<string>],  -- result if ok true - returns the story board
        "users" : [<string>], -- result if ok true - returns the users who added to the story board
        "message" : <String>  -- error message if ok false
    }

Success response:

    {
        "type" : "storyboard",
        "ok" : true,
        "storyboard" : [<String>],  -- story board as json array of Strings e.g. ["Hello there" , "General Kenobi"]
        "users" : [<String>] -- users who added to the story board as json array of String e.g. ["Obiwan Kenobi", "General Grievous"]
    }


Error response:

    {
        "type" : "storyboard",
        "ok" : false,
        "message" : <String> -- error message about what went wrong
    }


### General error responses: ###
These are used for all requests.

Error response: When a required field "key" is not in request

    {
        "ok" : false
        "message" : "Field <key> does not exist in request" 
    }

Error response: When a required field "key" is not of correct "type"

    {
        "ok" : false
        "message" : "Field <key> needs to be of type: <type>"
    }

Error response: When the "type" is not supported, so an unsupported request

    {
        "ok" : false
        "message" : "Type <type> is not supported."
    }


Error response: When the "type" is not supported, so an unsupported request

    {
        "ok" : false
        "message" : "req not JSON"
    }