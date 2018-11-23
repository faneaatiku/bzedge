# BitcoinZ BZEdge
**Keep running wallet to strengthen the BZedge network. Backup your wallet in many locations & keep your coins wallet offline. Copy your BitcoinZ walet into BZedge folder. BitcoinZ funds before 17 June 2018 will be duplicated on BZEdge. Replay protection: Send entire amount in each transparent address to a new address (including the free 0.1BZE). Those coins will be replay protected. To protect Z funds from replay, send your "protected" coin to the Z address, then the whole balance in one transaction to the next address.**

### Ports:
- RPC port: 1980
- P2P port: 1990

Install
-----------------
### Linux



Get dependencies
```{r, engine='bash'}
sudo apt-get install \
      build-essential pkg-config libc6-dev m4 g++-multilib \
      autoconf libtool ncurses-dev unzip git python \
      zlib1g-dev wget bsdmainutils automake
```

Install

```{r, engine='bash'}
# Clone BZedge Repository
git clone https://github.com/bzedge/bzedge.git
# Build
cd bzedge/
./zcutil/build.sh -j$(nproc)
# fetch key
./zcutil/fetch-params.sh
# Run
./src/bitcoinzd
# Test getting information about the network
cd src/
./bitcoinz-cli getmininginfo
# Test creating new transparent address
./bitcoinz-cli getnewaddress
# Test creating new private address
./bitcoinz-cli z_getnewaddress
# Test checking transparent balance
./bitcoinz-cli getbalance
# Test checking total balance 
./bitcoinz-cli z_gettotalbalance
# Check all available wallet commands
./bitcoinz-cli help
# Get more info about a single wallet command
./bitcoinz-cli help "The-command-you-want-to-learn-more-about"
./bitcoinz-cli help "getbalance"
```

### Docker

Build
```
$ docker build -t btcz/bitcoinz .
```

Create a data directory on your local drive and create a bitcoinz.conf config file
```
$ mkdir -p /ops/volumes/bitcoinz/data
$ touch /ops/volumes/bitcoinz/data/bitcoinz.conf
$ chown -R 999:999 /ops/volumes/bitcoinz/data
```

Create bitcoinz.conf config file and run the application
```
$ docker run -d --name bitcoinz-node \
  -v bitcoinz.conf:/bitcoinz/data/bitcoinz.conf \
  -p 1990:1990 -p 127.0.0.1:1980:1980 \
  btcz/bitcoinz
```

Verify bitcoinz-node is running
```
$ docker ps
CONTAINER ID        IMAGE                  COMMAND                     CREATED             STATUS              PORTS                                              NAMES
31868a91456d        btcz/bitcoinz          "bitcoinzd --datadir=..."   2 hours ago         Up 2 hours          127.0.0.1:1979->1979/tcp, 0.0.0.0:1989->1989/tcp   bitcoinz-node
```

Follow the logs
```
docker logs -f bitcoinz-node
```

The cli command is a wrapper to bitcoinz-cli that works with an already running Docker container
```
docker exec -it bitcoinz-node cli help
```

## Using a Dockerfile
If you'd like to have a production btc/bitcoinz image with a pre-baked configuration
file, use of a Dockerfile is recommended:

```
FROM btcz/bitcoinz
COPY bitcoinz.conf /bitcoinz/data/bitcoinz.conf
```

Then, build with `docker build -t my-bitcoinz .` and run.

### Windows
Windows build is maintained in [bitcoinz-win project](https://github.com/bitcoinz-pod/bitcoinz-win).

Security Warnings
-----------------

**BZedge is experimental and a work-in-progress.** Use at your own risk.

### For MACos use the build/macos branch and make sure you install the following:
```
brew install automake autoconf libtool pkg-config
xcode-select --install
```

In order to build use build-mac.sh with --disable-libs

```
./zcutil/build-mac.sh --disable-libs -j$(nproc)
```
Some errors have been reported on Mojave OS
Try installing macOS_SDK_headers_for_macOS_10.14.pkg package from the following location: /Library/Developer/CommandLineTools/Packages/
