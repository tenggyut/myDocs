## ES

###OS Config

```
echo "vm.max_map_count = 262144" >> /etc/sysctl.conf 
echo "idcube soft nproc 65536" >> /etc/security/limits.conf
echo "idcube hard nproc 65536" >> /etc/security/limits.conf
echo "idcube soft nofile 65536" >> /etc/security/limits.conf
echo "idcube hard nofile 65536" >> /etc/security/limits.conf
echo "idcube soft memlock unlimited" >> /etc/security/limits.conf
echo "idcube hard memlock unlimited" >> /etc/security/limits.conf
```