This repo is a test for encryption. It contains one clear file (this one) and a couple of encrypted ones. It uses [git-crypt](https://www.agwa.name/projects/git-crypt/)

Basic procedure:

### Linux

If you are creating the repo:

- Install git-crypt from your distro repo
- Clone or init a git repo
- Create a .gitattribute file where you specify what must be encrypted
- Either add your pgp user `git crypt add-gpg-user your_pgp_email@somethingg.com` or export a symmetric key `git crypt export-key /path/to/keyfile`
- Add other pgp users if needed (you must have their public key)
- Push

If you are cloning an existing encrypted repo:

- If you are already added as a pgp user, nothing to do
- If you have the symmetric key: `git crypt unlock /path/to/keyfile`

### Windows


#### If you are creating the repo:

- download [git-crypt.exe](https://github.com/LykkeCity/git-crypt/releases) and locate the file in the repo folder
- Open a command window at the repo folder
  - One method: Navigate to the project folder, hold down the Shift key and right-click the folder. The context menu will contain an entry, ‘Open command window here.”
- `./git-crypt init`
- Create a `.gitattributes` file (This file is where you give instructions on which files/folders will be encrypted)
  - One method:  Download and locate [this file](https://raw.githubusercontent.com/OpeningDesign/New_2nd_Story/master/_CLOSED_New_2nd_Story/.gitattributes) in the folder you would like to be encrypted.  All subfolders will be encrypted as well.
- Adding users or creating a 'collaboration' key.
  - Either...
    - add your pgp user `./git-crypt add-gpg-user your_pgp_email@somethingg.com` or 
    - export a symmetric key `./git-crypt export-key C:/path/to/{filename}.gpg`
      - Share this key with fellow collaborators
      - Save in a safe location
  - Add other pgp users if needed (you must have their public key)
- Push


#### To unlock a repo with a 'collaboration' key:
- Your collaborator has given you {filename}.gpg file.  Store somewhere safe.  Remember the file's path, for steps below.
- Open a command window at the repo folder
  - One method: Navigate to the project folder, hold down the Shift key and right-click the folder. The context menu will contain an entry, ‘Open command window here.”
- `./git-crypt.exe unlock C:/path/to/{filename}.gpg
- All encrypted file/folders should be accessible now.


**Warning** any file committed before it has been added to .gitattributes will not be encrypted
