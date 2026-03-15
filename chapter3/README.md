# Chapter 3 Examples (Step by Step)

Run these commands from `chapter3/`.
The local `ansible.cfg` already points to `hosts.ini`.

## 1. Start the lab machines

```bash
vagrant up
```

## 2. Validate connectivity and hostnames

```bash
ansible multi -m ping
ansible multi -a "hostname"
ansible multi -a "hostname" -f 1
```

## 3. Run quick health checks

```bash
ansible multi -a "df -h"
ansible multi -a "free -m"
ansible multi -a "date"
```

## 4. Gather system facts

```bash
ansible multi -m setup
```

## 5. Configure NTP (chrony) on all servers

```bash
ansible multi -b -m dnf -a "name=chrony state=present"
ansible multi -b -m service -a "name=chronyd state=started enabled=yes"
ansible multi -b -a "chronyc tracking"
```

## 6. Configure application servers

```bash
ansible app -b -m dnf -a "name=python3-pip state=present"
ansible app -b -m pip -a "executable=pip3 name=django<4 state=present"
ansible app -a "python3 -m django --version"
```

## 7. Configure the database server

```bash
ansible db -b -m dnf -a "name=mariadb-server state=present"
ansible db -b -m service -a "name=mariadb state=started"
```

## 8. Configure DB firewall access

```bash
ansible db -b -m dnf -a "name=firewalld state=present"
ansible db -b -m service -a "name=firewalld state=started enabled=yes"
ansible db -b -m firewalld -a "zone=database state=present permanent=yes"
ansible db -b -m firewalld -a "service=ssh zone=database state=enabled permanent=yes"
ansible db -b -m firewalld -a "source=192.168.56.0/24 zone=database state=enabled permanent=yes"
ansible db -b -m firewalld -a "port=3306/tcp zone=database state=enabled permanent=yes"
```

## 9. Install DB Python driver and create app DB user

```bash
ansible db -b -m dnf -a "name=python3-PyMySQL state=present"
ansible db -b -m mysql_user -a "name=django host=% password=12345 priv=*.*:ALL state=present login_unix_socket=/var/lib/mysql/mysql.sock check_implicit_admin=yes"
```

## 10. Verify grants for DB user

```bash
ansible db -b -a "mysql -uroot -e \"SHOW GRANTS FOR 'django'@'%';\""
```

## 11. Create APP User and Admin Group

```bash
ansible app -b -m group -a "name=admin state=present"
ansible app -b -m user -a "name=johndoe group=admin createhome=yes"
```

## 12. Install GIT package

```bash
ansible app -b -m package -a "name=git state=present"
```

## 13. Perform operations on files and folders

```bash
ansible multi -m stat -a "path=/etc/environment"
ansible multi -m copy -a "src=/etc/hosts dest=/tmp/hosts"
ansible multi -b -m fetch -a "src=/etc/hosts dest=/tmp"
ansible multi -m file -a "dest=/tmp/test mode=644 state=directory"
```

### 14. Check log files

```bash
ansible multi -b -a "tail /var/log/messages"
ansible multi -b -m shell -a "tail /var/log/messages | grep ansible-command | wc -l"
```