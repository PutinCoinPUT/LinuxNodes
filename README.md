# LinuxNodes

Here you can clone/download the Linux-Node Wallets for PUTinCoin:

putincoind.tar.gz = Linux CLI-node<br />
PutinCoin-qt-ubuntu.tar.gz = Ubuntu Linux GUI Node Wallet (tested up to Ubuntu 18.04)<br />
PutinCoin-qt-mint.tar.gz = Mint Linux GUI Node Wallet (tested with Cinnamon Edition)<br />
PutinCoin-qt-kali.tar.gz = Mint Linux GUI Node Wallet (tested with Kali Linux 2023.4)<br />

IMPORTANT: If you update an existing wallet, just replace the old executable with the new one, but pls. shut down the old wallet before, if it is running.

We recommend you to use Ubuntu 18.04, as the code was developed on it and it got all neccessary depencencies.

We don't recommend any lower version of Ubuntu, Mint & Kali. 

Also we cannot offer a detailed installation-guide for other Linux-Distributions than Ubuntu.

Command-Parameters for the "putincoind"-CLI Daemon you can find here: https://en.bitcoin.it/wiki/Original_Bitcoin_client/API_calls_list

We have added GUI node wallet compilations for MINT Linux & KALI Linux because of increasing demand from stakers.

For installing on Mint & Kali you need to get the following libraries by the respective "apt install" commands:

* libboost-all-dev
* libqt5gui5
* libdb5.3++-dev
* libdb5.3++
* libopenmpi-dev
* qrencode
* libminiupnpc-dev
* git

Then you need to make sure that the respective PutinCoin-qt executable has got the rights to be executed (755)!

The detailed description below is only for the Ubuntu GUI & CLI Node Wallet!

Ubuntu 18.04 and higher:

$ sudo -s

$ nano /etc/apt/sources.list.d/xenial-backports.list

Copy/Paste:

deb http://it.archive.ubuntu.com/ubuntu/ xenial main restricted
deb http://it.archive.ubuntu.com/ubuntu/ xenial universe
deb http://it.archive.ubuntu.com/ubuntu/ xenial-updates universe
deb http://it.archive.ubuntu.com/ubuntu/ xenial multiverse
deb http://it.archive.ubuntu.com/ubuntu/ xenial-updates multiverse
deb http://security.ubuntu.com/ubuntu xenial-security main restricted
deb http://security.ubuntu.com/ubuntu xenial-security universe
deb http://security.ubuntu.com/ubuntu xenial-security multiverse

Save. (Strg-X -> Y)

$ apt -y update

$ apt -y install libboost-system1.58.0 libboost-mpi1.58.0 libboost-graph-parallel1.58.0 libboost-program-options1.58.0 libboost-thread1.58.0 libqt5gui5 libdb5.3++-dev libdb5.3++ libopenmpi1.10 qrencode libminiupnpc10 git

$ wget http://archive.ubuntu.com/ubuntu/pool/main/b/boost1.58/libboost-filesystem1.58.0_1.58.0+dfsg-5ubuntu3.1_amd64.deb

$ dpkg -i libboost-filesystem1.58.0_1.58.0+dfsg-5ubuntu3.1_amd64.deb

$ git clone https://github.com/PutinCoinPUT/LinuxNodes

$ cd LinuxNodes

<strong>CLI-Node:</strong> $ tar xzf putincoind.tar.gz (further installation <strong>see "CLI-Setup:" below</strong>)

<strong>GUI-Node:</strong> $ tar xzf PutinCoin-qt.tar.gz (We think it's easiest to put a symlink to "Putincoin-qt" on Desktop)

$ chmod 755 putincoind <strong>(only for CLI)</strong>

$ chmod 755 PutinCoin-qt <strong>(only for GUI)</strong>




<strong>CLI-Setup:</strong>

$ sudo -s

$ cp putincoind /usr/bin

$ putincoind &

You get the following message:

"Error: To use putincoind, you must set a rpcpassword in the configuration file:
 /home/yourusername/.putincoin/putincoin.conf
It is recommended you use the following random password:
rpcuser=putincoinrpc
rpcpassword=13KEAXFFhx4CrE7pZNzKuBjZxaZZFrXwcGiNKLjQnR6f
(you do not need to remember this password)
The username and password MUST NOT be the same.
If the file does not exist, create it with owner-readable-only file permissions.
It is also recommended to set alertnotify so you are notified of problems;
for example: alertnotify=echo %s | mail -s "PutinCoin Alert" admin@foo.com"

$ cd ~/.putincoin

Delete everyhting in this folder EXCEPT "wallet.dat"  (folders need to be removed with "rm -rf")

$ nano putincoin.conf

Copy/Paste:

rpcuser=chooseyourownrpcusername
rpcpassword=chooseyourownrpcpassword
#Choose the RPC-Port you want to open for the node
rpcport=9998
rpcallowip=127.0.0.1
#The PnP-Port should be left at 8567
port=8567
#(0=off, 1=on) daemon - run in the background as a daemon and accept commands
daemon=1
txindex=1
listen=1
#(0=off, 1=on) server - accept command line and JSON-RPC commands
server=0
enableaccounts=0
staking=0

Save. (Strg-X -> Y)

$ putincoind &

Wallet-Daemon should be running now, check by entering

$ putincoind getinfo

It takes around 12 - 24 hours until the cli-node has fully synced with the PUT-blockchain


If you want to autostart the service on system boot or reboot you must set up a systemd service:

$ sudo -s

$ cd /etc/systemd/system

$ nano putincoind.service

Copy/Paste:

[Unit]
Description=Putincoin daemon
After=network.target

[Service]
User=root
Group=root
Type=forking
PIDFile=/home/REPLACE BY YOUR USERNAME !!/.putincoin/putincoind.pid
ExecStart=/usr/bin/putincoind -conf=/home/REPLACE BY YOUR USERNAME !!/.putincoin/putincoin.conf -pid=/home/REPLACE BY YOUR USERNAME !!/.putincoin/putincoind.pid -datadir=/home/REPLACE BY YOUR USERNAME !!/.putincoin
KillMode=process
Restart=always
TimeoutSec=120
RestartSec=30

[Install]
WantedBy=multi-user.target

Save. (Strg-X -> Y)

$ chmod 755 putincoind.service

$ systemctl daemon-reload

$ systemctl enable putincoind.service

$ putincoind stop

$ service putincoind start
