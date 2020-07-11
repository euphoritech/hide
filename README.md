# hide

Open source, AES-256 bit encrypted password manager with all encrypted passwords stored locally on your machine.

## Usage

```
$ hide search my_new_account
I found the following accounts:

0 of 412 total accounts returned


$ hide add -n my_new_account -u myname -p the_secret_password -e "Some extra stuff!!!!"
Successfully added account 'my_new_account'!


$ hide search my_new_account
I found the following accounts:
NAME            USERNAME        EXTRA                UUID
my_new_account  myname          Some extra stuff!!!! 964c0e29-9732-4f03-9920-86b35cd04afe
1 of 413 total accounts returned


$ hide show my_new_account
NAME            USERNAME        EXTRA                UUID
my_new_account  myname          Some extra stuff!!!! 964c0e29-9732-4f03-9920-86b35cd04afe


$ hide show my_new_account -p
NAME            USERNAME        PASSWORD            EXTRA                UUID
my_new_account  myname          the_secret_password Some extra stuff!!!! 964c0e29-9732-4f03-9920-86b35cd04afe
```

## Description

This is a CLI utility that can be used to store your account information,
including websites, usernames, passwords, and additional info, securely using
AES-256 bit encryption using a master secret that you configure.

## Install

```
$ npm install -g hide
```

## Why?

I've previously used Last Pass to securely store passwords for me within the
context of a browser extension, and it worked decent. The problem is I not only
have a ton of accounts with different passwords that I like to see,
but I'm a developer so basically always live in a terminal window.
This tool gives me the freedom to retrieve a username and/or password (among other information)
about an account with a single command, and store it on my machine using AES-256
bit encryption with a password I set.

## Config

#### CRYPT_SECRET (required)

The following should be set to control the global secret that's used with AES-256 bit encryption to secure the data stored on your machine.

**DON'T LOSE/FORGET THIS**

```
$ export CRYPT_SECRET=[your all time master secret value]
```

#### NODE_HIDE_FILEPATH

Directory where the encrypted file will live -- default: home directory (either process.env.HOME on unix/linux/mac or process.env.USERPROFILE on windows)

```
$ export NODE_HIDE_FILEPATH=~
```

#### NODE_HIDE_FILENAME

Name of flat file that holds the encrypted data of your accounts -- default: '\_\_node-hide-accounts'

```
$ export NODE_HIDE_FILENAME=my_encrypted_file
```

## Usage

### Add an account

#### PARAMETERS

- -n / --name (REQUIRED): The name of the account you're storing. It can be any combination of alphanumeric characters.
- -u / --username (optional): The username for the account.
- -p / --password (optional): The password for the account.
- -e / --extra (optional): Any additional information you'd like to provide about the account.

```
$ hide add -n my_new_account -u myname -p the_secret_password -e "Some extra stuff!!!!"

Successfully added account 'my_new_account'!
```

### Search your accounts

#### PARAMETERS

- -s / --search (optional): An optional term to look for accounts based on
  a case-insensitive search against the NAME or USERNAME.
  NOTE: the `search` command never shows the password for the account. Use `show` to retrieve the password.

```
$ hide search

I found the following accounts:
NAME                USERNAME        EXTRA            UUID
facebook.com        userna                           def7f984-c2d7-4069-907c-facfad597123
instagram.com       iguser                           def7f984-abc1-1111-2222-facfad597123
2 of 2 total accounts returned

$ hide search -s facebook

I found the following accounts:
NAME                USERNAME        EXTRA            UUID
facebook.com        userna                           def7f984-c2d7-4069-907c-facfad597123
1 of 2 total accounts returned
```

### Show a single account

#### PARAMETERS

Either uuid or name are at least required:

- -i / --uuid: The unique identifier of the account you want to review.
- -n / --name: The name of the account you're reviewing.

Optional

- -p / --password (optional): Whether to show the password. DEFAULT: false

```
$ hide show -i f62d5a21-4119-4a05-bced-0dca8f310d4b
$ hide show -n facebook.com

NAME            USERNAME        EXTRA           UUID
facebook.com    fbuser                          f62d5a21-4119-4a05-bced-0dca8f310d4b

$ hide show -n facebook.com -p

NAME            USERNAME        PASSWORD        EXTRA           UUID
facebook.com    fbuser          my_password1                    f62d5a21-4119-4a05-bced-0dca8f310d4b
```

### Update an account

#### PARAMETERS

Either uuid or name are at least required:

- -i / --uuid: The unique identifier of the account you want to update.
- -n / --name: The name of the account you're updating.

Optional

- -u / --username (optional): The username for the account.
- -p / --password (optional): The password for the account.
- -e / --extra (optional): Any additional information you'd like to provide about the account.

```
$ hide update -n facebook.com -u fbuser -p my_password1

Successfully updated account with name: 'facebook.com'!
```

### Delete an account

#### PARAMETERS

- -i / --uuid: The unique identifier of the account you want to delete.

```
$ hide delete -i f62d5a21-4119-4a05-bced-0dca8f310d4b

Successfully deleted account with uuid: 'f62d5a21-4119-4a05-bced-0dca8f310d4b'
```

### Re-encrypt your current file with a new secret

```
$ hide recrypt yourNew_Ultra_Secret_secret!@#

Successfully updated your encrypted file with new secret to: /Users/username/__node-hide-accounts
```

### Get the full file path of the encrypted flat file

Returns the file location on your machine where the
encrypted file lives.

#### PARAMETERS

None

```
$ hide file

Your encrypted file is in the following location:
/Users/yourname/__node-hide-accounts
```

### Decrypt file and store on disk

Decrypts the encrypted file that is used by hide to store your account
information, and stores it on disk in the same location as your file
with an appended '.json' to the file name.

#### PARAMETERS

None

```
$ hide decryptfile
Are you sure you want to decrypt your file and save it to disk (yes/no): yes

Successfully saved your decrypted account data to:
/Users/yourname/__node-hide-accounts.json
```

### Import from a CSV

Note: This requires the CSV have headers that match the following:

- `name`: the account name
- `username`: the username of the account
- `password`: the password of the account
- `extra`: any desired extra information you want to store with the account

#### PARAMETERS

- -f / --filepath: The full filepath of the CSV that we're importing data from.

```
$ hide import -f /Users/yourname/myfile.csv

Successfully added 123 accounts from CSV: /Users/yourname/myfile.csv!
```

### Encrypt text or a file

#### PARAMETERS

- -t / --text: Text you want to encrypt.
- -f / --file: A local file path containing data you want to encrypt.

```
$ hide encrypt -t testing123
$ hide encrypt testing123

0f318802819cb67ea05c
```

### Decrypt text or a file

#### PARAMETERS

- -t / --text: Text you want to decrypt.
- -f / --file: A local file path containing data you want to decrypt.

```
$ hide decrypt -t 0f318802819cb67ea05c
$ hide decrypt 0f318802819cb67ea05c

testing123
```

## Development

If you want to clone and add/update functionality, you can build
using the following.

### Build

```
$ npm run build
```

### Tests

```
$ npm test
```
