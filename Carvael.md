##Caravel

###Pre-requirements

####OS Dependencies

```
yum install zlib zlib-devel openssl openssl-devel libffi libffi-devel gcc gcc-c++ sqlite sqlite-devel
```

####Python2.7

```
tar axf Python-2.7.11.tar
cd Python-2.7.11
./configure
make
make install
```

###Play with Caravel

```
# create an admin user
fabmanager create-admin --app caravel

# Initialize the database
caravel db upgrade

# Create default roles and permissions
caravel init

# Load some data to play with
caravel load_examples

# Start the development web server
caravel runserver -d
```