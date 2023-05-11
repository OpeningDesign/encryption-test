This repo is a test for encryption. It contains one clear file (this one) and a couple of encrypted ones. It uses [git-crypt](https://www.agwa.name/projects/git-crypt/)

Make sure you read he [git-crypt readme](https://github.com/AGWA/git-crypt) before working with encryption, as there are several details important to know (ie. some files won't be encrypted unless you do it properly)

## Working with git crypt

### Linux

If you are creating the repo:

- Install git-crypt from your distro repo
- Clone or init a git repo
- `git crypt init`
- Create a .gitattribute file where you specify what must be encrypted (see below)
- Either add your pgp user `git crypt add-gpg-user your_pgp_email@something.com` or export a symmetric key `git crypt export-key /path/to/keyfile`
- Add other pgp users if needed (you must have their public key and they need trust level 5 - see below)
- Push

If you are cloning an existing encrypted repo:

- If you are already added as a pgp user: `git crypt unlock`
- If you have the symmetric key: `git crypt unlock /path/to/keyfile`

Note that when installing git-crypt, you get a program named git-crypt, and also the command "crypt" is added to git. So you can either use "git crypt" (the git command) or "git-crypt" (the git-crypt program)

### Windows

#### If you are creating the repo:

- download [git-crypt.exe](https://github.com/LykkeCity/git-crypt/releases) and locate the file in the repo folder. Placing the git-crypt.exe program in the repo folder is not absolutely necessary, but it's convenient, because then when you launch a terminal directly inside the repo folder, git-crypt will be found immediately without the need to type its full path
- Open a command window at the repo folder
  - One method: Navigate to the project folder, hold down the Shift key and right-click the folder. The context menu will contain an entry, ‘Open PowerShell window here'.
  - Other method: Start menu, type "cmd". You will get a command prompt showing "C:>". Navigate to your repo folder using the **cd** (change directory) command, ex: `cd Users\myUser\myProject`. Use `cd ..` to move back one directory up
- `./git-crypt init`
- Create a `.gitattributes` file (This file is where you give instructions on which files/folders will be encrypted)
  - One method:  Download and locate [this file](https://raw.githubusercontent.com/OpeningDesign/New_2nd_Story/master/_CLOSED_New_2nd_Story/.gitattributes) in the folder you would like to be encrypted.  All subfolders will be encrypted as well.
  - **Warning** any file committed before it has been added to .gitattributes will not be encrypted
- Adding users or creating a 'collaboration' key.
  - Either...
    - add your pgp user `./git-crypt add-gpg-user your_pgp_email@something.com` or
      - You might have to get the user's public key and add them to the gpg keyring first
        - Adding key
          - Obtain the public key of the person you'd like to collaborate with.  It will be a .txt, .gpg or .asc file. *(they are all the same common text file, just with different extensions).*
          - `gpg --import C:/path/to/filename`
        - Set trust level
          - `gpg --edit-key your_pgp_email@something.com`
          - `gpg> trust`
            - 1 = I don't know or won't say
              - 2 = I do NOT trust
              - 3 = I trust marginally
              - 4 = I trust fully
              - 5 = I trust ultimately
              - m = back to the main menu
          - Your decision? `5`
          - Do you really want to set this key to ultimate trust? (y/N) `y`
          - gpg> `q`
          - Push to remote repo
    - export a symmetric key `./git-crypt export-key C:/path/to/filename.gpg`
      - Share this key with fellow collaborators
      - Save in a safe location
    - Push to remote repo

#### To unlock with a personal key

Assumes you have your pgp key correctly configured on your local machine (see below) and your public key has been added to the repo by a follow collaborator.

- Windows: `./git-crypt unlock`
  - **Warning:** git-grypt might not work if you have any special character in your local repo path.
    
  - If Powershell
    
    - Navigate to the top level of the project folder (the level that has `git-crypt.exe` inside) hold down the Shift key and right-click the project folder. The context menu will contain an entry, ‘Open PowerShell window here'.
      - then `./git-crypt unlock`
      
        
  - If GitBash
    
    - Navigate to the top level of the project folder (the level that has `git-crypt.exe` inside) right-click the project folder. The context menu will contain an entry, ‘Git Bash Here'.
      
      - then`./git-crypt unlock`
        
        - if *Error: no GPG secret key available to unlock this repository.*
          
          - then `gpg --import <path to private key file>`
            
            - example ```gpg --import C:/keys/secret_key.asc``` (make sure there's no spaces in the path)
            
            - then `./git-crypt unlock`
            
            - if the error persists, check if there are any special characters in your repo path. Try removing the special characters or cloning the repo in another path.

          
          - or `./git-crypt unlock path/to/secretkey.asc`
            
            - if *Error: path\secretkey.asc: not a valid git-crypt key file.*

            - then check if there are any special characters in your repo path. Try removing the special characters or cloning the repo in another path.


          
            

- Linux: `git crypt unlock`



#### To unlock a repo with a symmtric (or 'collaboration') key:

- Your collaborator has given you a {filename}.gpg file.  Store somewhere safe.  Remember the file's path, for steps below.
- Open a command window at the repo folder (top most level -- that is no subdirectiories)
  - One method: Navigate to the project folder, hold down the Shift key and right-click the folder. The context menu will contain an entry, ‘Open PowerShell window here'.
- `./git-crypt.exe unlock C:/path/to/filename.gpg`
- All encrypted file/folders should be accessible now.

#### To see a list of Collaborators already associated with the repo

- `git log .git-crypt/`

#### To see which files are encrypted in the repo

- `./git-crypt status`

## Working with PGP keys

A PGP key is made of two keys: One private/secret, that you should NEVER give to anyone, and one public, that you can give to as many people as you want or even make publicly available (on your website, in your email signature, etc). 

Each key of the pair can decrypt what the other encrypted. So other people who have your public key can read encrypted files/text that you and only you issued (and therefore certify that what they are reading can only have been made by you, this is also called signing), and they can also send you text/files that only you can read, by encrypting them with your public key (so only your private key can decrypt it)

Once you generated a key pair, both can be exported to .asc files, and these files can be imported on other machines (cellphones, other computers) to be able to encrypt/decript with the same keys. It is also important to keep a backup of those two files otherwise if you loose them what has been encrypted with any of them is lost forever.

You can make as many keys as you want for yourself, but you can also make only one and use on any other computer or operating system or application where you need pgp keys. Just be careful where you place your private key, any application or server that can access it will be able to read secret messages sent to you and send messages as if they were signed by you.

Remeber that like any encryption system, although pgp encryption is unbreakable right now, one day, nobody can tell when,  it will become breakable and the content you encrypt today will become readable.

**Note** pgp stands for 'pretty good privacy' which is the official protocol, gpg stands for 'GNU pgp' (they love those brain twisters at GNU) which is GNU's (open-source) version of the pgp protocol. It's confusing but it usually refers to the same thing. To resume, gpg is an open-source program that produces and manages pgp keys.

### Linux

#### Creating a key for yourself

* Install gpg from your distro repo
* Open a terminal
* `gpg --full-generate-key` and accept all the defaults: RSA/RSA,3072 bits, 0 - never expire
* Give your name, your email (a pgp key is bound to an email), and optionally a comment
* `gpg --list-keys` lists all the keys installed for your user. Check that your new key is there
* If you need to give your public key to other people, export your public key as an .asc file: `gpg --armor --export you@server.com > /home/youruser/my_public_key.asc`
* It is a good idea to backup both your public and private asc files. Generate another one for your private key with `gpg --armor --export-secret-keys yourname@gmail.com > /home/youruser/my_private_key.asc`

Note that .asc is a simple text file, you can also export as .txt or .pgp

#### Adding PGP keys of other people

`gpg --import /path/to/some_key.asc`

If you are going to use that key to add the person to git-crypt, you need to raise their trust level first (see above)

### Windows - Creating New GPG Keys with Kleopatra

* Install gpg for windows from https://www.gpg4win.org/. This will install gpg and a keys management application called Kleopatra
  * Run as an adminstrator
    * ![](imgs/admin_gpg4win.png)
* After install, run Kleopatra
* Create a new pair of keys
  * File --> New Key Pair...
  * Choose **Create a personal OpenPGP key pair**
  * Enter Name and Email (use email address assocaited with Gitlab and Github accounts)
  * Review Parameters, and if okay, click **Create**.
  * Create Passphase and write it down somewhere so you don't forget it. 
  * **Make a Backup Of Your Key Pair...**
    * Save this .asc file elsewhere.  This is your private key.  Don't share with anyone.
  * To share your public key with others, right click on your name/email in the list of key and go to **Export..** You can share this file with your collaborators.  If in doubt, open this file in a text editor, and it should say something like **-----BEGIN PGP PUBLIC KEY BLOCK-----**
    * ![](imgs/export_keys.png)
  * You can also **Export Secret Keys**... but again don't share this with anyone.  If in doubt open the file in a text editors, and it should say something like **-----BEGIN PGP PRIVATE KEY BLOCK-----**

#### More notes on working with pgp keys and git-crypt on Windows

When you install Git for Windows and use the "git bash here" right-click option, you get an actual Linux terminal (bash is a linux terminal interpreter, ported to windows). So you can use most of the linux commands listed on this page. For example, `gpg --list-keys` useful to check that your pgp key has correctly been added by Kleopatra and will be found by git-crypt. 

If it isn't there, you can add it manually with `gpg --import /path/to/some_key.asc`, using the path to your own key saved by Kleopatra. Note that in the bash terminal, directory separators must be linux-style (/) instead of windows-style (\)

### Adding more emails to an existing pgp key

pgp keys can be bound to more than one email.You can add more emails to your existing key: https://help.github.com/en/articles/associating-an-email-with-your-gpg-key:

- `gpg --list-secret-keys --keyid-format LONG` (check your key ID)
- `gpg --edit-key 3AA5C34371567BD2` (you can also use your email address insteadof the key ID)
- `gpg> adduid`
- Fill in name, email, comments, Press Okay
- Type 0 to exit
- Save your key file again: `gpg --armor --export 3AA5C34371567BD2`

### ### Expired Keys

- delete the old key in ```/.git-crypt/keys/default/0/``` folder

- add the user again.  The new key should have the same email address as the old key

### Creating a Keyring

- `gpg --no-default-keyring --keyring <NAMEOFKEYRING.gpg> --fingerprint`

- `gpg --no-default-keyring --keyring <NAMEOFKEYRING.gpg> --import <path to key file>`
  
  <!--stackedit_data:
  eyJoaXN0b3J5IjpbMTg3Njk4NjI2OF19
  -->
