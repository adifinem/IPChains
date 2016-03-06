NOTICE: This module is old. _Really old_. So is ipchains in 
general. I suppose if you have a system old enough to have 
ipchains (and not iptables or better) then it might work 
there and be useful. Use at your own discretion, if at all!

-----------------------------------------------------------------

Copyright (c) 2016 Jessica Hutchison (Formerly Jessica Quaintance). 
All rights reserved. This package is free software; you can 
redistribute it and/or modify it under the same terms as Perl itself 
(see LICENSE), with the exception of the libipfwc.c, ipchains.c, 
and the files in include/ which  have separate terms derived from 
those of the original ipchains sources (see LICENSE.ipchains). 
Please see README.ipchains for the README that was included with the
original source code for ipchains and contains copyrights and
credits for such.

For the full original source code, documentation and other 
information related to the ipchains project please see 
http://www.rustcorp.com/linux/ipchains/. The Ipfw.pm module is 
a seperate entity from such, so please do not bother the author 
of the ipchains source with issues related to this module.

For the most current version of this module please see
http://www.splatlabs.com/ipchains.html (Out of order). For more 
information about the module, or to submit questions, comments, 
or bug reports about such, please email j@splatlabs.com.

-----------------------------------------------------------------

To build this module, run the following commands:

    perl Makefile.PL
    make
    make install

To use this module you'll need to have several features enabled
in your kernel. From the ipchains README:

--
This module will not (yet) work with the 2.3.x or 2.4.x kernel series,
and there is no support planned as iptables is the norm for those.

If your kernel version is before 2.1.102; you need the ipfwchains
patch to the 2.1.x or 2.0.x kernel series.

You need to be running a kernel compiled with CONFIG_IP_FIREWALL (for
2.1.102 and above) or CONFIG_IP_FIREWALL_CHAINS set to `Y'.  You can
tell that your current kernel is compiled with this by looking for
/proc/net/ip_fwchains' - if it exists, then your kernel is ready for
the ipchains utility.

--

To use features such as IP Masquerading, IP Forwarding, IP Aliasing, 
etc., you will have to also enable these individually in your
kernel configuration. 

-----------------------------------------------------------------

The IPChains.pm module was created to act as an interface to the
Linux IP firewalling routines so that one might set, list, or 
otherwize modify and/or access Linux firewalling rules and
information from perl, and to do so relatively painlessly.

As an example, if you wanted to deny all traffic from 
192.168.100.0/24 going to 192.168.1.0/24, and log all attempts,
you could create a rule as such:

$fw = IPChains->new(Source 		=> 	"192.168.100.0",
   		    SourceMask 	 	=>	"24",
		    Dest		=>	"192.168.1.0",
		    DestMask		=>	"24",
		    Rule		=>	"DENY",
		    Log			=>	"1"
	 	    );

$fw->append("input");

(Note that "input" is the chain you're applying the rule to., and
masks can also be written in the 255.255.255.0 notation).
Then, to list the results you could clear the options, set
the Verbose option, then call the list() method:

$fw->clopts();
$fw->attribute(Verbose, 1);
$fw->list("input");
		
Available options, attributes and syntax are listed in the POD
documentation contained in IPChains.pm. 

TODO:

1) Add a package to manage the logs generated by ipchains if the
   logging option is specified.
2) Possibly add more methods for searching through set rules and
   selecting/manipulating rules by search criteria.

BUGS:

No -S support quite yet. RSN.

! (invert rule) doesn't seem to work. Will be fixed RSN too.


CONTRIBUTIONS:

Thanks for bug fixes/patches/updates/additions or all of the above to:

Heather Adkins <hadkins@excitecorp.com> - early troubleshooting/bug fix
Sven Koch <haegar@comunit.net> - bug fixes and list_chains() function
Francis J. Lacoste <francis.lacoste@iNsu.COM> - bug fixes

