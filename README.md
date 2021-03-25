
# Password Hashing
This project is intended to demonstrate basic password cracking with Hascat and it serves as notes for myself. 

Make sure to have Hashcat downloaded.

Hashes.txt contains the target hash and rockyou-75.txt contains the wordlist (rockyou-75.txt is the abridged version of rockyou.txt, which is an old list of common passwords). Hascat will attempt these passwords when cracking. Hashcat will also need the following two arguments:

>    -m <hash-mode> - the type of hash to crack
>    -a <attack-mode> - the type of attack to run

More options information can be found typing `hashcat --help`, 

## Problem 1:

Crack this MD5 hash, 03E4079B565AB2A47A2EFF7F42AE45B8.

For this problem we will use a hash-mode 0 (MD5) and attack-mode 0 (Straight):

`$ hashcat -m 0 -a 0 hashes.txt rockyou-75.txt`

We may need to add a final flag, --force, to hashcat commands when using Linux or a VM, or if Hashcat complains about required hardware not being present. For the example above, you'd use `hashcat -m 0 -a 0 hashes.txt rockyou-75.txt --force` 


Hashcat should output:
03e4079b565ab2a47a2eff7f42ae45b8:iloveyou!  -> hash:password


## Problem 2:

Crack this SHA1 hash, dc6f0dbebfc5747330deeedfbd8475568a740d0a. The following salt value was added 
*before* the hash, 80808080.

For this problem we will use a hash-mode 120 (sha1($salt.$pass)) and attack-mode 0 (Straight):

`$ hashcat -m 120 -a 0 hashes.txt rockyou-75.txt`

Hashcat should output:

dc6f0dbebfc5747330deeedfbd8475568a740d0a:80808080:pandemonium  -> (hash:salt:password)

## Problem 3:

Crack this SHA-512 hash, ff8d646ac52b7794adaddaad606042ff6d2d71c5b91cbf1c11d411c790419cf1651ebe71551cd1973abac9d32d1392122cc676f4aa8494e7da6325a1050fd2da. The following salt value was added *after* the hash, 31415926535897932384626433832795028841.

For this problem we will use a hash-mode 1720 (sha512($salt.$pass)) and attack-mode 0 (Straight):

`$ hashcat -m 1720 -a 0 hashes.txt rockyou-75.txt`

Hashcat should output:

ff8d646ac52b7794adaddaad606042ff6d2d71c5b91cbf1c11d411c790419cf1651ebe71551cd1973abac9d32d1392122cc676f4aa8494e7da6325a1050fd2da:31415926535897932384626433832795028841:oleander -> (hash:salt:password)

## Problem 4:

Crack this unsalted SHA1 hash, 43fb93b0762621345d15386bc9f2e08396a16ef5.

The password for this has is not in the rockyou.txt wordlist. We will crack this password using the given 2 hints.


  >  This user has used his phone number as a password on other sites
  >  This user lives in California

We will assume that the password is the users 10 digit phone number and use the following mask command ?d, which will specify the type of characters to use. Only digits in this case.

> * Built-in charsets:
>   ?d = 0123456789

To specify the character set for all ten digits of the phone number we do ?d?d?d?d?d?d?d?d?d?d

For this problem we will use a hash-mode 100 (SHA1) and attack-mode 3 (Brute Force): 

`hashcat -m 100 -a 3 hashes.txt "?d?d?d?d?d?d?d?d?d?d"` 

Hashcat should output:

43fb93b0762621345d15386bc9f2e08396a16ef5:4158675309 -> (hash:password)

---------------------------------------------------------------------------------------------------------------------------------------



Hash types and more information can be found here https://hashcat.net/wiki/doku.php?id=example_hashes




