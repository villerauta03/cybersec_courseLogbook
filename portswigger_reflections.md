# PortSwigger reflections
This file is mainly meant for the purpose of logging my own thoughts and reflections about any PortSwigger assignments I complete for the purpose of documenting my progress and to return to the teacher in case documentation of any completed PortSwiggers labs is needed in the future.




# Topic: SQL Injection
**1. SQL injection vulnerability in WHERE clause allowing retrieval of hidden data →**
  - SQL injection seems like its really easy to get to something you are not supposed to, but I’m sure there are ways to stop it.
  - I learned how to do very basic request edits to see hidden information from a table. Modifying the GET statement wasn’t something I knew how to do, I had to do the tutorial to understand what I was supposed to do.
  - The most difficult part I would say would probably have to be just understanding the command structure and how I’m supposed to format the statement. I understood it after a while though.    

**2. SQL injection vulnerability allowing login bypass →**
  - This lab is building off of what was learnt from the previous lab, moreso reinforcing the already learned material than anything new. At first I thought “there’s no way it’s that easy to login to an administrator account in an unprotected system” but I was proved wrong.
  - There wasn’t any significant increase in challenge from the last lab, and seeing as how I learned in the last lab how to modify the GET statement, editing took only a moment. Although, the way to format the request was still a bit odd to me.

**3. SQL injection with filter bypass via XML encoding →**
  - The use of obfuscating the UNION SELECT was not something I thought was possible, but I suppose everything can be exploited if you poorly protect it. The WAF protections in place were at least some form of protection, which I believe to be the first in any of the PortSwigger labs I've come across so far, so that's pretty nice.
  - Unlike most of the other labs, this one was "Practicioner" level, so I was expecting a severely noticeable jump in difficulty, but it seems to not be that much more difficult than the "Apprentice" labs. At least at this phase. Although, the most annoying part was having to sit there for like 5 minutes while my Burp Suite was installing the obfuscating extension into itself so I could pass the lab.  
  - Funny enough the hardest part was finding the stock check feature because I didnt think to scroll down the products page originally. After that it wasn't so difficult and I was able to pass it with minimal looking at the hints.

**4. SQL injection attack, querying the database type and version on Oracle →**
  - Modifying the requests themselves seems to be at least a little annoying to me still. I couldn't quite understand why my inputted '+UNION+SELECT+' command wasn't working when I tried it, but the one from the provided solution later worked even though it seemed to be completely the same string. Regardless, I should read up on the structures of GET and POST rqeusts a little before the next lab because it's becoming an issue.
  - The most difficult part was trying to format the SQL query to fit the GET request's "category" parameter. It was annoying trying to put it together until I found a cheatsheet for it on the internet and with a little help from the community solutions.

**5. Blind SQL injection with conditional responses →**
  - I found this lab to be particularly interesting. Realistically, a lot of sites would have at least some form of protection agains plain SQL injection, but I can guarantee at least a solid couple would forget to take into account the different objects and specific replies when the user inputs something. In this case the website would leave a "Welcome back" message on the site whenever the user logged in, which we were able to exploit in a blind SQL injection to confirm or deny statements. This made it simple to slowly gather the password of the administrator user through Burp Intruder by slowly iterating over each length to get the length of the administrator password (20 letters), and making advantage of the simple list attack, getting the password of the administrator by slowly iterating over each position in the string and checking if the website has the "Welcome back" message.
  - The most difficult part of this was still formatting the SQL correctly. I missed a single quotation mark and had to spend a solid 20 minutes trying to troubleshoot on where I went wrong before I noticed it. I should read up on the SQL documentation and syntax a bit more.



    
# Topic: Authentication
**1. Username enumeration via different responses →**
  - Brute-forcing is a tedious process, thought it has the possibility of getting results efficiently. Keeping the “wrong username/password” responses the same was highlighted well, since with just that error and a sample data you could easily guess a username, and the objective just became guessing the password, which is a lot easier than keeping track of two running brute-forces at once.
  - Hardest part was still trying to understand where everything was. It took me more than a while to figure out I was supposed to use the “Intruder” tab on Burp. I also added the wrong part of the syntax to be brute-forced initially, so it took me twice as long to get the correct username.  

**2. Password reset broken logic →**
  - This was probably the simplest PortSwigger lab I completed with the Burp environment. Using plaintext addresses with flawed logic is obviously very dangerous, so it wasn’t a particularly challenging task to complete, anyone could do it even without any practice. The most challenging part was that I accidentally selected the wrong request at first and looked at it for a couple of minutes before realizing.  

**3. 2FA simple bypass →**
  - The flaws in the beginner level authentication labs are always very simple, and very easy to fix in any system. It's important to keep in mind to properly implement the 2FA if you are going to add it to your application, the entire point of it is to block the login unless the authentication code is given, so the user should never be switched to the "logged in" state before this code is given. Implementing some additional checks is a crucial step to protecting user accounts.
  - The lab was not difficult enough to constitute anything as being difficult. It was simply changing the address during login, which let me into the victim's account.

**4. Username enumeration via subtly different responses →** 
  - The main point of this lab was to gain the username for the application through use of different error messages. The site would have different error messages depending on if only the password was wrong or if the email address was also wrong, which would allow us to validate the existence of the email address and then focus all out efforts on getting just the password.
  - At first I loaded the common usernames payload from the lab page to the login request in Intruder. I first tried to manually look through the messages to see which one was the correct username, but I didn't anticipate the difference between the messages being just a missing dot so it took me quite a while until I just decided to use the Grep - Extract setting to match the invalid password message to the correct response. After that it was a simple case of just attacking the password with the retrieved username and logging in after getting a positive return.
  - The lab was not particularly difficult to complete but it was a bit tedious, especially when I had to rerun the attack multiple times.

**5. Password brute-force via password change →**
  - I want to make it perfectly clear that as of writing this current one I am currently losing my mind. I tried to do two different Authentication labs with Burp Suite Community Edition in this topic, and both I have had to quit because of the limitations on the free version of Burp. At first I tried to complete the "2FA Broken Logic"-lab, but that one had a very long bruteforce section (at least 3 hours), so I was forced to drop it after spending about an hour and a half on it. Afterwards I tried to complete the "Username enumeration via response timing"-lab, but that one just refused to time the responses properly. So, after a long time of trying to make it work, I was also forced to drop that lab. All together, I just wasted about **3-4 hours** on these two.
  - The change password functionality in the lab makes it vulnerable to bruteforcing. The main point of this lab is to use that "change password"-function to change the password on carlos' account to access his "My Account"-page.
  - I started by checking the change password functionality on the provided credentials. I changed the password to something else, then entered the correct current password but unmatching new passwords. I sent this request to Burp Intruder, where I changed the username to "carlos" and then added a payload position on the current password field. I also added a grep match for the "New passwords do not match." message to catch the correct password when Burp finds it. It did not take too long to find the password "jennifer", after which I stopped the Intruder, logged out of the standard account, logged into carlos and completed the lab.
  - This one wasnt too difficult, though it wasnt exactly easy either. Mostly I was tired and cranky from the other two labs not working on the free version so I kind of rushed through it using any idea I had, which got me through the lab pretty quickly. The most annoying part was definitely the mismatching error messages with the password change, I had a bit of trouble at the start because it kept timing me out before I got the idea of what to check for.

    
  
   
# Topic: Access Control
**1. Unprotected admin functionality →**
  - Solving this lab was probably the easiest one I’ve had to do so far, because the entire task was done through just managing the URL. Obviously there needs to be some level of protection so that you cannot just simply go to a page, add /administrator-panel to the end of the URL, and then have access to delete accounts. That seems like common sense.
  - The trickiest part of this lab was trying to figure out the string of text to add to the end of the URL to give me access to the admin panel to delete users, but manually brute-forcing it took a maximum of 2 minutes, so it wasn’t that tricky. I suspect in a real scenario like this it would not be quite so simple, but it would still be ridiculously easy and low-security.

**2. User role can be modified in user profile →**
  - This one was slightly harder to complete than the previous lab, because unlike with the first one, to access the deletion functionality in the admin panel you needed to be logged in as a user with a roleid of 2, so there was at least some level of protection.
  - I tried to just intercept the POST and add the “roleid”:2 into the message, but it was blocked, so I tried to do it with Burp Repeater. I was struggling with it for a while until I realized I forgot a comma from the syntax which is why it wasn’t working. I managed to delete

**3. Unprotected admin functionality with unpredictable URL →**
  - The main task of this lab is to find the admin panel, which has been assigned to an unpredictable URL. So instead of it just being in /admin or something similar, it has been placed in a slightly less predictable URL. The task is to find this administrator panel and using it to delete the user "carlos".
  - This lab was able to be completed quite simply with just a simple look through the browser Inspect-panel. Under "Sources", scrolling down the current page's file (in any page) would reveal the following JavaScript code:
```
var isAdmin = false;
if (isAdmin) {
   var topLinksTag = document.getElementsByClassName("top-links")[0];
   var adminPanelTag = document.createElement('a');
   adminPanelTag.setAttribute('href', '/admin-pwae1g');
   adminPanelTag.innerText = 'Admin panel';
   topLinksTag.append(adminPanelTag);
   var pTag = document.createElement('p');
   pTag.innerText = '|';
   topLinksTag.appendChild(pTag);
}
```
  - The code in question seemingly creates a link in the "top-links" class to lead the administrator user into the administrator panel. However, it also provides us the administrator panel URL, as the JavaScript code is available for inspection to anyone using the website. The admin panel site also does not require you to be an administrator user to access it, meaning that we are able to delete the "carlos" user after entering `/admin-pwae1g` into the address bar.
  - The only annoying part was navigating the Inspect feature of the Browser, but otherwise everything was rather simple.

**4. User role controlled by request parameter →**
  - The main task of this lab is to access the admin panel at `/admin` and delete the user "carlos". As a regular user with the credentials `wiener:peter`, you do not have access to do anything on the administrator panel. The task is to figure out a way to gain access to the admin panel through editing a submitted value somewhere in the functionality of the site.
  - This task was not incredibly difficult to achieve. First we logged in to our given user, which in Burp Proxy returned a response with the parameter `Admin=false`. We can't edit responses, so instead I tried to go to `/admin`, except this time I could see the `Admin=` parameter in the "Cookie"-parameter field.
  - After this it was very simple, we just change the `Admin=false` to `Admin=true` using Burp Repeater, which agve us the Element of the website. In this element, we were able to see the URL calls to delete a user, specifically, `/admin/delete?username=carlos`. Trying to access this normally through the URL would return us an unauthorized error message, however with Burp Suite, we can change the `Admin=` to `true`, which allows the request to go through and delete the "carlos"-user, thus completing  the lab.
  - This lab was not particularly difficult, the most annoying part of it was to realize the uselessness of logging in with the wiener:peter account. I tried to do a couple /admin calls while logged in but received the CSRF error so I had to go log out.

**5. Referer-based access control →**
  - The main task of this lab is to access certain administrator functionality based on the Referer header of a request. To complete the lab, the user `wiener` has to be promoted to administrator through exploiting the request weakness of the Referer-header.
  - At first, I thought it was as simple as going into the administrator panel with the provided `administrator` account details and running the Request inside of Burp Repeater, but that didn't complete the lab. You must be **logged in** as wiener:peter to complete the lab. Meaning the request had to orignate from the cookie of the "wiener"-user.
  - To solve this, I logged in as wiener:peter, went to `/admin/roles?username=carlos&action=upgrade`, opened the request in Burp Repeater and just added the Referer header to the end, this time with the right cookie. I was then able to solve the lab.
  - The most annoying part about this lab was the inclusion of the correct cookie being required because it took me a couple seconds to think up of the reason why it wasn't working. (I just didn't read the instructions completely before getting started so that's on me) 

# Topic: JWT attacks
To complete tasks in this topic you will need to install the "JWT Editor"-extension into Burp Suit from the built-in BApp Store.


**1. JWT authentication bypass via unverified signature →**
  - The main task of this lab is to delete the user `carlos` by accessing the `/admin` panel on an unauthorized session by modifying the unsecure JWT token on the website. There is a provided account with the credentials `wiener:peter`.
  - I was able to understand what to do after some browsing of the JWT Topic material. After installing the JWT Editor from BApp, I attempted to decode the Base64 selected value from the GET / request that was sent after I logged in as `wiener:peter`. It was a bit annoying at first because I selected the entire token and I thought it wasn't working, but I realized pretty quickly I was supposed to just select one segment of the token up to the next `.`.
  - After getting the token and inputting it into repeater, I changed the GET request to redirect me to `/admin`, and then changed the `sub:` field in the JWT token to say "administrator" instead of "wiener", which got me access to the page. From there I just copied the path to delete the `carlos` user, which was: `/admin/delete?username=carlos`, and sent the POST request to the site with the "administrator" username still in the sub: field.
  - The lab was new and I was glad to learn new material. JWT tokens are also useful because I will be using them in another course for my web development project. The most difficult part was to edit the token in Burp because the material didn't say it couldn't decode the entire token in one go.

**2. JWT authentication bypass via flawed signature verification →**
  - JWT tokens might sometimes be left without verifying a token. Sometimes there's errors in the code where empty token signatures are not verified, and anyone without a verified signature can gain access to authorized pages.
  - The main task of this lab is to use an unsigned JWT against the website to access the admin panel at `/admin` and use it to delete the user `carlos`. The user is granted the credentials "wiener:peter"
  - At first I wasn't sure where to start. I logged in with `wiener:peter` and tried to call the `GET /admin` first with no cookie, then with no session header, then with `session=` but no JWT. Eventually I stumbled on the right answer of deleting the signature portion (the last part) of the JWT token, but it didn't quite work so simply. I changed the `sub` parameter to have the value "administrator" instead of "weiner", and then changed the `alg` parameter to `none`. This got the /admin page for me from the website and it was now loaded into my Burp Repeater.
  - After I got the /admin page into my Burp Repeater, I selected the API call that deletes the "carlos" user: `/admin/delete?username=carlos`, and changed the request's type from `GET /admin` to `POST /admin/delete?username=carlos`, while keeping the same JWT token we had from the last request (the one with no signature).
  - After sending the POST request, the user "carlos" was deleted and we completed the lab. The most irritating part of the lab was figuring out which part of the JWT to remove and which part to edit.

**3. JWT authentication bypass via weak signing key →**
  - Some JWT tokens use a weak string as the secret key, making them easy to crack through bruteforcing. Sometimes developers forget to change the placeholder JWT key value, and they end up being something on a known wordlist. Instead of running this bruteforcing on Burp Suite, we are going to be doing it on **hashcat** inside of the Kali Linux VM we have installed to test the Booking System Project.
  - The main task of this lab is to bruteforce the JWT token you get from the application against a wordlist of common keys, then after you have managed to crack the secret key of the application, use it to send a modified session token that gives you access to `/admin`, and through there delete the "carlos" user. For the purposes of completing this lab, we have been given the credentials of "wiener:peter".
  - I had to basically complete this lab in two parts. The first part is the bruteforcing of the JWT token, the second part is the generation of a valid session token.
  - For the first part, the first thing I did was open up Kali Linux VM and create a working environment folder. I utilized the instructions on the PortSwigger website to get going, and copied the wordlist given in the task description into a .txt file in the working environment. I then copied the session token from the website request after logging in as "wiener:peter", and set it as the thing to be cracked in hashcat. The specific command I used was: `hashcat -a 0 -m 16500 eyJraWQiOiJjYTcyNThhYy03MTQ1LTRkZGEtYjBkYi04ZWMwYWEzZjc4ODQiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTc0NDEwNzc2NCwic3ViIjoid2llbmVyIn0.wI0-Y9DN9mfl_alzEewNlJS2Tgpa0R4HLuaOA4NimJY wordlist.txt`. (The common keys wordlist from the PortSwigger lab assignment page is inside of wordlist.txt in the working environment)
  - Hashcat ran the command very quickly (within 10 seconds), and returned the JWT secret key `secret1`. With this secret decoded, we copied it into the Burp Decoder and encoded it in the Base64 format into `c2VjcmV0MQ==`. We then took this new encoded key and created a new symmetric key with it in the JWT Editor tab. We now return to the Burp Repeated, and begin editing the JWT token. First we change the `sub:` field from "wiener" to "administrator". Then we select the "JSON Web Token" tab in the request, and at the bottom we click "Sign", and "OK" in the pop-up. This will have generated a correct signature based on our earlier generated encoded secret key, and we should now be able to send the `GET /admin` request and receive back the website.
  - After receiving the `GET /admin` website, we simply take the `/admin/delete?username=carlos` user deletion API call in the panel, paste it into the call so it goes to `POST /admin/delete?username=carlos` instead of `GET /admin`, and send it forward. The "carlos"-user deleted successfully and we have completed the lab.
  - The most difficult part of this lab was to decode the secret key. I had to use a hashcat command, which I had to look up help for on how to do. I admit to using the command in the solution on PortSwigger after not finding much help in online materials and struggling with the command for more than 20 minutes. After that though, the rest of the lab was moderately challenging. I got through the rest quite simply without too much trouble, though I did struggle with finding all of the correct tabs and encoding `secret1` correctly. The given wordlist was also incredibly large, so I was expecting it to take much longer to go through than 10 seconds. 
