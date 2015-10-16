# ntpplot
## Plot NTP statistics and present them over HTTP!

### About
ntpplot levrages the robust statistics repoting built into NTP to plot various
graphhs regarding normal and abnormal operation.

### Who is this for?
Anyone running a production NTP server (whether in an internal environment such
as a datacenter or an office, or an external pulic facing environment such as
contribution to pool.ntp.org) will benefit from a little transperency.

### How does this work?
The concept is simple and strives to levrage existing components:  
- A configuration file that sets up statistics output for NTP is included with
`includefile` in `ntp.conf`.
- A [Gnuplot](http://www.gnuplot.info/) script that generates the plots is configured with cron to run
preidically.
- An Apache configuration file allows serving the plots folder and images over HTTP.

### Requirements:
- [NTP version 4.2.8 or higher](http://www.ntp.org/) (untested with lower NTP 4.x versions, unlikely to work with 3.x and lower due to [log format changes](http://www.ntp.org/ntpfaq/NTP-s-trouble.htm#TAB-STATFIL))
- [Gnuplot version 4.6 or higher](http://www.gnuplot.info/) (untested with lower Gnuplot versions)
- [Apache version 2.4](https://httpd.apache.org/docs/2.4/) (optional - needed so serve plots over HTTP, will not work with Apache 2.2 as is)

### Installation:
- Clone repo under `/opt`:
```
git clone https://github.com/erikkugel/ntpplot.git /opt/ntpplot
```
- Create a folder called `/var/log/ntp` owned by whichever user runs `ntp`:
```
mkdir -p -v /var/log/ntp
```
- Include NTP's statistics configuration in `/etc/ntp.conf`:
```
echo "includefile /opt/ntpplot/conf/ntp-stats.conf" >> /etc/ntp.conf
```
- Symlink cron job configuration under `/etc/cron.d`:
```
ln -v -s -f /opt/ntpplot/conf/cron-ntpplot /etc/cron.d/ntpplot
```
- Include Apache config in Apache 2.4:
```
echo "Include /opt/ntpplot/conf/httpd-ntpplot.conf" >> /etc/httpd/httpd.conf
```
- Reload NTP and Apache configuration.

### Screenshots:
![NTP Peer Versions](screenshots/sys_ntp_versions.png "NTP Peer Versions")
![NTP Packet Errors](screenshots/sys_packet_errors.png "NTP Peer Versions")

### Resources:
[NTP Troubleshooting Guidelines](http://www.ntp.org/ntpfaq/NTP-s-trouble.htm)  
[NTP Advanced Logging Configuration](https://www.novell.com/support/kb/doc.php?id=7009361)
