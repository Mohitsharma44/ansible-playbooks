## What's in this setup?

3 VMS

- 2 x Application Servers
- 1 x Database ServerA

Ad-hoc commands using ansible for setting these up:
### On Group: app

#### Install Python-MySQL
`ansible app -s -m apt -a "name=python-mysqldb state=present" -i ./hosts`

#### Install python-pip
`ansible app -s -m apt -a "name=python-pip state=present" -i ./hosts`

#### Install django
`ansible app -s -m pip -a "name=django" -i ./hosts`

> Verify django is installed and working correctly
> `ansible app -a "python -c 'import django; print django.get_version()'" -i ./hosts`

### On Group: db

#### Install MariaDB
`ansible db -s -m apt -a "name=mariadb-server state=present" -i ./hosts`

#### Start and Enable MySQL
ansible db -s -m service -a "name=mysql state=started enabled=yes" -i ./hosts

#### Setup iptable rules

- **Flush rules**
`ansible db -s -a "iptables -F" -i ./hosts`

- **Accept tcp connection from 192.168.60.0/24 subnet to MariaDB**
`ansible db -s -a "iptables -A INPUT -s 192.168.60.0/24 -p tcp -m tcp --dport 3306 -j ACCEPT" -i ./hosts`

#### InstallPython-MySQL on db
`ansible db -s -m apt -a "name=python-mysqldb state=present" -i ./hosts`

#### Setup DB for user django
`ansible db -s -m mysql_user -a "name=django host=% password=12345 priv=*.*:ALL state=present"` -i ./hosts
