---
layout: post
title: Storing user passwords safely in the database
date: '2014-01-30T19:42:00+02:00'
tags:
- security
tumblr_url: https://blog.mist.io/post/75060934180/storing-user-passwords-safely-in-the-database
---
Data security and privacy have been in the spotlight in 2013 after the Snowden revelations. But for services like Mist.io where sensitive data are involved, protecting access to these data was always essential. Enforcing security is no simple task and it is much more than just securing your database and server. It is about coding practices, company policies and constant vigilance and education. Security is a mindset that needs to run through every stage of development, not a patch to be added at some milestone.

Today, we are starting a series of security-related posts through our blog, in an attempt to not only build trust and communicate our security policy to our users, but also to stimulate discussion and practices around common security issues. In our first post, we’ll deal with probably the most common way that attackers gain access to sensitive information: password recovery.

A few months ago, in what appears to be the largest publicly known password database compromise in history, hackers [gained access](https://shouldichangemypassword.com/adobebreach.php) to more than 150 million Adobe customer email addresses and passwords. Adobe claimed that all stored passwords were encrypted, but researchers [quickly discovered](http://nakedsecurity.sophos.com/2013/11/04/anatomy-of-a-password-disaster-adobes-giant-sized-cryptographic-blunder/)&nbsp;that the company failed to introduce modern encryption and hashing techniques while storing password hints in plaintext, making it much easier to recover a large portion of these passwords.

![image](http://imgs.xkcd.com/comics/encryptic.png)  
_Source: [xkcd](http://xkcd.com/1286/)_

### The wrong way to store passwords

**Plaintext** : This was a popular way to store passwords in the last millenium. The problem is, if attackers gain access to your database, they instantly get all the users e-mails and passwords. And since most people reuse the same password for different services, they get access to user emails or worst.

**Obfuscated passwords** : So, if cleartext passwords are a bad idea, lets just obfuscate them so they aren’t readable. E.g. stored\_value = base64(password). Better? No, not really. The passwords can not be instantly read any more, but it is very easy to reverse the process and recover the passwords. Obscurity != Security.

**Encrypted passwords** : Obfuscating passwords is not enough, so why not encrypt them then? E.g. stored\_value = aes(key, password). The problem is, encryption is symmetrical. The server needs access to the key in order to encrypt passwords during login and an attacker who gains access to the server, gains access to the key too. Thus the passwords can easily be decrypted.

**One way crypto-hashes** : One way hashes can only be easily computed in one direction. This is still a very popular method but it can be partially circumvented. Apart from the fact that some algorithms are weak (md5, I’m looking at you), two identical hashes mean two identical passwords. That means that a [rainbow table](http://en.wikipedia.org/wiki/Rainbow_table) attack can quickly recover a significant amount of common or weaker passwords.

**Adding a bit of salt** : A salt, is a random bytestring that gets added to the mix when calculating the hash. E.g. stored\_value = salt + crypto\_hash(salt+CTP). That means that two identical passwords, will not result in the same stored value, thus making a rainbow table attack impractical. This could have been enough a few years ago, but not any more. Most gamers today own GPUs that can compute more than 1 billion SHA256 hashes per second. A malicious attacker could use hundreds of them to decrypt your passwords.

### The winner: Use some salt and take your time.

Hashes like SHA256 or SHA512 are optimized for speed. But faster computing time in this instance works for the attackers, not us. Users can tolerate some extra 200ms when logging in but for an attacker that has to calculate billions of combinations it makes a huge difference. That is why algorithms like pbkdf2, bcrypt or SHA512\_crypt are designed to be slower to compute.

In Mist.io, we use PBKDF2 to hash the password and store it to the database. PBKDF2 applies a pseudorandom function, such as a cryptographic hash, cipher, or HMAC to the input password along with a salt value and repeats the process many times to produce a derived key. We apply 20.000 rounds of pbkdf2\_sha512 hashing before storing the key to our database, making it much harder and time-consuming for an attacker to recover a password. Moreover, by using different salt values for every user, the resulting hash is always different even when the initial passwords were the same, which prevents password recovery by an attacker using tables of precomputed hash values.

If your application is written in python like ours, we strongly recommend using the excellent [passlib library](http://pythonhosted.org/passlib/)&nbsp;for your password hashing needs. [Here](http://pythonhosted.org/passlib/new_app_quickstart.html)&nbsp;is a quickstart guide to get you started.

Safely storing passwords in the database is only the first step in protecting user passwords. There are other security practices like limiting login attempt rate and enforcing strong passwords that can protect user accounts from on-site attempts to gain access. We will review some best practices and how we enforce them in Mist.io in one of our next blog posts. Stay tuned!

_\*There was an excellent presentation last summer in EuroPython by [Thomas J. Waldmann](https://twitter.com/ThomasJWaldmann "Thomas J Waldmann at Twitter") regarding password storage. You can download the slideshow [here](https://ep2013.europython.eu/conference/talks/passwords-the-server-side)_

**Do you have machines across clouds and want to easily manage them?&nbsp;[Sign up for Mist.io and try it for free!](https://mist.io)**

