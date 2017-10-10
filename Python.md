##Python

###Install

####Pre-requirements

```
yum install zlib zlib-devel openssl openssl-devel bzip2-devel
```

####Compile

```
wget https://www.python.org/ftp/python/2.7.12/Python-2.7.12.tar.xz
tar axf Python-2.7.12.tar.xz
cd Python-2.7.12
./configure --enable-shared
```

####Install Pip

```
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
```

###API

file open

```
The argument mode points to a string beginning with one of the following
 sequences (Additional characters may follow these sequences.):

 'r'   Open text file for reading.  The stream is positioned at the
         beginning of the file.

 'r+'  Open for reading and writing.  The stream is positioned at the
         beginning of the file.

 'w   Truncate file to zero length or create text file for writing.
         The stream is positioned at the beginning of the file.

 'w+  Open for reading and writing.  The file is created if it does not
         exist, otherwise it is truncated.  The stream is positioned at
         the beginning of the file.

 'a   Open for writing.  The file is created if it does not exist.  The
         stream is positioned at the end of the file.  Subsequent writes
         to the file will always end up at the then current end of file,
         irrespective of any intervening fseek(3) or similar.

 'a+  Open for reading and writing.  The file is created if it does not
         exist.  The stream is positioned at the end of the file.  Subse-
         quent writes to the file will always end up at the then current
         end of file, irrespective of any intervening fseek(3) or similar.
```

####Write utf-8 file

```
import codecs

file = codecs.open("lol", "w", "utf-8")
file.write(u'\ufeff')
file.close()
```

####Install lxml

```
yum install libxml2 libxml2-devel libxslt libxslt-devel
```

####Use Regex to Extract Text

```
import re
text = 'xxx'
found = re.search('AAA(.+?)ZZZ', text).group(1)
```

####crack email protection

```
from bs4 import BeautifulSoup

def decode(cfemail):
    enc = bytearray.fromhex(cfemail)
    return bytearray([c ^ enc[0] for c in enc[1:]]).decode('utf8')

def deobfuscate_cf_email(soup):
    for encrypted_email in soup.select('a.__cf_email__'):
        decrypted = decode(encrypted_email['data-cfemail'])
        
        # remove the <script> tag from the tree
        script_tag = encrypted_email.find_next_sibling('script')
        script_tag.decompose()
        # replace the <a class="__cf_email__"> tag with the decoded result
        encrypted_email.replace_with(decrypted)

soup = BeautifulSoup('''
     <li class="li3"><a class="__cf_email__" href="/cdn-cgi/l/email-protection" data-cfemail="af9e969f9a979d999b9c99ef9e999c81ccc0c2">[email&#160;protected]</a><script data-cfhash='f9e31' type="text/javascript">/* <![CDATA[ */!function(t,e,r,n,c,a,p){try{t=document.currentScript||function(){for(t=document.getElementsByTagName('script'),e=t.length;e--;)if(t[e].getAttribute('data-cfhash'))return t[e]}();if(t&&(c=t.previousSibling)){p=t.parentNode;if(a=c.getAttribute('data-cfemail')){for(e='',r='0x'+a.substr(0,2)|0,n=2;a.length-n;n+=2)e+='%'+('0'+('0x'+a.substr(n,2)^r).toString(16)).slice(-2);p.replaceChild(document.createTextNode(decodeURIComponent(e)),c)}p.removeChild(t)}}catch(u){}}()/* ]]> */</script></li>
 ''')

deobfuscate_cf_email(soup)

print soup
```