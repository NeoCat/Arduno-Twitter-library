# Twitter client library for Arduino
A library to post tweets to Twitter from Arduino with an Ethernet Shield.
This library uses relay server on GooleAppEngine to handle OAuth authentication:

http://arduino-tweet.appspot.com/

## Example
Some sample sketches for Arduino included(/examples/).

 - SimplePost : Send a tweet once Arduino is reset. (Note that duplicated tweets
   will be rejected by Twitter.)
 - Twitter_Serial_GW : Send tweets based on the input from the Serial.

## How to use
 + Add this library to your Arduino IDE.
 
 + Get a token to post a message using OAuth from the relay server:
   
   http://arduino-tweet.appspot.com/ -> "Step 1."
    
   When you allow the access, your token is issued.

 + Pass your token to the constructor of Twitter class, like:

        Twitter twitter("YOUR-TOKEN");

## Reference
### Creation
You need to create an instance of Twitter class like below:

    Twitter twitter("YOUR-TOKEN");

You need also begin Ethernet library.

### Functions
#### bool post(const char *message)

Begin posting the specified message to Twitter. If connection to twitter.com is established successfully, this function returns true.

If failed to connect Twitter, returns false. (Check Ethernet is correctly configured.)

Posting is not done yet even it returns true. You should call twitter.checkStatus() periodically or twitter.wait().

#### bool checkStatus(Print *debug = NULL)

Check if the posting request is running. Returns true if it's still running.

You can specify the debug argument to output the response from server for debugging, e.g.

    checkStatus(&Serial);

#### int status(void)

Returns the HTTP status code in response from Twitter, e.g. 200 - OK.

Only available after posting the message is done and checkStatus() returns false.

#### int wait(Print *debug = NULL)

Wait until posting the message is done. Return value of this function is HTTP status code in response from Twitter.

Equivalent to the code below:

    while (checkStatus(debug));
        return statusCode;

