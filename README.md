## Installing BFGMiner and CPUMiner on Raspberry Pi

### Initial Setup
Setup folder:

    mkdir mining
    cd mining

Install dependencies:

    sudo apt-get update
    sudo apt-get install autogen automake autoconf m4 pkg-config libgcrypt20-dev libtool uthash-dev libjansson-dev libevent-dev libcurl4-gnutls-dev libncurses5-dev screen vim

### BFGMiner
Download source:

    git clone https://github.com/luke-jr/bfgminer.git
    Cloning into 'bfgminer'...
    remote: Counting objects: 47615, done.
    remote: Total 47615 (delta 0), reused 0 (delta 0), pack-reused 47615
    Receiving objects: 100% (47615/47615), 35.68 MiB | 1.07 MiB/s, done.
    Resolving deltas: 100% (30838/30838), done.

Install:

    cd bfgminer/
    ./autogen.sh
    ./configure
    make

Running from command line:

    ./bfgminer -o stratum+tcp://stratum.bitcoin.cz:3333 -u jamesleesaunders.bert -p woof -S all
    
Running with config (see: 'bfgminer.conf' example file in this repo):

    ./bfgminer --config ../bfgminer.conf

Tutotial:
* https://computers.tutsplus.com/tutorials/how-to-create-a-raspberry-pi-bitcoin-miner--cms-20353?_ga=2.51452969.1697175413.1508571994-1473909186.1508571994

### CGMiner
Return back to parent 'mining' directory:

    cd ..

Download source:

    git clone https://github.com/ckolivas/cgminer.git
    Cloning into 'cgminer'...
    remote: Counting objects: 27988, done.
    remote: Total 27988 (delta 0), reused 0 (delta 0), pack-reused 27988
    Receiving objects: 100% (27988/27988), 19.13 MiB | 1.16 MiB/s, done.
    Resolving deltas: 100% (18298/18298), done.

Install:

    cd cgminer/
    ./autogen.sh
    CFLAGS="-O2 -Wall -march=native" ./configure --enable-bitmine_A1
    make

Running from command line:

    ./cgminer --userpass jamesleesaunders.elmo:woof --url stratum+tcp://stratum.slushpool.com:3333

### CPUMiner
Return back to parent 'mining' directory:

    cd ..

Download source:

    git clone https://github.com/pooler/cpuminer
    Cloning into 'cpuminer'...
    remote: Counting objects: 1475, done.
    remote: Total 1475 (delta 0), reused 0 (delta 0), pack-reused 1475
    Receiving objects: 100% (1475/1475), 592.51 KiB | 174.00 KiB/s, done.
    Resolving deltas: 100% (959/959), done.

Install:

    cd cpuminer/
    ./autogen.sh
    ./configure CFLAGS="-O3" # Make sure -O3 is an O and not a zero!
    make

Running from command line:

    ./minerd --url stratum+tcp://stratum.bitcoin.cz:3333 --userpass jamesleesaunders.ernie:woof

Running with config (see: 'cpuminer.conf' example file in this repo):

    ./minerd --config ../cpuminer.conf

Tutorial:
* https://www.gadgetdaily.xyz/mine-bitcoins-with-raspberry-pi/

### Create Startup Service
Copy file init.d script (see: 'miners' file in this repo) to /etc/init.d/ and set correct permissions:
    
    cp ./miners /etc/init.d/
    sudo chmod 755 /etc/init.d/miners
    sudo chown root:root /etc/init.d/miners
    sudo /etc/init.d/miners start
    
Reboot Rasbberr Pi:

    sudo reboot
    
### Other Random Notes

    screen -ls
    sudo screen -dr bfgminer
    CTRL+A+D
