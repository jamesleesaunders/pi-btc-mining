## Installing BFGMiner and CPUMiner on Raspberry Pi

### Initial Setup
Setup folder:

    mkdir mining
    cd mining

Alternativly:

    git clone https://github.com/jamesleesaunders/pi-btc-mining.git

Install dependencies:

    sudo apt-get update
    sudo apt-get install autogen automake autoconf m4 pkg-config libgcrypt20-dev libtool uthash-dev libjansson-dev libevent-dev libcurl4-gnutls-dev libncurses5-dev screen vim libbase58-0 libblkmaker-0.1-6 libclang-common-3.9-dev libclc-amdgcn libclc-dev libclc-r600 libmicrohttpd12 mesa-opencl-icd ocl-icd-libopencl1

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
    git checkout bf292c686d749ddd908b3832ae25e8bc3b307b7d (this version works better on Pi?)
    ./autogen.sh
    ./configure
    make

Running from command line:

    ./bfgminer -o stratum+tcp://stratum.bitcoin.cz:3333 -u jamesleesaunders.bert -p woof -S all
    
Running with config (see: [bfgminer.conf](bfgminer.conf) example file):

    ./bfgminer --config ../bfgminer.conf

To overclock bfgminer Antminer U1 add changing the 'clock' as per table below:

    ./bfgminer -o stratum+tcp://stratum.bitcoin.cz:3333 -u jamesleesaunders.bert -p woof -S antminer:all --set-device antminer:clock=x0881

| Frequency | Hash Rate (GH/s) | antminer:clock |
|-----------|------------------|----------------|
| 200       | 1.6 (default)    | x0781          |
| 225       | 1.8              | x0881          |
| 250       | 2.0              | x0981          |
| 275       | 2.2              | x0A81          |

Tutotial:
* https://computers.tutsplus.com/tutorials/how-to-create-a-raspberry-pi-bitcoin-miner--cms-20353?_ga=2.51452969.1697175413.1508571994-1473909186.1508571994

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

    ./minerd --url stratum+tcp://stratum.bitcoin.cz:3333 --userpass jamesleesaunders.ernie:woof --algo sha256d

Running with config (see: [cpuminer.conf](cpuminer.conf) example file):

    ./minerd --config ../cpuminer.conf

Tutorial:
* https://www.gadgetdaily.xyz/mine-bitcoins-with-raspberry-pi/

### Create Startup Service
Copy file init.d script (see: [mining](mining) example file) to /etc/init.d/ and set correct permissions:
    
    sudo cp ./mining /etc/init.d/
    sudo chmod 755 /etc/init.d/mining
    sudo chown root:root /etc/init.d/mining
    
Test it starts and stops:

    sudo /etc/init.d/mining start
    sudo /etc/init.d/mining stop
    
Set it to start automatically on boot with:    

    sudo update-rc.d mining defaults
    sudo update-rc.d mining enable
    
Reboot Rasbberr Pi:

    sudo reboot
    
Check mining service running:

    ps -A | grep mine
    268 ?        14:03:06 minerd
    269 pts/1    00:02:11 bfgminer

### Setup logrotate
Rotate logs to ensure we don't eat up space with log files:

    sudo vim /etc/logrotate.conf
    
Add:

    # system-specific logs may be configured here
    /home/pi/mining/bfgminer.log /home/pi/mining/cpuminer.log {
        missingok
        weekly
        rotate 4
        create 0644 root utmp
    }

Test with:

    sudo logrotate -f /etc/logrotate.conf
    
Reboot Rasbberr Pi:

    sudo reboot
