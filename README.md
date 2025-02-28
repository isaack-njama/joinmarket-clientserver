![Release workflow status](https://github.com/JoinMarket-Org/joinmarket-clientserver/actions/workflows/unittests.yml/badge.svg)

# joinmarket-clientserver

JoinMarket is software to create a special kind of bitcoin transaction called a CoinJoin transaction. Its aim is to improve the confidentiality and privacy of bitcoin transactions.

A CoinJoin transaction requires other people to take part. The right resources (coins) have to be in the right place, at the right time, in the right quantity. This isn't a software or tech problem, it's an economic problem. JoinMarket works by creating a new kind of market that would allocate these resources in the best way.

One group of participants (called market makers) will always be available to take part in CoinJoins at any time. Other participants (called market takers) can create a CoinJoin at any time. The takers pay a fee which incentivizes the makers. A form of smart contract is created, meaning the private keys will never be broadcasted outside of your computer, resulting in virtually zero risk of loss (aside from malware or bugs). As a result of free-market forces the fees will eventually be next to nothing.

Widespread use of JoinMarket improves bitcoin's fungibility and privacy. This implementation of JoinMarket also implements [PayJoin](https://en.bitcoin.it/wiki/PayJoin).

For a quick introduction to Joinmarket you can watch [this demonstration](https://youtu.be/hwmvZVQ4C4M) of installation and usage given by [Adam Gibson](https://github.com/AdamISZ) during the [Understanding Bitcoin conference](https://understandingbtc.com/) on April 6 2019.

### Wallet features

* Segwit addresses (native bech32 ('bc1') by default; p2sh wrapped ('3') optionally).
* Multiple "mixdepths" or pockets (by default 5) for better coin isolation
* Ability to spend directly, or with coinjoin; export private keys; BIP84/49 compatible seed (Trezor, Samourai etc.) and mnemonic extension option
* Fine-grained control over bitcoin transaction fees
* Basic coin control - can freeze individual utxos to stop them being spent in any transaction
* Can run sequence of coinjoins in automated form, either auto-generated (see `tumbler.py`) or self-generated sequence.
* Can specify exact amount of coinjoin (figures from 0.01 to 30.0 btc and higher are practical), can choose time and number of counterparties
* Uses [fidelity bonds](docs/fidelity-bonds.md) for protection against sybil attacks.
* Can run passively to receive small payouts for taking part in coinjoins (see [doc page](docs/YIELDGENERATOR.md))
* GUI to support Taker role, including tumbler/automated coinjoin sequence.
* PayJoin - [BIP78](https://github.com/bitcoin/bips/blob/master/bip-0078.mediawiki) to pay users of other wallets (e.g. merchants), as well as between two compatible wallet users (Joinmarket, Wasabi, others). This is a way to boost fungibility/privacy while paying.
* Protection from [forced address reuse](https://en.bitcoin.it/wiki/Privacy#Forced_address_reuse) attacks.
* Address labeling

### Quickstart - RECOMMENDED INSTALLATION METHOD (Linux and macOS only)

Download the latest [release](https://github.com/Joinmarket-Org/joinmarket-clientserver/releases) as tar or zip, and extract it.

Make sure to validate the signature on the tar/zip file provided with the [release](https://github.com/Joinmarket-Org/joinmarket-clientserver/releases) (or check the signature in git if you install that way using `git log --show-signature`).

JoinMarket requires Python 3.7 or newer installed.

(**macOS users**: Make sure that you have Homebrew and Apple's Command Line Tools installed.)

    ./install.sh

(There are options you can apply to the installation - see `./install.sh -?`. But the defaults should work.)

Follow instructions on screen; provide sudo password when prompted, then when finished:

    source jmvenv/bin/activate
    cd scripts

You can optionally install a Qt GUI application, you will be prompted to choose this during installation.

You should now be able to run the scripts like `python wallet-tool.py` etc., just as you did in the previous Joinmarket version.

Alternative to this "quickstart": follow the [install guide](docs/INSTALL.md).

### More installation guides

* [Installation on Windows](docs/INSTALL.md#installation-on-windows).
* [Installation with Docker](docs/INSTALL.md#docker-installation)
* [Installation guide for RaspiBlitz](https://github.com/openoms/bitcoin-tutorials/blob/master/joinmarket/README.md).
* [Installation guide for RaspiBolt](https://raspibolt.org/guide/bonus/bitcoin/joinmarket.html).
* [Installation guide for Qubes+Whonix](https://github.com/qubenix/qubes-whonix-bitcoin/blob/master/1_joinmarket.md).
* [Youtube video installation tutorial for Ubuntu](https://www.youtube.com/watch?v=zTCC86IUzWo).

### Usage

If you are new, follow and read the links in the [usage guide](docs/USAGE.md).

If you are running Joinmarket-Qt, you can instead use the [walkthrough](docs/JOINMARKET-QT-GUIDE.md) to start.

If you used the old version of Joinmarket, the notes in the [scripts readme](scripts/README.md) help to understand what has and hasn't changed about the scripts (warning: this refers to changes from several years ago, so may be slightly outdated).

If you are looking for the available makers, run the [orderbook](docs/orderbook.md). It's recommended to run your own orderbook locally, but there are also public mirrors:
* https://nixbitcoin.org/orderbook (clearnet) / http://qvzlxbjvyrhvsuyzz5t63xx7x336dowdvt7wfj53sisuun4i4rdtbzid.onion/orderbook (Tor)
* http://nnuifroxn5aolsqa2svedcskojlqfp2ygt4u42ac7njehsbemagpwiqd.onion (Tor)

### Useful third-party projects

* [***JoininBox***](https://github.com/openoms/joininbox). A minimalistic, security focused linux environment for JoinMarket with a terminal based graphical menu. Good for setting up on a dedicated machine or raspberry pi: https://github.com/openoms/joininbox

* [***JAM***](https://github.com/joinmarket-webui/joinmarket-webui). Jam is a web UI for JoinMarket with focus on user-friendliness. It aims to provide sensible defaults and be easy to use for beginners while still providing the features advanced users expect: https://github.com/joinmarket-webui/joinmarket-webui

### PayJoin

If you want to use the PayJoin feature to pay/receive money to/from another [BIP78]((https://github.com/bitcoin/bips/blob/master/bip-0078.mediawiki))-supporting wallet, read [this guide](docs/PAYJOIN.md).

### Joinmarket-Qt

Provides single join and multi-join/tumbler functionality (i.e. "Taker") only, in a GUI.

If binaries are built, they will be gpg signed and announced on the Releases page.

If you haven't chosen the Qt option during installation with `install.sh`, then to run the script `joinmarket-qt.py` from the command line you will need to install two more packages.  Use these 2 commands while the `jmvenv` virtual environment is activated:

```
pip install .[gui]
```
After this, the command `python joinmarket-qt.py` from within the `scripts` subdirectory should work.
There is a [walkthrough](docs/JOINMARKET-QT-GUIDE.md) for what to do next.

### Architecture notes

See [architecture-notes.md](docs/architecture-notes.md).

### TESTING

Instructions for developers for testing [here](docs/TESTING.md). If you want to help improve the project, please have a read of [this todo list](docs/TODO.md) and the [Help Wanted tag](https://github.com/JoinMarket-Org/joinmarket-clientserver/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22) on the issue tracker.

### Community

+ IRC: `#joinmarket` on irc.libera.chat https://kiwiirc.com/nextclient/irc.libera.chat#joinmarket (logs can be found [here](https://gnusha.org/joinmarket/))

+ IRC on tor: `#joinmarket` on [Hackint](https://www.hackint.org/). This channel is bridged to the above Libera Chat channel.

+ Bitcoin wiki page: https://en.bitcoin.it/wiki/JoinMarket

+ Subreddit: https://reddit.com/r/joinmarket

+ Bitcointalk thread: https://bitcointalk.org/index.php?topic=919116.msg10096563

+ Twitter: https://twitter.com/joinmarket
https://twitter3e4tixl4xyajtrzo62zg5vztmjuricljdp2c5kshju4avyoid.onion/joinmarket

+ Telegram: https://t.me/joinmarketorg
