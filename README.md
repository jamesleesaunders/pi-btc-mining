
Setup folder:
* mkdir mining
* cd mining

Install dependencies:
sudo apt-get install autoconf libtool uthash-dev libjansson-dev libevent-dev libcurl4-gnutls-dev libncurses5-dev

Download bfgminer source:
pi@raspberrypi:~/mining $ git clone https://github.com/luke-jr/bfgminer.git
Cloning into 'bfgminer'...
remote: Counting objects: 47615, done.
remote: Total 47615 (delta 0), reused 0 (delta 0), pack-reused 47615
Receiving objects: 100% (47615/47615), 35.68 MiB | 1.07 MiB/s, done.
Resolving deltas: 100% (30838/30838), done.

Install bfgminer:
pi@raspberrypi:~/mining $ cd bfgminer/
pi@raspberrypi:~/mining/bfgminer $ sudo ./autogen.sh
pi@raspberrypi:~/mining/bfgminer $ sudo ./configure
pi@raspberrypi:~/mining/bfgminer $ sudo make

Test bfgminer:
pi@raspberrypi:~/mining/$ ./bfgminer/bfgminer -o stratum.bitcoin.cz:3333 -u jamesleesaunders.bert -p woof -S all
pi@raspberrypi:~/mining $ ./bfgminer/bfgminer --config ./bfgminer.conf

Return back to parent mining directory:
pi@raspberrypi:~/mining/bfgminer $ cd ..

Download cpuminer source:
pi@raspberrypi:~/mining $ git clone https://github.com/pooler/cpuminer
Cloning into 'cpuminer'...
remote: Counting objects: 1475, done.
remote: Total 1475 (delta 0), reused 0 (delta 0), pack-reused 1475
Receiving objects: 100% (1475/1475), 592.51 KiB | 174.00 KiB/s, done.
Resolving deltas: 100% (959/959), done.

Install cpuminer:
pi@raspberrypi:~/mining $ cd cpuminer/
pi@raspberrypi:~/mining/cpuminer $ sudo ./autogen.sh  # only needed if building from git repo
pi@raspberrypi:~/mining/cpuminer $ sudo ./configure CFLAGS="-O3"  # make sure -O3 is an O not a zero!
pi@raspberrypi:~/mining/cpuminer $ sudo make

Run cpuminer:
pi@raspberrypi:~/mining $ ./cpuminer/minerd --url stratum+tcp://stratum.bitcoin.cz:3333 --userpass jamesleesaunders.ernie:woof
pi@raspberrypi:~/mining $ ./cpuminer/minerd --config ./cpuminer.conf


bfgminer.conf
-------------
{
	"pools": [{
		"url": "http://stratum.bitcoin.cz:3333",
		"user": "jamesleesaunders.bert",
		"pass": "woof",
		"pool-priority": "0"
	}],
	"scan": "all"
}


cpuminer.conf
-------------
{
	"url": "stratum+tcp://stratum.bitcoin.cz:3333",
	"user": "jamesleesaunders.ernie",
	"pass": "woof",
	"algo": "sha256d"
}


bfgminer tutorial:
	https://computers.tutsplus.com/tutorials/how-to-create-a-raspberry-pi-bitcoin-miner--cms-20353?_ga=2.51452969.1697175413.1508571994-1473909186.1508571994

cpuminer tutorial:
	https://www.gadgetdaily.xyz/mine-bitcoins-with-raspberry-pi/


