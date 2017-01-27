# dnsSuffixSpoofingManInTheMiddleRootZoneSomething

[The story](http://blog.5th.ch/dnssuffixspoofingmaninthemiddlerootzonesomething) behind this. 

The commands to run those docker containers.
<pre>

# setup new ip adresses on host
sudo ifconfig en0 alias 172.16.42.2/32 up
sudo ifconfig en0 alias 172.16.42.3/32 up

docker build -t evil-dns evil-dns
docker build -t httpd-com httpd_razw3hgtpmkesdh.com
docker build -t httpd-org httpd_razw3hgtpmkesdh.org

docker run --name evil-dns -d -p 53:53/udp -p 53:53 evil-dns --hostsfile hostsfile.txt
docker run --name httpd-com -d -p 172.16.42.2:80:8080 httpd-com
docker run --name httpd-org -d -p 172.16.42.3:80:8080 httpd-org

# add DNS sufix and point to docker DNS server
echo -e "search razw3hgtpmkesdh.com\nnameserver 127.0.0.1" > /etc/resolv.conf

</pre>

Now point your Google Chrome to [http://razw3hgtpmkesdh.org](http://razw3hgtpmkesdh.org) and the content of `http://razw3hgtpmkesdh.com` should be displayed.
