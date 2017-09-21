split -b 100000000 ubuntu-apt-cache-big.tar.gz ubuntu-apt-cache-big.tar.gz.part



cat ubuntu-apt-cache-big.tar.gz.part* > ubuntu-apt-cache-big.tar.gz
