# gazelle2
Bittorrent tracker

INSTALLATION NOTES
1. Set up MySQL and memcached. We run memcached with the command:
	memcached -d -m 5120 -s /var/run/memcached.sock -a 0777 -t16 -C -u root
	This gives it 5 gigs of RAM, you probably want to set that a bit lower!
2. Run gazelle.sql (preferably as root) to create the database, the table, and the default data.
3. Install sphinx - we recommend you use the included sphinx.conf
	For documentation, read http://www.sphinxsearch.com/docs/current.html
	
	After you've installed, create the indices:
	/usr/local/bin/indexer -c /etc/sphinx/sphinx.conf --all
	
4. Move classes/config.template to classes/config.php. Edit the config.php as needed. 
	We use http://grc.com/passwords.html for our passwords - you'll be generating a lot of these.
5. Sign up. The first user is made a sysop!
6. Set up cron jobs. You need a cron job for the schedule, a cron job for 
the peerupdate (all groups are cached, but the peer counts change often, 
so peerupdate is a script to update them), and the two sphinx indices. 
These are our cron jobs:

0,15,30,45 *    *       *       *       /usr/local/bin/php /var/www/vhosts/what/schedule.php SCHEDULE_KEY >> /root/schedule.log
10,25,40,55 *  *        *       *       /usr/local/bin/php /var/www/vhosts/what/peerupdate.php SCHEDULE_KEY >> /root/peerupdate.log
*       *       *       *       *       /usr/local/bin/indexer -c /etc/sphinx/sphinx.conf --rotate delta
*       
5       0,12    *       *       *       /usr/local/bin/indexer -c /etc/sphinx/sphinx.conf --rotate --all


CHANGELOG
2010-04-07
-Fix critical bug where users can view staff forum posts by changing the ID on the reports page

2010-03-18
-Clear notifications per torrent or filter

2010-03-16
-Notifications groups actually work now
-Post history and subscriptions pages now default to unread posts with collapsed post bodies

2010-03-14
-Group notifications by filter

2010-03-13
-Added ability to view a user's downloaded torrents as well as snatched

2010-03-12
-Thread subscriptions
-Various bugfixes, see resolved gazelle bug forum
-Standardised Email and Image regexes across gazelle

2010-03-10
-Completed requestsv2, feel free to use it now

2010-03-09
-Add size and files column to notifications page and clone the browse layout
-Don't redirect if ssl url == nonssl url
-Fix some more warnings when calling sphinxapi.php
-Year filter in notifications also checks remaster year

2010-03-07
-Change INSERT INTO to REPLACE INTO to avoid errors when updating the sphinx*_delta tables

2010-03-04
-Added initial version of requestsv2, will need more updates so not advised to
update yet

2010-03-02
-Fixed bug in notifications by tags

2010-03-01
-Fixed bug in notifications by release type

2010-02-28
-Fixed bug which causes stats to be altered if the tracker updates while someone with stat editing powers moderates a profile
-Fixed artist permission
-Fixed two permission bugs
-Removed references to What.CD in takemoderate.php
-Fixed E_NOTICE with regards to taglist on browse2.php
-Removed geodistribution from stats, fixed stats so they don't whitepage
-Fixed the user geodistribution stats and geoip database updater, and added a function for an unsigned ip2long
-Kill poll manager, it doesn't work anymore - use the forums
	-Fix width of poll replies, don't display poll if there aren't any
-Re-add reports folder
-Fixed "database schema" tool
-Fix upscale pool blank message
-Fix number of posts in a forum after a thread has been moved out of it
-Strip out SVN revision echo

