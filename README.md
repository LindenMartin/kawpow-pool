# NOMP KawPoW Algorithm Pool
Mining pool for Kawpow algorithm.

-------
### Node Open Mining Portal consists of 3 main modules:
| Project | Link |
| ------------- | ------------- |
| [Kawpow pool](https://github.com/LindenMartin/kawpow-pool) | https://github.com/LindenMartin/kawpow-pool|
| [Kawpow Stratum Pool](https://github.com/LindenMartin/kawpow-stratum-pool) | https://github.com/LindenMartin/kawpow-stratum-pool |
| [Node multi hashing](https://github.com/LindenMartin/node-multi-hashing) | https://github.com/LindenMartin/node-multi-hashing |

-------
### Requirements
***NOTE:*** _These requirements will be installed in the install section!_<br />
* Ubuntu Server 20.04.* LTS
* Coin daemon
* Node Version Manager
* Node 8.1.4
* Process Manager 2 / pm2
* Redis Server
* ntp

-------

### Install RavenCoin Daemon

    adduser pool
    usermod -aG sudo pool
    su - pool
    wget https://github.com/RavenProject/Ravencoin/releases/download/v4.3.2.1/raven-4.3.2.1-x86_64-linux-gnu.zip
    unzip raven-4.3.2.1-x86_64-linux-gnu.zip
    rm raven*zip
    cd raven-4.3.2.1/bin
    mkdir -p ~/.raven/
    touch ~/.raven/raven.conf
    echo "rpcuser=user1" > ~/.raven/raven.conf
    echo "rpcpassword=pass1" >> ~/.raven/raven.conf
    echo "prune=550" >> ~/.raven/raven.conf
    echo "daemon=1" >> ~/.raven/raven.conf
    ./ravend
    ./raven-cli getnewaddress

Example output: RNxxxjk - it is the address of your pool, you need to remember it and specify it in the configuration file pool_configs/ravencoin.json.
    
    ./raven-cli getaddressesbyaccount ""
    
Information about pool wallet address.
    
    ./raven-cli getwalletinfo
    
Get more information.

    ./raven-cli getblockcount
    
Information about synchronization of blocks in the main chain.

    ./raven-cli help
Other helpfull commands.

-------

### Install Pool

    sudo apt install git -y
    cd ~
    git config --global http.https://gopkg.in.followRedirects true
    git clone https://github.com/LindenMartin/kawpow-pool.git
    cd kawpow-pool/
    ./install.sh

-------
### Configure Pool

    cd ~/kawpow-pool
    nano config.json

```javascript
{
    
    "poolname": "KawPoW Coins Pool",
    
    "devmode": false,
    "devmodePayMinimim": 0.25,
    "devmodePayInterval": 120,
    
    "logips": true,       
    "anonymizeips": true,
    "ipv4bits": 16,
    "ipv6bits": 16,
    
     "defaultCoin": "ravencoin",
    
    "poollogo": "https://zelcash.github.io/zeltrez.io/images/logos/Ravencoin.svg",
    
    "discordtwitterfacebook": "Join our #mining channel on Discord: <a href='https://discord.gg/vzcbVNW' target='_blank'>https://discord.gg/vzcbVNW</a>",
    
    "pagetitle": "KawPoW Algorithm Pool - 0.5% Fees Promo - Run by professionals",
    "pageauthor": "My Name Is...",
    "pagedesc": "A reliable, low fee, easy to use mining pool for KawPoW algorithm coins! Get started mining today!",
    "pagekeywds": "GPU,CPU,Hash,Hashrate,Cryptocurrency,Crypto,Mining,Pool,Ravencoin,Zelantus,Sato,Easy,Simple,How,To",

    "btcdonations": "BTCaddress",
    "ltcdonations": "LTCaddress",
    "rvndonations": "RVNaddress",

    "logger" : {
        "level" : "debug",
        "file" : "/home/pool/nomp-kawpow-pool/logs/nomp_debug.log"
    },

    "cliHost": "127.0.0.1",
    "cliPort": 17117,

    "clustering": {
        "enabled": false,
        "forks": "auto"
    },

    "defaultPoolConfigs": {
        "blockRefreshInterval": 400,
        "jobRebroadcastTimeout": 30,
        "connectionTimeout": 600,
        "emitInvalidBlockHashes": false,
        "validateWorkerUsername": true,
        "tcpProxyProtocol": false,
        "banning": {
            "enabled": true,
            "time": 600,
            "invalidPercent": 50,
            "checkThreshold": 500,
            "purgeInterval": 300
        },
        "redis": {
            "host": "127.0.0.1",
            "port": 6379
        }
    },

    "website": {
        "enabled": true,
        "sslenabled": false,
        "sslforced": false,
        "host": "0.0.0.0",
        "port": 80,
        "sslport": 443,
        "sslkey": "/home/pool/nomp-kawpow-pool/certs/privkey.pem",
        "sslcert": "/home/pool/nomp-kawpow-pool/certs/fullchain.pem",
        "stratumHost": "127.0.0.1",
        "stats": {
            "updateInterval": 15,
            "historicalRetention": 28800,
            "hashrateWindow": 600
        },
        "adminCenter": {
            "enabled": false,
            "password": "NOT_WORKING_YET"
        }
    },

    "redis": {
        "host": "127.0.0.1",
        "port": 6379
    },

    "switching": {
        "switch1": {
            "enabled": false,
            "algorithm": "sha256",
            "ports": {
                "3333": {
                    "diff": 10,
                    "varDiff": {
                        "minDiff": 16,
                        "maxDiff": 512,
                        "targetTime": 15,
                        "retargetTime": 90,
                        "variancePercent": 30
                    }
                }
            }
        },
        "switch2": {
            "enabled": false,
            "algorithm": "scrypt",
            "ports": {
                "4444": {
                    "diff": 10,
                    "varDiff": {
                        "minDiff": 16,
                        "maxDiff": 512,
                        "targetTime": 15,
                        "retargetTime": 90,
                        "variancePercent": 30
                    }
                }
            }
        },
        "switch3": {
            "enabled": false,
            "algorithm": "x11",
            "ports": {
                "5555": {
                    "diff": 0.001,
                    "varDiff": {
                        "minDiff": 0.001,
                        "maxDiff": 1, 
                        "targetTime": 15, 
                        "retargetTime": 60, 
                        "variancePercent": 30 
                    }
                }
            }
        }
    },

    "profitSwitch": {
        "enabled": false,
        "updateInterval": 600,
        "depth": 0.90,
        "usePoloniex": true,
        "useCryptsy": true,
        "useMintpal": true,
        "useBittrex": true
    }

}

```

Change "address": "RNxxxg", to your pool created wallet address in file ravencoin.json:

    cd ~/nomp-kawpow-pool/pool_configs
    nano ravencoin.json

```javascript
{
    "enabled": true,
    "coin": "ravencoin.json",

    "address": "RNxxxg",
    
    "donateaddress": "RNxxxg",

    "rewardRecipients": {
        "RNxxxg": 0.5
    },

    "paymentProcessing": {
        "enabled": true,
        "schema": "PROP",
        "paymentInterval": 300,
        "minimumPayment": 1,
        "maxBlocksPerPayment": 10,
        "minConf": 30,
        "coinPrecision": 8,
        "daemon": {
            "host": "127.0.0.1",
            "port": 8766,
            "user": "user1",
            "password": "pass1"
        }
    },

    "ports": {
	"10008": {
            "diff": 0.05,
    	    "varDiff": {
    	        "minDiff": 0.025,
    	        "maxDiff": 1024,
    	        "targetTime": 10,
    	        "retargetTime": 60,
    	        "variancePercent": 30,
    		"maxJump": 25
    	    }
        },
        "10016": {
	    "diff": 0.10,
            "varDiff": {
                "minDiff": 0.05,
                "maxDiff": 1024,
    	        "targetTime": 10,
    	        "retargetTime": 60,
    	        "variancePercent": 30,
		"maxJump": 25
            }
        },
        "10032": {
	    "diff": 0.20,
            "varDiff": {
    		"minDiff": 0.10,
    		"maxDiff": 1024,
    	        "targetTime": 10,
    	        "retargetTime": 60,
    	        "variancePercent": 30,
    		"maxJump": 50
    	    }
        },
	"10256": {
	    "diff": 1024000000,
            "varDiff": {
                "minDiff": 1024000000,
                "maxDiff": 20480000000,
    	        "targetTime": 10,
    	        "retargetTime": 60,
    	        "variancePercent": 30,
		"maxJump": 25
            }
        }
    },

    "daemons": [
        {
            "host": "127.0.0.1",
            "port": 8766,
            "user": "user1",
            "password": "pass1"
        }
    ],

    "p2p": {
        "enabled": false,
        "host": "127.0.0.1",
        "port": 8767,
        "disableTransactions": true
    },

    "mposMode": {
        "enabled": false,
        "host": "127.0.0.1",
        "port": 3306,
        "user": "me",
        "password": "mypass",
        "database": "rvn",
        "checkPassword": true,
        "autoCreateWorker": false
    },

    "mongoMode": {
        "enabled": false,
        "host": "127.0.0.1",
        "user": "",
        "pass": "",
        "database": "rvn",
        "authMechanism": "DEFAULT"
    }

}

```

### Run Pool
    
    cd ~/nomp-kawpow-pool
    ./pool-start.sh
    
-------
