# PortSwigger reflections
This file is mainly meant for the purpose of logging my own thoughts and reflections about any PortSwigger assignments I complete for the purpose of documenting my progress and to return to the teacher in case documentation of any completed PortSwiggers labs is needed in the future.

## Topic: SQL Injection
1. SQL injection vulnerability in WHERE clause allowing retrieval of hidden data →
  - SQL injection seems like its really easy to get to something you are not supposed to, but I’m sure there are ways to stop it.
  - I learned how to do very basic request edits to see hidden information from a table. Modifying the GET statement wasn’t something I knew how to do, I had to do the tutorial to understand what I was supposed to do.
  - The most difficult part I would say would probably have to be just understanding the command structure and how I’m supposed to format the statement. I understood it after a while though.    

2. SQL injection vulnerability allowing login bypass →
  - This lab is building off of what was learnt from the previous lab, moreso reinforcing the already learned material than anything new. At first I thought “there’s no way it’s that easy to login to an administrator account in an unprotected system” but I was proved wrong.
  - There wasn’t any significant increase in challenge from the last lab, and seeing as how I learned in the last lab how to modify the GET statement, editing took only a moment. Although, the way to format the request was still a bit odd to me.

3. SQL injection with filter bypass via XML encoding →
  - The use of obfuscating the UNION SELECT was not something I thought was possible, but I suppose everything can be exploited if you poorly protect it. The WAF protections in place were at least some form of protection, which I believe to be the first in any of the PortSwigger labs I've come across so far, so that's pretty nice.
  - Unlike most of the other labs, this one was "Practicioner" level, so I was expecting a severely noticeable jump in difficulty, but it seems to not be that much more difficult than the "Apprentice" labs. At least at this phase. Although, the most annoying part was having to sit there for like 5 minutes while my Burp Suite was installing the obfuscating extension into itself so I could pass the lab.  
  - Funny enough the hardest part was finding the stock check feature because I didnt think to scroll down the products page originally. After that it wasn't so difficult and I was able to pass it with minimal looking at the hints.

4. SQL injection attack, querying the database type and version on Oracle →
  - Modifying the requests themselves seems to be at least a little annoying to me still. I couldn't quite understand why my inputted '+UNION+SELECT+' command wasn't working when I tried it, but the one from the provided solution later worked even though it seemed to be completely the same string. Regardless, I should read up on the structures of GET and POST rqeusts a little before the next lab because it's becoming an issue.
  - The most difficult part was trying to format the SQL query to fit the GET request's "category" parameter. It was annoying trying to put it together until I found a cheatsheet for it on the internet and with a little help from the community solutions.

5. Blind SQL injection with conditional responses →
  - I found this lab to be particularly interesting. Realistically, a lot of sites would have at least some form of protection agains plain SQL injection, but I can guarantee at least a solid couple would forget to take into account the different objects and specific replies when the user inputs something. In this case the website would leave a "Welcome back" message on the site whenever the user logged in, which we were able to exploit in a blind SQL injection to confirm or deny statements. This made it simple to slowly gather the password of the administrator user through Burp Intruder by slowly iterating over each length to get the length of the administrator password (20 letters), and making advantage of the simple list attack, getting the password of the administrator by slowly iterating over each position in the string and checking if the website has the "Welcome back" message.
  - The most difficult part of this was still formatting the SQL correctly. I missed a single quotation mark and had to spend a solid 20 minutes trying to troubleshoot on where I went wrong before I noticed it. I should read up on the SQL documentation and syntax a bit more.

    
## Topic: Authentication
1. Username enumeration via different responses →
  - Brute-forcing is a tedious process, thought it has the possibility of getting results efficiently. Keeping the “wrong username/password” responses the same was highlighted well, since with just that error and a sample data you could easily guess a username, and the objective just became guessing the password, which is a lot easier than keeping track of two running brute-forces at once.
  - Hardest part was still trying to understand where everything was. It took me more than a while to figure out I was supposed to use the “Intruder” tab on Burp. I also added the wrong part of the syntax to be brute-forced initially, so it took me twice as long to get the correct username.  

2. Password reset broken logic →
  - This was probably the simplest PortSwigger lab I completed with the Burp environment. Using plaintext addresses with flawed logic is obviously very dangerous, so it wasn’t a particularly challenging task to complete, anyone could do it even without any practice. The most challenging part was that I accidentally selected the wrong request at first and looked at it for a couple of minutes before realizing.  

3. 2FA simple bypass →
  - The flaws in the beginner level authentication labs are always very simple, and very easy to fix in any system. It's important to keep in mind to properly implement the 2FA if you are going to add it to your application, the entire point of it is to block the login unless the authentication code is given, so the user should never be switched to the "logged in" state before this code is given. Implementing some additional checks is a crucial step to protecting user accounts.
  - The lab was not difficult enough to constitute anything as being difficult. It was simply changing the address during login, which let me into the victim's account.

4. Username enumeration via subtly different responses →  
  - The main point of this lab was to gain the username for the application through use of different error messages. The site would have different error messages depending on if only the password was wrong or if the email address was also wrong, which would allow us to validate the existence of the email address and then focus all out efforts on getting just the password.
  - At first I loaded the common usernames payload from the lab page to the login request in Intruder. I first tried to manually look through the messages to see which one was the correct username, but I didn't anticipate the difference between the messages being just a missing dot so it took me quite a while until I just decided to use the Grep - Extract setting to match the invalid password message to the correct response. After that it was a simple case of just attacking the password with the retrieved username and logging in after getting a positive return.
  - The lab was not particularly difficult to complete but it was a bit tedious, especially when I had to rerun the attack multiple times. 

## Topic: Access Control
1. Unprotected admin functionality →
  - Solving this lab was probably the easiest one I’ve had to do so far, because the entire task was done through just managing the URL. Obviously there needs to be some level of protection so that you cannot just simply go to a page, add /administrator-panel to the end of the URL, and then have access to delete accounts. That seems like common sense.
  - The trickiest part of this lab was trying to figure out the string of text to add to the end of the URL to give me access to the admin panel to delete users, but manually brute-forcing it took a maximum of 2 minutes, so it wasn’t that tricky. I suspect in a real scenario like this it would not be quite so simple, but it would still be ridiculously easy and low-security.

2. User role can be modified in user profile →
  - This one was slightly harder to complete than the previous lab, because unlike with the first one, to access the deletion functionality in the admin panel you needed to be logged in as a user with a roleid of 2, so there was at least some level of protection.
  - I tried to just intercept the POST and add the “roleid”:2 into the message, but it was blocked, so I tried to do it with Burp Repeater. I was struggling with it for a while until I realized I forgot a comma from the syntax which is why it wasn’t working. I managed to delete 
