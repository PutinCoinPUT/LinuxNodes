# LinuxNodes

Here you can clone/download the Linux-Node Wallets for PUTinCoin:

putincoind.tar.gz = Linux CLI-node
PutinCoin-qt.tar.gz = Linux GUI Node Wallet

We recommend you to use Ubuntu 16.04, as the code was developed on it and it got all neccessary depencencies.

We don't recommend any lower version of Ubuntu. 

Also we cannot offer installation-guides for other Linux-Distributions than Ubuntu.




If you got Ubuntu 16.04:

$ sudo -s

$ apt-get install build-essential libssl-dev libdb-dev libdb++-dev libboost-all-dev git libssl1.0.0-dbg libdb-dev libdb++-dev libboost-all-dev libminiupnpc-dev libevent-dev libcrypto++-dev libgmp3-dev git

$ git clone https://github.com/PutinCoinPUT/LinuxNodes

$ cd LinuxNodes

CLI-Node: $ tar xzf putincoind.tar.gz -> cp putincoind /usr/bin (further installation see "CLI-Setup" below)

GUI-Node: $ tar xzf PutinCoin-qt.tar.gz (We think it's easiest to put a symlink to "Putincoin-qt" on Desktop)




If you got Ubuntu 18.04 and higher:

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

$ apt -y install libboost-system1.58.0 libboost-mpi1.58.0 libboost-graph-parallel1.58.0 libboost-program-options1.58.0 libboost-thread1.58.0 libboost_filesystem1.58.0 libqt5gui5 libdb5.3++-dev libdb5.3++ libopenmpi1.10 qrencode libminiupnpc10 git

$ wget http://archive.ubuntu.com/ubuntu/pool/main/b/boost1.58/libboost-filesystem1.58.0_1.58.0+dfsg-5ubuntu3.1_amd64.deb

$ dpkg -i libboost-filesystem1.58.0_1.58.0+dfsg-5ubuntu3.1_amd64.deb

$ git clone https://github.com/PutinCoinPUT/LinuxNodes

$ cd LinuxNodes

CLI-Node: $ tar xzf putincoind.tar.gz (further installation see "CLI-Setup" below)

GUI-Node: $ tar xzf PutinCoin-qt.tar.gz (We think it's easiest to put a symlink to "Putincoin-qt" on Desktop)

$ chmod 755 putincoind

$ chmod 755 PutinCoin-qt




CLI-Setup

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
rpcport=9998
port=8567
#(0=off, 1=on) daemon - run in the background as a daemon and accept commands
daemon=1
txindex=1
#(0=off, 1=on) server - accept command line and JSON-RPC commands
server=0
rpcallowip=127.0.0.1
staking=0

Save. (Strg-X -> Y)

$ putincoind &

Wallet-Deamon should be running now, check by entering

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
#PIDFile=/home/youruser/.putincoin/putincoind.pid
ExecStart=/usr/bin/putincoind -conf=/home/youruser/.putincoin/putincoin.conf
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
