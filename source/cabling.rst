
Cabling scheme
--------------

Network cabling is realized connecting the machines to a single switch placed on top of the 
rack. 
The switch is a `DELL N1524 <http://www.dell.com/us/business/p/networking-n1500-series/pd>`_ 
and can be reached on the internal subnet for configuration via HTTP at address
`192.168.30.199 <http://192.168.30.199>`_ .

The switch defines four VLANs for traffic separation and services isolation:

+-----------+---------------+--------------------+--------------------+
| VLAN      |   NETWORK     |    USAGE           |     PORTS          |
+===========+===============+====================+====================+
| 1 default | 192.168.1.0   |  ESCS and service  | 1 2 3 4 5 6 7      | 
|           | 192.167.189.0 |  network           | 8 9 11 18 20 22 24 |
+-----------+---------------+--------------------+--------------------+
| 10 data   | 192.168.10.0  |  10Gbe network     | Te1 Te2 Te3 Te4    | 
+-----------+---------------+--------------------+--------------------+
| 51 antenna| 192.168.51.0  |  Antenna apparatus | 13 15 17 19 21 23  | 
+-----------+---------------+--------------------+--------------------+
| 52 roach  | 192.168.52.0  |  roach services    | 10 12 14 16        |
|           |               |  and control       |                    |
+-----------+---------------+--------------------+--------------------+
