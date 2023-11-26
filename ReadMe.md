# apache mod_uwsgi flask deployment

### create virtual-env and install libs
- `python -m venv venv`
- `pip install -r requirements.txt`

## Apache download
- download link: `https://httpd.apache.org/docs/current/platform/windows.html#down`
- choose `Apache Lounge`
- config apache 

### config apache
1. set path , server listen port and server name :
```shell
Define SRVROOT "H:\Programs\Apache24"
ServerRoot "${SRVROOT}"
...
Listen 80
Listen 8000
...
ServerName test-flask-deploy
```
2. build a new file `conf/app.conf`: 
```shell
<VirtualHost *:8000>
    ServerName localhost:8000
    WSGIScriptAlias / I:\PythonProject\FLASKPROJECT\flask_demo\wsgi.py
    <Directory I:\PythonProject\FLASKPROJECT\flask_demo>
		Require all granted
		AllowOverride All
    </Directory>
</VirtualHost>
```
3. add into `httpd.conf`:
```shell
# Include Flask file
Include conf/app.conf
```

## Install mod_wsgi.whl 
1. *choose your python and mod_wsgi corresponding version*   
download and install: https://www.lfd.uci.edu/~gohlke/pythonlibs/#mod_wsgi
```shell
(venv) I:\PythonProject\flask_demo>pip install mod_wsgi-4.9.2-cp311-cp311-win_amd64.whl
Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
Processing i:\pythonproject\flask_demo\mod_wsgi-4.9.2-cp311-cp311-win_amd64.whl
Installing collected packages: mod-wsgi
Successfully installed mod-wsgi-4.9.2
```
2. copy below to httpd.conf
```shell
(venv) I:\PythonProject\flask_demo>mod_wsgi-express module-config
LoadFile "C:/Python311/python311.dll"
LoadModule wsgi_module "i:/pythonproject/flaskproject/flask-tmp/venv/Lib/site-packages/mod_wsgi/server/mod_wsgi.cp311-win_amd64.pyd"
WSGIPythonHome "i:/pythonproject/flaskproject/flask-tmp/venv"
```



## Build a flask app if not exist:
use the command and run `flask --app app run --debug`

## Create wsgi.py if not exist:
```python 
# flask_demo/wsgi.py
import sys
sys.path.insert(0, 'I:\\PythonProject\\FLASKPROJECT\\flask_demo')
from app import create_app
application = create_app()
```

## Debug info 
1. sqlite3.OperationalError: no such table 找不到表的原因
```shell
[Mon Nov 27 01:41:26.098807 2023] [wsgi:error] [pid 4404:tid 1216] H:\\Programs\\Apache24\\bin\\app.sqlite\r
[Mon Nov 27 01:41:26.106838 2023] [wsgi:error] [pid 4404:tid 1216] [client ::1:57768] H:\\Programs\\Apache24\\bin\\app.sqlite\r
```

- 注意apache用的是bin目录下的sqlite文件，该表是空的，为什么会自动创建，未知。
- 由于官方的做法会导致这样的问题，可以尝试用sqlalchemy配置数据，用配置类加载配置，