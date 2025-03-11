# PortSwigger reflections
This file is mainly meant for the purpose of logging my own thoughts and reflections about any PortSwigger assignments I complete for the purpose of documenting my progress and to return to the teacher in case documentation of any completed PortSwiggers labs is needed in the future.

## Topic: SQL Injection
SQL injection vulnerability in WHERE clause allowing retrieval of hidden data →
  - SQL injection seems like its really easy to get to something you are not supposed to, but I’m sure there are ways to stop it.
  - I learned how to do very basic request edits to see hidden information from a table. Modifying the GET statement wasn’t something I knew how to do, I had to do the tutorial to understand what I was supposed to do.
  - The most difficult part I would say would probably have to be just understanding the command structure and how I’m supposed to format the statement. I understood it after a while though.    

SQL injection vulnerability allowing login bypass →
  - This lab is building off of what was learnt from the previous lab, moreso reinforcing the already learned material than anything new. At first I thought “there’s no way it’s that easy to login to an administrator account in an unprotected system” but I was proved wrong.
  - There wasn’t any significant increase in challenge from the last lab, and seeing as how I learned in the last lab how to modify the GET statement, editing took only a moment. Although, the way to format the request was still a bit odd to me.

## Topic: Authentication
Username enumeration via different responses →
  - Brute-forcing is a tedious process, thought it has the possibility of getting results efficiently. Keeping the “wrong username/password” responses the same was highlighted well, since with just that error and a sample data you could easily guess a username, and the objective just became guessing the password, which is a lot easier than keeping track of two running brute-forces at once.
  - Hardest part was still trying to understand where everything was. It took me more than a while to figure out I was supposed to use the “Intruder” tab on Burp. I also added the wrong part of the syntax to be brute-forced initially, so it took me twice as long to get the correct username.  

Password reset broken logic →
  - This was probably the simplest PortSwigger lab I completed with the Burp environment. Using plaintext addresses with flawed logic is obviously very dangerous, so it wasn’t a particularly challenging task to complete, anyone could do it even without any practice. The most challenging part was that I accidentally selected the wrong request at first and looked at it for a couple of minutes before realizing.  

## Topic: Access Control
Unprotected admin functionality →
  - Solving this lab was probably the easiest one I’ve had to do so far, because the entire task was done through just managing the URL. Obviously there needs to be some level of protection so that you cannot just simply go to a page, add /administrator-panel to the end of the URL, and then have access to delete accounts. That seems like common sense.
  - The trickiest part of this lab was trying to figure out the string of text to add to the end of the URL to give me access to the admin panel to delete users, but manually brute-forcing it took a maximum of 2 minutes, so it wasn’t that tricky. I suspect in a real scenario like this it would not be quite so simple, but it would still be ridiculously easy and low-security.

User role can be modified in user profile →
  - This one was slightly harder to complete than the previous lab, because unlike with the first one, to access the deletion functionality in the admin panel you needed to be logged in as a user with a roleid of 2, so there was at least some level of protection.
  - I tried to just intercept the POST and add the “roleid”:2 into the message, but it was blocked, so I tried to do it with Burp Repeater. I was struggling with it for a while until I realized I forgot a comma from the syntax which is why it wasn’t working. I managed to delete 
