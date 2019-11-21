---
id: index
title: DDD
---
<!----- Conversion time: 1.197 seconds.


Using this Markdown file:

1. Cut and paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β14
* Sun Feb 10 2019 09:39:47 GMT-0800 (PST)
* Source doc: https://docs.google.com/open?id=1VZHbn1Aq9iA_3q6U5eW4lhmzPldlbxUNBvtyyNQkV6E
* This document has images: check for >>>>>  gd2md-html alert:  inline image link in generated source and store images to your server.
----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 0; ALERTS: 3.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p><a href="#gdcalert1">alert1</a>
<a href="#gdcalert2">alert2</a>
<a href="#gdcalert3">alert3</a>

<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>


[http://www.sitepoint.com/speeding-up-existing-apps-with-a-redis-cache/](http://www.sitepoint.com/speeding-up-existing-apps-with-a-redis-cache/)

**Introduction to Redis**


###### 
    **TABLE OF CONTENTS**



1.  WHAT IS REDIS?
    1.  WHAT IS REDIS USED FOR?
    1.  WHICH COMPANIES USE REDIS?
1.  HOW TO INSTALL REDIS
1.  REDIS DATA TYPES
1.  INTERACTING WITH THE REDIS SERVER
1.  PLAYING WITH DATA
1.  MODELING DATA
1.  USING REDIS WITH YOUR PHP APPLICATION
1.  CONCLUSION
1.  LEARN MORE / RESOURCES


##### **WHAT IS REDIS?**

**Redis is a key-value database which is often grouped under the NoSQL category of databases. It was initially released by the developer Salvatore Sanfilippo on the 10th of April 2009.**

**Redis is an in-memory database which means that it's data is stored on its host's RAM memory. This makes Redis blazingly fast because it does not have to wait for traditional hard drive seek and read times.**

**Being a NoSQL database, Redis allows you to create schema-less data structures that fit your applications needs. You can use Redis for simple things, like creating a visitor counter, or for more detailed information, such as storing user information.**


###### **WHAT IS REDIS USED FOR?**

**Redis shines when you require something that's fast, really fast. Redis can be utilized for a range of different use cases, here are just a few:**



*   **Caching**
*   **Queuing**
*   **Pub/sub**
*   **Machine learning**


###### **WHICH COMPANIES USE REDIS?**

**Redis is used by a range of companies but most notably: Snapchat, Twitter, StackOverflow, GitHub and Pinterest. There are some really great technical presentations and blog posts where you can learn how these and other companies are using Redis:**



*   **[How Twitter Uses Redis To Scale - 105TB RAM, 39MM QPS, 10,000+ Instances](http://highscalability.com/blog/2014/9/8/how-twitter-uses-redis-to-scale-105tb-ram-39mm-qps-10000-ins.html)**
*   **[Using Redis at Pinterest for Billions of Relationships](https://blog.pivotal.io/pivotal/case-studies/using-redis-at-pinterest-for-billions-of-relationships)**
*   **[How does CodePen use Redis? Critical and simple use cases, backups, and config tweaks](https://scaleyourcode.com/blog/article/25)**


##### **HOW TO INSTALL REDIS**

**We've spoken about the benefits of Redis and some of its use cases, so let's get it installed and start interacting with it. We will be installing the Redis server and Redis command line tool on Ubuntu 14.04 . Let's add a new repository:**


```
sudo add-apt-repository ppa:chris-lea/redis-server
```


**Now let's update our system:**


```
sudo apt-get update
```


**We now need to install the Redis server:**


```
sudo apt-get install -y redis-server
```


**Redis should now be installed. We can confirm this by accessing the Redis command line tool by running the following command:**


```
redis-cli
```


**Here's an example:**



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Radis0.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Radis0.png "image_tooltip")


**You can now exit the command line tool by running the 'quit' command. Redis uses port 6379 by default.**


##### **REDIS DATA TYPES**

**Similar to other databases, Redis supports several data types:**



*   **Strings**
*   
*   **Lists**
*   
*   **Sets**
*   
*   **Hashes**
*   
*   **Sorted Sets**
*   **Bitmaps**

**Choosing the correct data type for your data and requirements is key to getting the most out of Redis for performance and scalability. Let's learn about each data type:**



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Radis1.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Radis1.png "image_tooltip")



##### **INTERACTING WITH THE REDIS SERVER**

**Now that we have the Redis server installed, we can start interacting with it. To do so, we will be using the Redis command line tool that we tested earlier. To get started, run the following command:**


```
redis-cli
```


**Run the 'help' command:**


```
Type: "help @<group>" to get a list of commands in <group>
      "help <command>" for help on <command>
      "help <tab>" to get a list of possible help topics
      "quit" to exit
```


**We can use the 'help' command to see how to use commands. For example,  "help HGET" returns the following:**


```
 HGET key field
  summary: Get the value of a hash field
  since: 2.0.0
  group: hash
```


**We can see that this command is part of the "hash" group, if we wish to learn more information about that group we can run the following command:**


```
help @hash
```


**This will then output all of the commands available in that group, such as HGET, HGETALL and HKEYS. If you are not sure where to start with the help command then type 'help ' (please note the space) and then press the tab key. Redis will then suggest a topic, you can press enter to learn more about that topic if you are interested or keep pressing tab to cycle through topics.**


##### **PLAYING WITH DATA**

**Let's start adding some data into Redis and modify that data. We'll create a basic key name "firstkey" by running the following command:**


```
SET firstkey "1"
```


**We've set the value of 'firstkey' to 1. We can confirm this by running the 'GET' command:**


```
GET firstkey
```


**The value returned will be "1." Great, you have now set and retrieved your first key in Redis. To continue from here, we can update our key's value by incrementing it. By running the "INCR" command, our key will be incremented by one and the new value will be returned:**


```
127.0.0.1:6379> INCR firstkey
(integer) 2
```


**You can also decrement your key by using the "DECR" command. This will decrement the value by one and return the latest value of the key.**


##### **MODELING DATA**

**Redis can be used for much more than a simple counter. Let's look at how we could model a traditional data structure such as a user table. If we were using MySQL, then a row in our simple users table might look something like this:**



<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/Radis2.png). Store image on your image server and adjust path/filename if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/Radis2.png "image_tooltip")


**Please note: the password in this example has been hashed. This means that the password is not stored in the database as plain text. Normally a hashed password would be a lot longer compared to the one I use in this example. For example, if you used a sha256 function to hash the password then the length would be approximately 64 characters.**

**We store the password as a hash in case our database is ever stolen or possibly accessed maliciously. To check if a user has entered the correct password then you must hash their input and check the database for that hashed value. To increase security many people add a "salt" to their hashing process. A salt is used to help randomize the hashes (so two users with the same password would have a different hash) and make it even harder for hackers to crack a hashed password. If you are interested then you can learn more here - [http://www.codeproject.com/Articles/704865/Salted-Password-Hashing-Doing-it-Right](http://www.codeproject.com/Articles/704865/Salted-Password-Hashing-Doing-it-Right) and [https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet).**

**To achieve this in Redis we can utilize the hash data type. We will be using the HMSET command to add in our use data. Here is the structure of the HMSET command:**

**HMSET [hash name] [key1] [value1] [key2] [value2]**

**Let's see the command in action:**


```
HMSET user:1 id 1 username alexbraunton password a67b9b40f76 email alex@example.com verified Y
```


**If Redis is happy with the formatting and input then it will return an "OK". We can check that the hash has been added to the server by running the "KEYS *" command:**


```
127.0.0.1:6379> KEYS *
1) "firstkey"
2) "user:1"
```


**Great! The value has been saved into Redis. Let's check the hash and see our values:**


```
127.0.0.1:6379> HGETALL user:1
 1) "id"
 2) "1"
 3) "username"
 4) "alexbraunton"
 5) "password"
 6) "a67b9b40f76"
 7) "email"
 8) "alex@example.com"
 9) "verified"
10) "Y"
```


**If you just need to see the available keys (and not the values) of a hash then you can run the "HKEYS" command:**


```
127.0.0.1:6379> HKEYS user:1
1) "password"
2) "email"
3) "username"
4) "id"
5) "verified"
```


**To get a specific value, we can access it by its key using the "HGET" command:**


```
127.0.0.1:6379> HGET user:1 email
"alex@example.com"
```


**The user wants to update their email address, so let's update our user's information. We can do this by using the "HMSET" command:**


```
127.0.0.1:6379> HMSET user:1 email "alex_new@example.com"
OK
127.0.0.1:6379> HGET user:1 email
"alex_new@example.com"
```


**You can see that Redis returned "OK" and when we check the key's value, we see that it has been updated. Let's say one of our users needs to be deleted from Redis. We can do this by running the "DEL" command:**


```
127.0.0.1:6379> DEL user:1
(integer) 1
127.0.0.1:6379> KEYS *
1) "firstkey"
```


**We passed the hash into the DEL command as an argument and Redis returned an integer of 1 to let us know the operation had been completed. If we tried to run the "DEL user:1" command again then Redis would return an integer of 0. The second command we ran was the "KEYS *" one to confirm that it has been deleted.**


##### **USING REDIS WITH YOUR PHP APPLICATION**

**Now that you have the Redis server installed and you are familiar with the command line tool, let's look at integrating Redis with our PHP web application.**

**For a PHP code base, I would recommend using the Predis library as it is very fast and incredibly easy to get started with but also is very configurable for when need it.**

**You can find lots of great documentation such as installation instructions and how to get started on the official Predis GitHub page - http://github.com/nrk/predis**


##### **CONCLUSION**

**Here's what we learned:**



1.  **We've learned how to install Redis on Ubuntu 14.04.**
1.  
1.  **We've learned what Redis is and what it's useful for.**
1.  
1.  **We have been introduced to the Redis command line tool and learned some of the Redis commands.**
1.  
1.  **We have learned how to interact with Redis using our PHP web application using the Predis package.**


##### **LEARN MORE / RESOURCES**

**If you have enjoyed this guide and would like to learn more about Redis then here are some links to help get you started:**



*   **Official Redis website - [http://redis.io/](http://redis.io/)**
*   
*   **List of Redis commands - [http://redis.io/commands](http://redis.io/commands)**
*   
*   **Predis code library - [https://github.com/nrk/predis](https://github.com/nrk/predis)**

<!-- Docs to Markdown version 1.0β14 -->
