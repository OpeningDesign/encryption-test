This repo is a test for encryption. It contains one clear file (this one) and a couple of encrypted ones. It uses [git-crypt](https://www.agwa.name/projects/git-crypt/)

## Working with git crypt

### Linux

If you are creating the repo:

- Install git-crypt from your distro repo
- Clone or init a git repo
- Create a .gitattribute file where you specify what must be encrypted
- Either add your pgp user `git crypt add-gpg-user your_pgp_email@somethingg.com` or export a symmetric key `git crypt export-key /path/to/keyfile`
- Add other pgp users if needed (you must have their public key and they need trust level 5 - see below)
- Push

If you are cloning an existing encrypted repo:

- If you are already added as a pgp user: `git crypt unlock`
- If you have the symmetric key: `git crypt unlock /path/to/keyfile`

### Windows


#### If you are creating the repo:

- download [git-crypt.exe](https://github.com/LykkeCity/git-crypt/releases) and locate the file in the repo folder
- Open a command window at the repo folder
  - One method: Navigate to the project folder, hold down the Shift key and right-click the folder. The context menu will contain an entry, ‘Open command window here.”
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
				    -   1 = I don't know or won't say
	  			    -   2 = I do NOT trust
	  			    -   3 = I trust marginally
	  			    -   4 = I trust fully
	  			    -   5 = I trust ultimately
	  			    -   m = back to the main menu
			    - Your decision? `5`
			    - Do you really want to set this key to ultimate trust? (y/N) `y`
			    - gpg> `q`
			    - 	Push to remote repo
    - export a symmetric key `./git-crypt export-key C:/path/to/filename.gpg`
      - Share this key with fellow collaborators
      - Save in a safe location
	- Push to remote repo


#### To unlock a repo with a 'collaboration' key:
- Your collaborator has given you {filename}.gpg file.  Store somewhere safe.  Remember the file's path, for steps below.
- Open a command window at the repo folder (top most level -- that is no subdirectiories)
  - One method: Navigate to the project folder, hold down the Shift key and right-click the folder. The context menu will contain an entry, ‘Git Bash Here”
- `./git-crypt.exe unlock C:/path/to/filename.gpg`
- All encrypted file/folders should be accessible now.

#### To see a list of Collaborators already associated with the repo
- `git log .git-crypt/`

## Working with PGP keys

A PGP key is made of two keys: One private/secret, that you should NEVER give to anyone, and one public, that you can give to as many people as you want or even make publicly available (on your website, in your email signature, etc). 

Each key of the pair can decrypt what the other encrypted. So other people who have your public key can read encrypted files/text that you and only you issued (and therefore certify that what they are reading can only have been made by you), and they can also send you text/files that only you can read, by encrypting them with your public key.

Once you generated a key pair, both can be exported to .asc files, and these files can be imported on other machines (cellphones, other computers) to be able to encrypt/decript with the same keys. It is also important to keep a backup of those two files otherwise what has been encrypted with any of them is lost forever.

Remeber that like any encryption system, although pgp encryption is unbreakable right now, one day, nobody can tell when,  it will become breakable and the content you encrypt today will become readable.

### Linux

#### Creating a key for yourself

* Install gpg from your distro repo
* Open a terminal
* `gpg --full-generate-key` and accept all the defaults: RSA/RSA,3072 bits, 0 - never expire
* Give your name, your email (a pgp key is bound to an email), and optionally a comment
* `gpg --list-keys` lists all the keys installed for your user. Check that your new key is there
* If you need to give your public key to other people, export your public key as an .asc file: `gpg --armor --export you@server.com > /home/youruser/my_public_key.asc`
* It is a good idea to backup both your public and private asc files. Generate another one for your private key with `gpg --armor --export-secret-keys regisndetene@gmail.com > /home/youruser/my_private_key.asc`

#### Adding PGP keys of other people

`gpg --import /path/to/some_key.asc`
