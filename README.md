## FirstCryptoBank

- Copyright (c) 2017-2018 The FirstCryptoBank Project.
- Portions Copyright (c) 2012-2013 The Cryptonote developers.

## Compiling FirstCryptoBank Wallet from source

## Install library

	sudo apt-get update
	sudo apt-get install build-essential g++ python-dev autotools-dev libicu-dev libbz2-dev  git

	sudo apt-get install libldns-dev pkg-config cmake libssl-dev libzmq3-dev libunbound-dev 
	sudo apt-get install libsodium-dev 
	sudo apt-get install libminiupnpc-dev libunwind8-dev liblzma-dev libreadline6-dev libexpat1-dev graphviz libgtest-dev doxygen
	sudo apt-get install qt5-default

### Build instructions

**1. Clone wallet sources**

```
git clone https://github.com/FirstCryptoBank/fcbwallet.git
```

**2. Set symbolic link to coin sources at the same level as `src`. For example:**

```
ln -s ../firstcryptobank firstcryptobank
```

Alternative way is to create git submodule:

```
git submodule add https://github.com/FirstCryptoBank/firstcryptobank.git firstcryptobank
```

Replace URL with git remote repository of your coin.

**3. Build**

```
mkdir build && cd build && cmake .. && make
```
