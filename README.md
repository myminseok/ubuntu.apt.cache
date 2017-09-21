ubuntu-14.04.3-server-amd64.iso


split -b 100000000 ubuntu-apt-cache-big.tar.gz ubuntu-apt-cache-big.tar.gz.part



cat ubuntu-apt-cache-big.tar.gz.part* > ubuntu-apt-cache-big.tar.gz


