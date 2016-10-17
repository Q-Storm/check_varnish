# check_varnish
Varnish performance plugin for Nagios
=====================================

This plugin can give you a better insight into how affectively varnish is running and tell you if its having problems

Install
-------
To use this plugin you need to have varnishstat installed which is installed by default when you install varnish.

Perl is also required for this plugin. If you don’t have Perl installed you can install in by running the command below

sudo apt-get install perl

or

sudo yum install perl

Now you can clone the repo above

git clone https://github.com/thomasweaver/check_varnish.git

Now you should have a file “check_varnish.pl” make sure that it has execute permissions:

chmod u+x check_varnish.pl

You now need to copy this file to your nagios plugins folder. You should consult your nagios config to find out where this is. Mine was ‘/usr/lib/nagios/plugin’

mv check_varnish.pl /usr/lib/nagios/plugins/.


Help
----
check_varnish.pl – Monitor and report on varnish usage

check_varnish.pl [-c|–cache] [-b|–bin ] [-d|–backend ] [-s|–stats ] [-t|–technique ] [-w|–warning ] [-c|–critical ] [-h|–help]

DESCRIPTION

This script will report on various varnish stats including: varnish cache hit ratio backend error count (Total or Ratio) Any other counter in varnishstat If no counters are required the script will ensure the varnish binary is running

OPTIONS

-a –cache – this will make the script output cache_hit ratio perfdata

-b –bin – to specify a different location of the default varnishstat binary location. Default is ‘/usr/bin/varnishstat’

-d –backend – specify script to output backend data you can output ratio, total or both

-h –help – output this message

-w –warning – specify the warning threshold. Required for cache and backend checks

-c –critical – specify the critical threshold. Required for cache and backend checks

-s –stats – specify a comma separated list of all the stats you wish to check Critical and Warning can be specified and all values will be compared to these values.

-t –technique – when specifying stats you can also specify what technique you wish to use to compare the values to the thresholds. specify lt for less than and gt for greater than. Default is gt

EXAMPLES

Check varnish is running

./check_varnish.pl

Check varnish Cache Hit Ratio and warn if ratio is below 0.8

./check_varnish.pl -a -w 0.8 -c 0.6

Check varnish Backends

./check_varnish.pl -d all

Check varnish client requests and drops

./check_varnish.pl -s client_drop,client_req

Nagios Set Up
-------------

Once you have run the command in the CLI and all is working you can add the command:

define command {

command_name check_varnish

command_line $USER1$/check_varnish.pl $ARG1$

register 1
}

More can be found at http://www.toms-blog.com/post/nagios-varnish-performance-plugin/