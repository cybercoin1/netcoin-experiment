NETCoin - An SHA256 PoW Cloner!

Based on ABCCoin and shakezula's work on cloning Bitcoin and scrypt-based coins from some time in 2013.

I did an awful job of renaming everything, because I didn't really care at the time.  Use Sublime to open everything and select "Find in Files" and add the top level directory to search in.  Check case-sensitive (so it's true) and replace "ABCCoin" with "..." and "ABCCOIN" and "AbcCoin".

I downloaded Berkeley DB 4.8.30-NC from Oracle and OpenSSL 1.0.1u to build this "coin".

But, that was after I cleaned up two old laptops.  I did a fresh install of Parrot OS, a security related distro based on Debian, which has development tools.  Aside from DB and OpenSSL libs, all the others were fine as far as I could tell.

In your ~/Downloads folder, tar zxf db-4.8.30-NC.tar.gz, cd in there, and cd build_unix.  In build_unix, ../dist/configure --prefix=/where/it/goes --enable-cxx.  make and make install.  Depending on your prefix you might have to sudo make install, if it's going to e.g. /usr/local.

In your ~/Downloads folder again, tar zxf openssl-1.0.1e.tar.gz, cd into there, and ./configure --prefix=/opt (for example).  Then make && (sudo) make install.

Add libminiupnp, usually with sudo apt-get install libminiupnpc-dev.

As you can see I didn't have to do much.  I uncommented the part that makes a genesis block and took it from ~/.netcoin/debug.log and pasted in the block details (block hash, nonce, merkle hash) and recompiled.  You might want to put your genesis block hash in the check points.  I did, and didn't bother with any further blocks, even though I had my block reward at 10 coins and created 35,000 of them.

It fails out twice, once with an assert on genesis block hash, and the second time with the rpcuser and rpcpassword.  I ran on my network with ~/dir/src/netcoind -connect=10.1.1.100 -listen & on one laptop, and ~/dir/src/netcoind -connect=10.1.1.121 -listen & on the other laptop.  You specify the IP addresses of the other machines, so you connect to e.g. 10.1.1.121 on 10.1.1.100 and vice versa.

After getting that going and checking debug.log, and creating ~/.netcoin/netcoin.conf, I commented out the genesis block creation code after generating it once, recompiled, and let it run.  Then qmake in the top level source directory, and make to build netcoin-qt.  Again, with the same command line parameters, ./netcoin-qt -connect=x -listen & to use the GUI.

You could check your addresses and import a key after running once, then pay yourself adding the Coinbase address for the genesis block, perhaps, although I haven't checked that.  I just leave them, since it was never a real coin anyway.  I just wanted to see it all work.

If whatevercoin-qt fails to link and it's not an include or library path issue, you might have to add the dynamic link flag on the LIBS line for the Makefile, -ldl.  I never built static, so you will want that for a dynamic build against system libraries.
