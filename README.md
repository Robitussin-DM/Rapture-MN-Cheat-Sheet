# Rapture-MN-Cheat-Sheet
Cheat Sheet for setting up a <a href="https://www.our-rapture.com">Rapture Masternode</a>.  This guide is intended as a cheat sheet/checklist for somewhat experienced users who have set up a masternode before and/or for those familiar with the <a href="https://docs.google.com/document/d/1kz6OuR1ybR_ZpvKEvJQybUdn_jVZVfviks7h-77_1Gs/edit#">official guide</a>.  

Alternatively, if I am helping you set up your masternode in Discord I may point you here.


## Prerequisite
Before you start anything, sign up for a Vultr account at: https://www.vultr.com/?ref=7606548

<a href="https://www.vultr.com/?ref=7606548"><img src="https://www.vultr.com/media/banner_1.png" width="728" height="90"></a>

After using the above link, use code **VULTRMATCH** to match dollar for dollar up to $100 of your initial funding.


## Local Wallet Actions

In your local Rapture wallet, go to Tools -> Debug Console

Type ``masternode genkey``

Note the output, we will call it ``MASTERNODEKEY`` in the next steps.  Replace ``MASTERNODEKEY`` with this value below (in 2 places).

Type ``getaccountaddress masternode1``

Copy the address, send exactly 1000 RAP to this address and wait for 15 confirmations

Type ``masternode outputs`` and note the ``LONGSTRING`` followed by a ``0`` or ``1``

Edit ``C:\Users\USERNAME\AppData\Roaming\RaptureCore\masternode.conf`` in your favorite text editor, adding the following line (editing the ``MASTERNODEKEY``, ``LONGSTRING``, and ``X.Y.Z.A`` and the ``0`` or ``1`` accordingly)

``masternode1 X.Y.Z.A:14777 MASTERNODEKEY LONGSTRING 0``



## VPS actions

Log in to your Vultr server and execute the following commands:

``sudo fallocate -l 1G /swapfile && chmod 600 /swapfile && mkswap /swapfile && swapon /swapfile && echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab && free -h``

``sudo apt-get update -y``

``sudo apt-get upgrade -y``

``sudo apt install -y nano git fail2ban``

``adduser rapturenode``

``usermod -aG sudo rapturenode``

``su rapturenode``

``cd``

``wget https://github.com/RaptureCore/Rapture/releases/download/v1.1.2.2/rapturecore-1.1.2-linux64.tar.gz``

``tar -xvf rapturecore-1.1.2-linux64.tar.gz``

``rm rapturecore-1.1.2-linux64.tar.gz``

``mkdir ~/.rapturecore``

``touch ~/.rapturecore/rapture.conf``

``echo "daemon=1" >> ~/.rapturecore/rapture.conf``

``echo "rpcuser=user"`shuf -i 100000-10000000 -n 1` >> ~/.rapturecore/rapture.conf``

``echo "rpcpassword=pass"`shuf -i 100000-10000000 -n 1` >> ~/.rapturecore/rapture.conf``

``echo "externalip="`curl -s4 api.ipify.org` >> ~/.rapturecore/rapture.conf``

``echo "masternode=1" >> ~/.rapturecore/rapture.conf ``

``echo "masternodeprivkey=MASTERNODEKEY" >> ~/.rapturecore/rapture.conf``

``echo "maxconnections=50" >> ~/.rapturecore/rapture.conf``

``~/rapturecore-1.1.2/bin/raptured``

``sudo apt-get -y install python-virtualenv virtualenv``

``git clone https://github.com/RaptureCore/sentinel.git``

``cd ~/sentinel``

``virtualenv ./venv``

``./venv/bin/pip install -r requirements.txt``

``./venv/bin/python bin/sentinel.py``

``crontab -l | { cat ; echo "* * * * * cd /home/rapturenode/sentinel && ./venv/bin/python bin/sentinel.py >/dev/null 2>&1"; } | crontab -``

``watch rapturecore-1.1.2/bin/rapture-cli getblockcount``

When the number matches the highest block on http://explorer.our-rapture.com/, go to the "Masternodes" tab on the local wallet and "Start Alias"

