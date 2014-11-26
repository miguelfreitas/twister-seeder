Twister-seeder
==============

Twister-seeder is a crawler for the [Twister](http://twister.net.co) 
([github](https://github.com/miguelfreitas/twister-core)) network,
which exposes a list of reliable nodes via a built-in DNS server.

Code based on [Bitcoin-seeder](https://github.com/sipa/bitcoin-seeder).

Features:
* regularly revisits known nodes to check their availability
* bans nodes after enough failures, or bad behaviour
* accepts nodes down to v0.3.19 to request new IP addresses from,
  but only reports good post-v0.3.24 nodes.
* keeps statistics over (exponential) windows of 2 hours, 8 hours,
  1 day and 1 week, to base decisions on.
* very low memory (a few tens of megabytes) and cpu requirements.
* crawlers run in parallel (by default 96 threads simultaneously).

USAGE
-----

Using of it is [highly appreciated](http://twister.net.co/?p=410). If you
have a 24×7 machine and you are able to add an special NS record to your domain,
please consider running twister-seeder. Then let @miguelfreitas know
and he will add your domain to the code base.

Assuming you want to run a dns seed on dnsseed.example.com, you will
need an authorative NS record in example.com's domain record, pointing
to for example vps.example.com:

    dig -t NS dnsseed.example.com

As answer you should get something like this:

> dnsseed.example.com.   86400    IN      NS     vps.example.com.

On the system vps.example.com, you can now run dnsseed:

    ./dnsseed -h dnsseed.example.com -n vps.example.com

If you want the DNS server to report SOA records, please provide an
e-mailadres (with the @ part replaced by .) using `-m`.

RUNNING AS NON-ROOT
-------------------

Typically, you'll need root privileges to listen to port 53 (name service).

One solution is using an iptables rule (Linux only) to redirect it to
a non-privileged port:

    iptables -t nat -A PREROUTING -p udp --dport 53 -j REDIRECT --to-port 5353

If properly configured, this will allow you to run dnsseed in userspace, using
the -p 5353 option.
