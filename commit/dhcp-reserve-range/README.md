#dhcp-reserve-range.slax

Junos routers do not support a range to be excluded from the dynamic dhcp pool in 
addition static-bindings must be inside the dynamic range for a pool.  This means
you cannot reserve a section of the dhcp range to be used only for static-bindings.
For example you may want 172.16.1.32-47 to be reserved for static bindings only and
have static bindings start at 48.

This script mimicks the desired behavior by doing tranient change on commit that 
has fake static-bidings to "reserve" the range for future static-bindings.  If
a static-binding exists for an IP address in the range a fake binding is not created.

##Changelog 
 
v1.0:
* Initial Release

##Example

Configuration:

    bbennett@SRX210> show configuration system services dhcp    
    domain-name lan;
    name-server {
        172.16.1.10;
        8.8.8.8;
        8.8.4.4;
    }
    domain-search {
        lan;
    }
    /* Trust */
    pool 172.16.1.0/24 {
        apply-macro reserved-range {
            high 172.16.1.47;
            low 172.16.1.32;
        }
        address-range low 172.16.1.32 high 172.16.1.254;
        router {
            172.16.1.1;
        }
    }
    static-binding c8:bc:c8:a3:38:9d {
        fixed-address {
            172.16.1.32;
        }
        host-name thecheat;
    }
    static-binding 00:04:f2:2d:e2:c5 {
        fixed-address {
            172.16.1.34;
        }
        host-name polycom-phone;
    }
    static-binding 00:04:a3:03:9e:79 {
        fixed-address {
            172.16.1.33;
        }                                   
        host-name sprinkler;
    }
    propagate-settings fe-0/0/7.0;


Result:

    bbennett@SRX210> show system services dhcp binding  
    IP address       Hardware address   Type     Lease expires at
    172.16.1.33      00:04:a3:03:9e:79  static   never               
    172.16.1.34      00:04:f2:2d:e2:c5  static   never               
    172.16.1.64      00:0c:29:ac:1e:f9  dynamic  2012-11-06 16:46:58 UTC
    172.16.1.62      00:0c:29:f1:30:fd  dynamic  2012-11-06 16:11:13 UTC
    172.16.1.57      00:18:dd:01:8e:43  dynamic  2012-11-06 15:18:08 UTC
    172.16.1.61      00:24:6c:cd:57:48  dynamic  2012-11-06 10:42:33 UTC
    172.16.1.55      18:b4:30:03:c4:d7  dynamic  2012-11-05 18:03:43 UTC
    172.16.1.49      20:c9:d0:89:e6:27  dynamic  2012-11-06 06:51:57 UTC
    172.16.1.51      28:c0:da:e1:f9:80  dynamic  2012-11-06 05:59:40 UTC
    172.16.1.53      30:85:a9:4b:24:c9  dynamic  2012-11-06 17:31:23 UTC
    172.16.1.63      38:e7:d8:00:c2:60  dynamic  2012-11-06 15:25:17 UTC
    172.16.1.56      50:cc:f8:dd:24:9d  dynamic  2012-11-06 15:01:16 UTC
    172.16.1.59      58:67:1a:45:06:69  dynamic  2012-11-06 03:39:40 UTC
    172.16.1.54      74:f0:6d:32:56:c1  dynamic  2012-11-06 11:04:06 UTC
    172.16.1.50      90:2b:34:31:2b:46  dynamic  2012-11-06 05:59:02 UTC
    172.16.1.65      9c:8e:99:8a:97:6a  dynamic  2012-11-06 05:57:00 UTC
    172.16.1.35      be:ef:ac:10:01:23  static   never               
    172.16.1.36      be:ef:ac:10:01:24  static   never               
    172.16.1.37      be:ef:ac:10:01:25  static   never               
    172.16.1.38      be:ef:ac:10:01:26  static   never               
    172.16.1.39      be:ef:ac:10:01:27  static   never               
    172.16.1.40      be:ef:ac:10:01:28  static   never               
    172.16.1.41      be:ef:ac:10:01:29  static   never               
    172.16.1.42      be:ef:ac:10:01:2a  static   never               
    172.16.1.43      be:ef:ac:10:01:2b  static   never               
    172.16.1.44      be:ef:ac:10:01:2c  static   never               
    172.16.1.45      be:ef:ac:10:01:2d  static   never               
    172.16.1.46      be:ef:ac:10:01:2e  static   never               
    172.16.1.47      be:ef:ac:10:01:2f  static   never               
    172.16.1.32      c8:bc:c8:a3:38:9d  static   never               
    172.16.1.60      cc:6d:a0:39:ca:1c  dynamic  2012-11-05 21:42:53 UTC
    172.16.1.52      e0:91:f5:9c:7e:3e  dynamic  2012-11-06 06:00:23 UTC
    172.16.1.58      fc:c7:34:dc:f9:77  dynamic  2012-11-06 16:59:34 UTC