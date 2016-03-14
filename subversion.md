###Subversion

####Install

#####CentOS 7

add repo: create file */etc/yum.repo.d/wandisco.repo*, and add the following code in it
```
[WandiscoSVN]
name=Wandisco SVN Repo
baseurl=http://opensource.wandisco.com/centos/7/svn-1.9/RPMS/$basearch/
enabled=1
gpgcheck=0
```

issue this command to install subversion server:
```
yum install epel-release
yum clean all
yum makecache
yum install subversion
```