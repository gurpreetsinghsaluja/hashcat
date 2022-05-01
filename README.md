# Hashcat Presentation Notes
-----------------------------------
## For ECE 9069: Introduction to Hacking (Cybersecurity) course, during master's degree in Software Engg. at Western University, ON, Canada
___________________________________

## Hashcat Tool
* Hashcat is a Password recovery(a.k.a Password cracking) tool. 
* It is open-source and multi-platform(Windows, Linux, MacOS).
* It supports multiple hashing algorithms such as MD5, SHA-1,SHA-2, NTLM etc. Over 350+ hash types.
* It claims to be the world's fastest password recovery tool..
* It also supports use of GPU hardware acceleration for faster password cracking.

### Useful Links:
[Hashcat Download](https://hashcat.net/hashcat/)

[Tutorial on running Hashcat on Windows 10](https://www.youtube.com/watch?v=rRUiRz_znLQ)

[Hashcat Demo](https://www.youtube.com/watch?v=EfqJCKWtGiU&t=888s)

[Hashcat on Kali Linux tutorial notes](https://seguranca-informatica.pt/hashcat-tutorial-for-beginners/)
____
## Password Hashing

> Password Hashing is the process of converting a password of any length and applying a mathematical algorithm to generate a fixed length string called Hash value.
![Password Hashing](https://images.ctfassets.net/23aumh6u8s0i/ES2U6Gx7w0yVF9Asidr26/75531c3695f09272142b543a94acc0de/hash-flow)

>It is a Standard Industry practice to save passwords in Hashes and not as plaintext. However, it's not suprising not all follow the standards like the company [RockYou](https://en.wikipedia.org/wiki/RockYou) which stored passwords as plaintexts. RockYou was hacked and the passwords were stolen which are now being used as wordlist when password cracking.
___

## Access to Hash Database
* White hat hackers would have access provided by the company itself.
* Systems gets hacked and hash data along with other data are stolen and then leaked or sold online in the dark web.
* Hackers then use this hash dump to crack passwords and gain access to systems. 

### News Links:
* [Nvidia Hack where Password Hashes were stolen.](https://www.notebookcheck.net/Ruthless-hackers-have-leaked-71-355-email-addresses-and-password-hashes-of-Nvidia-employees.606681.0.html
)

* [Big Basket Data Breach news.](https://www.moneycontrol.com/news/technology/details-of-20-million-bigbasket-users-leaked-online-after-data-breach-last-year-6817061.html
)
---
## Users of Hashcat
![Users of Hashcat dichotomy](https://www.davidenker.com/wordpress/wp-content/uploads/2019/11/Donald-Devil-Angel.jpg)

| For Good | For Bad    |
| -------- | -------------- |
| Whitehat Hackers  | Blackhat hackers |
| To check for weak passwords being used in the system. | To gain access to systems by cracking their password for fun, money or cyber warfare. |
___
## Hashcat Syntax
>Usage: hashcat [options]... hash|hashfile|hccapxfile [dictionary|mask|directory]...

### Hashcat uses minimum of 4 arguments.
1. –m (Hash type) Ex “–m 0” means md5 , “-m 1000” means Windows NT Hash.
1. -a( Attack type) Ex “-a 0” means dictionary attack.
1. Hash value or file containing hash values that are to be cracked.
1. Specify a dictionary(wordlist), mask or directory to be used. Ex:- rockyou.txt wordlist
### Useful Link:
[Hashcat Demo with Argument explanation](https://www.youtube.com/watch?v=EfqJCKWtGiU&t=888s)

## Attack Types in Hashcat

The 5 most popular type of attacks in hashcat are:
### 1. Dictionary attack – Mode 0
> This is a straight forward attack in which each password in text file is tried as the password candidate. 
> Due to its simplicity, it is also called as “straight” mode or “Wordlist attack”.
### 2. Combinator attack – Mode 1
> Here, we concatenate words from multiple wordlist to create password candidates.
> For example, if we have files dictionary 1 and dictionary 2 as follows:
> Input\
\
Dictionary 1:\
yellow\
green\
black\
blue\
\
> Dictionary 2:\
Car\
bike\
\
> The possilbe password candidates after concatentation in combinator attack are as shown below:\
yellowcar\
greencar\
blackcar\
bluecar\
yellowbike\
greenbike\
blackbike\
bluebike\
\
> Now, all the above are converted into hash using the chosen algorithm and then compared with the target hash.
\
>You need to specify exactly 2 dictionaries in you command line: e.g. ./hashcat64.bin -m 0 -a 1 hash.txt dict1.txt dict2.txt

### 3. Brute-Force Attack – Mode 2
>* Here, all the combination of the keyspace is tried. It is the easiest of all the attacks as every combination of the keyspace are used for generating a password candidate.\
>* Charset and password length range is specified.\
>* Total combinations is c^p. This is due to the number of characters being c and the password length range being p. So c gets multiplies by p as p places of characters need to be filled.\
It is the relatively most old, outdated and time-consuming method.

### 4. Hybrid Attack – Mode 6
> It is just like a combinator attack.
It is a combination of dictionary and brute force attack. For example, the syntax can be:
$ ... -a 6 example.dict ?d?d?d?d
\
>example.dict file contains:\
Password\
hello\
\
generates the following password candidates:\
password0000\
 password0001 \
 password0002\
.\
.\
.\
Password9999\
Hello0000\
Hello0001\
hello0002\
.\
.\
.\
hello9999\

### 5. Mask Attack – Mode 7
>It tries specific combinations from a given Keyspace.In this method the number of password candiates will be very less compared to that in Brute force method and thus saving time by cracking the password faster by using only possible password candiates.
>\
Eg: We want to crack the password: Singh2022\
Here we are taking a password pattern in which, as according to general human tendency, first letter is captialised and the last 4 digits are numbers.\
If we use brute force attack, it will lead to the following time to crack the password
62^9 @ 100M/s (62^9)/((10^8)*60*60*24*365) = 4years\
Otherwise, when we use the mask attack, computation time would be: 52*26*26*26*26*10*10*10*10 combinations -> 40minutes

### Useful Links:
[Hashcat Attack Types with examples](https://programmerclick.com/article/1714841109/)

[Hashcat Tutorial for different Attack Types ](https://ethicalhackingguru.com/the-complete-hashcat-tutorial/)

___
## Demo
* Download and install Hashcat tool. It comes pre-installed in Kali Linux.
* In case you want to try thr hascat tool, Create Hashes to be cracked and save them in a text file.You can also use hashes from a hash dump/database if you have access to it.
* Download rockyou.txt worldlist file from the [link.](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt) It's available in Kali Linux out of the box. Use a search command to find it's location in the system.
* The command to be used when using hashcat in Windows look like as show below.
> `hashcat.exe -m 0 -a 0 -o <output file where cracked password will be saved> <Hash/file with list of hash values to be cracked> <wordlist file>`

#### Useful Link:
[Online md5 hash generator](https://www.md5hashgenerator.com/)
___
## References
Note: References posted at different sections of this notes is also listed below.

1. https://hashcat.net/wiki/doku.php?id=hashcat
1. https://auth0.com/blog/hashing-passwords-one-way-road-to-security/
1. https://www.notebookcheck.net/Ruthless-hackers-have-leaked-71-355-email-addresses-and-password-hashes-of-Nvidia-employees.606681.0.html
1. https://hashcat.net/wiki/
1. https://www.moneycontrol.com/news/technology/details-of-20-million-bigbasket-users-leaked-online-after-data-breach-last-year-6817061.html
1. https://www.davidenker.com/wordpress/wp-content/uploads/2019/11/Donald-Devil-Angel.jpg
1. https://www.youtube.com/watch?v=EfqJCKWtGiU&t=888s

