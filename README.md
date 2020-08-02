# d24n Corporate Documents

In the spirit of dogfooding and general neurosis, I am keeping the Decentralization Foundation's
"corporate books" as much as I can in [a web application which notarizes each document on the
Ethereum blockchain](https://github.com/docstore-bureaucracy/d24n-0x1a4934109b54911a724dfa0e45d5370dbbe923b0).

You can find the source code and general docs of this "ethdocstore" application
at GitHub [`swaldman/ethdocstore`](https://github.com/swaldman/ethdocstore). Ingested documents
are also automatically uplaoded to private github respository [`docstore-bureaucracy/d24n-0x1a4934109b54911a724dfa0e45d5370dbbe923b0`](https://github.com/docstore-bureaucracy/d24n-0x1a4934109b54911a724dfa0e45d5370dbbe923b0).

If you wish to gain access to private "locked" documents, you'll need to get in touch
with an Ethereum address that can become an authorized user of the [Ethereum contract behind this application](https://github.com/docstore-bureaucracy/d24n-0x1a4934109b54911a724dfa0e45d5370dbbe923b0).

Once your Ethereum address has been authorized, you will be able to make your own web login credentials (username and password) using the client in
this repository.

As a prerequisite, you will need a Java 8 or Java 11 virtual machine installed on your computer. Then it would go something like this.
First clone the repository, and make sure the Ethereum address is imported into your [_sbt-ethereum_](https://www.sbt-ethereum.io/) shoebox. If you don't know what that
means, don't worry. We'll walk you through. _sbt-ethereum_ is very tab-complete, with long commands that are annoying to type in but easy to tab through.
(If you want a quick tutorial on tab-completing, try [here](https://www.sbt-ethereum.io/tutorials/getting-started.html#run-your-first-command).) Alternatively, you can
just copy and paste the commands from this tutorial.

```
$ git clone https://github.com/swaldman/d24n-docstore-client.git
Cloning into 'd24n-docstore-client'...
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 10 (delta 1), reused 10 (delta 1), pack-reused 0
Unpacking objects: 100% (10/10), done.
$ cd d24n-docstore-client/
$ ./sbtw
```

You might see a bunch of scrolling and messages at this point, as sbt and related libraries load from the internet.
It won't take as long the second time. Next you'll need to import your address. If you have a JSON (v3) Ethereum Wallet file,
you can use

```
sbt:d24n-client> ethKeystoreWalletV3FromJsonImport
V3 Wallet JSON:
```

Paste in the JSON text of your wallet file, then...

```
[info] Imported JSON wallet for address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D', but have not validated it.
[info] Consider validating the JSON using 'ethKeystoreWalletV3Validate 0xc33071eAd8753B04e0ee108CC168f2B22F93525D'.
Would you like to define an alias for address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (on chain with ID 1)? [y/n] y
Please enter an alias for address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (on chain with ID 1): d24n-authorized
[info] Alias 'd24n-authorized' now points to address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (for chain with ID 1).
[info] Refreshing caches.
[success] Total time: 52 s, completed Aug 2, 2020, 6:19:42 PM
```

Now you have imported the address, and given it an alias (I've chose 'd24n-authorized' for this exercise.)
If you do not have a JSON wallet file, but have the raw public key (Danger Will Robinson!), just use the command `ethKeystoreWalletV3FromPrivateKeyImport` instead:

```
sbt:d24n-client> ethKeystoreWalletV3FromPrivateKeyImport
Please enter the private key you would like to import (as 32 hex bytes): ******************************************************************
The imported private key corresponds to address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D'. Is this correct? [y/n] y
Generating V3 wallet, algorithm=pbkdf2, c=262144, dklen=32
Enter passphrase for new wallet: *******************
Please retype to confirm: *******************
[info] Wallet created and imported into sbt-ethereum shoebox: '/Users/swaldman/Library/Application Support/sbt-ethereum'. Please backup, via 'ethShoeboxBackup' or manually.
[info] Consider validating the wallet using 'ethKeystoreWalletV3Validate 0xc33071eAd8753B04e0ee108CC168f2B22F93525D'.
Would you like to define an alias for address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (on chain with ID 1)? [y/n] y
Please enter an alias for address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (on chain with ID 1): d24n-authorized
[info] Alias 'd24n-authorized2' now points to address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (for chain with ID 1).
[info] Refreshing caches.
[success] Total time: 51 s, completed Aug 2, 2020, 6:27:52 PM
```

Great. Now you have access to your authorized address within this application. Now we can create a web login with it.
First make sure the new address is your current session address. You could use the hex address, but we'll use the alias we've defined for it:

```
sbt:d24n-client> ethAddressOverride d24n-authorized
[info] Sender override set to '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (on chain with ID 1, aliases ['d24n-authorized']).
[success] Total time: 0 s, completed Aug 2, 2020, 6:31:46 PM
```

Then we can go ahead and register:

```
sbt:d24n-client> docstoreRegisterUser
You would be registering as '0xc33071eAd8753B04e0ee108CC168f2B22F93525D'. Is that okay? [y/n] y
Username: test-user
Password: ********
Confirm password: ********
You will be asked to sign a challenge in order to prove you are associated with address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D'.
[info] Challenge successfully received.

==> D O C U M E N T   S I G N A T U R E   R E Q U E S T
==>
==> The signing address would be '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (with aliases ['d24n-authorized'] on chain with ID 1).
==> 
==> This data does not appear to be a transaction for chain with ID 1.
==>
==> Raw data: 0x5369676e6174757265204368616c6c656e6765202d2052616e646f6d2042797465733a202a0a82b0f3bfa9854917080346539c8d363db361c1673935e2d9897cb0942fb8
==>
==> Raw data interpreted as as UTF8 String: "Signature Challenge - Random Bytes: *\n��󿩅I\027\b\003FS��6=�a�g95�ى|��/�"


Are you sure it is okay to sign this data as '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (with aliases ['d24n-authorized'] on chain with ID 1)? [y/n] y
[info] Unlocking address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D' (with aliases ['d24n-authorized'] on chain with ID 1).
Enter passphrase or hex private key for address '0xc33071eAd8753B04e0ee108CC168f2B22F93525D': *******************
[info] Registration accepted.
[info] User 'test-user' successfully registered as '0xc33071eAd8753B04e0ee108CC168f2B22F93525D'.
[success] Total time: 68 s (01:08), completed Aug 2, 2020, 6:34:49 PM
```

Now, go to the [d24n docstore application](https://docstore.bureaucracy.com/0x1A4934109b54911A724dFA0e45D5370dbBE923B0/index.html)
and click on a private ("locked") link. The web page will ask you to log in. Use the username / password pair you entered when you registered above.

And you are done!

