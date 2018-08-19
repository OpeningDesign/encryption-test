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


If you are creating the repo:

- Install [Git](https://git-scm.com/download/win)
- Optionally, install git-crypt.exe system-wide by copying into one of the Windows system folders or adding its path to the [system path](https://www.addictivetips.com/windows-tips/set-path-environment-variables-in-windows-10/)
- Clone or init a git repo
- Open a command window (Start menu -> type `cmd`)
- Navigate to the git repo
- Either add your pgp user `git-crypt add-gpg-user your_pgp_email@somethingg.com` or export a symmetric key `git-crypt export-key C:\path\to\keyfile`
- Add other pgp users if needed (you must have their public key)
- Push

If you are cloning an existing encrypted repo:

- If you are already added as a pgp user, nothing to do
- If you have the symmetric key: `git-crypt unlock C:\path\to\keyfile`

**Warning** any file committed before it has been added to .gitattributes will not be encrypted
