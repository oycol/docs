RRDCACHE Service

```shell
/usr/bin/rrdcached -w 300 -p /run/rrdcache.pid -g 
```



```shell
更改hostname
basedir
plugindir

yum install collectd-rrd*
yum install collectd-perl
yum install perl-Collectd

#Hostname    "localhost"
#FQDNLookup   true
#BaseDir     "/var/lib/collectd"
#PIDFile     "/var/run/collectd.pid"
#PluginDir   "/usr/lib64/collectd"
#TypesDB     "/usr/share/collectd/types.db"
```

```shell
root@zstor[~]# rrdtool xport  
RRDtool 1.7.2  Copyright by Tobias Oetiker <tobi@oetiker.ch>
               Compiled Jul 22 2020 15:58:31

Usage: rrdtool [options] command command_options
 
* xport - generate XML dump from one or several RRD

    rrdtool xport [-s|--start seconds] [-e|--end seconds]
    	[-m|--maxrows rows]
    	[--step seconds]
    	[--enumds] [--json]
    	[-d|--daemon address]
    	[DEF:vname=rrd:ds-name:CF]
    	[CDEF:vname=rpn-expression]
    	[XPORT:vname:legend]
```

```shell
CDEF:total=idle,nice,user,system,interrupt,+,+,+,+ \
CDEF:idle_p=idle,total,/,100,* \
CDEF:nice_p=nice,total,/,100,* \
CDEF:user_p=user,total,/,100,* \
CDEF:system_p=system,total,/,100,* \
CDEF:interrupt_p=interrupt,total,/,100,* \

rrdtool  	\
xport  \
--daemon unix:/var/run/rrdcached.sock  \
--json  \
--end     now \
DEF:idle=/var/lib/collectd/rrd/ufs.local/aggregation-cpu-average/cpu-idle.rrd:value:AVERAGE \
DEF:nice=/var/lib/collectd/rrd/ufs.local/aggregation-cpu-average/cpu-nice.rrd:value:AVERAGE   \
DEF:user=/var/lib/collectd/rrd/ufs.local/aggregation-cpu-average/cpu-user.rrd:value:AVERAGE  \
DEF:system=/var/lib/collectd/rrd/ufs.local/aggregation-cpu-average/cpu-system.rrd:value:AVERAGE  \
DEF:interrupt=/var/lib/collectd/rrd/ufs.local/aggregation-cpu-average/cpu-interrupt.rrd:value:AVERAGE  \
XPORT:interrupt:interrupt \
XPORT:system:system \
XPORT:user:user \
XPORT:nice:nice \
XPORT:idle:idle 


rrdtool  	\
xport  \
--daemon unix:/tmp/rrdcached.sock  \
--json  \
--end   now

```



## Interface

```shell
rrdtool  \
xport  \
--daemon unix:/var/run/rrdcached.sock  \
--json  \
--end   now \
DEF:if_octets_rx=/var/lib/collectd/rrd/ufs.local/interface-eno2/if_octets.rrd:rx:AVERAGE \
DEF:if_octets_tx=/var/lib/collectd/rrd/ufs.local/interface-eno2/if_octets.rrd:tx:AVERAGE \
CDEF:cif_octets_rx=if_octets_rx,8,* \
CDEF:cif_octets_tx=if_octets_tx,8,* \
XPORT:cif_octets_rx:if_octets_rx \
XPORT:cif_octets_tx:if_octets_tx \
CDEF:overlap=cif_octets_rx,cif_octets_tx,+,8,* \
XPORT:overlap:overlap
```

### DISK

```shell
rrdtool  \
xport  \
--daemon unix:/var/run/rrdcached.sock  \
--json  \
--end   now \
DEF:disk_octets_read=/var/lib/collectd/rrd/ufs.local/disk-sdc/disk_octets.rrd:read:AVERAGE \
DEF:disk_octets_write=/var/lib/collectd/rrd/ufs.local/disk-sdb/disk_octets.rrd:write:AVERAGE \
XPORT:disk_octets_read:disk_octets_read \
XPORT:disk_octets_write:disk_octets_write 
```