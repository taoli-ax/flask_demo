# apache mod_uwsgi flask deployment

## Apache download
- download link: `https://httpd.apache.org/docs/current/platform/windows.html#down`
- choose `Apache Lounge`
- modify file httpd.conf, my path`H:\Programs\Apache24\conf\httpd.conf`
  - line 37 change to your Apache path
    

### create virtual-env
`python -m venv venv`

### install mod_wsgi.whl
*choose your python and mod_wsgi corresponding version*
```shell
(venv) I:\PythonProject\flask_demo>pip install mod_wsgi-4.9.2-cp311-cp311-win_amd64.whl
Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
Processing i:\pythonproject\flask_demo\mod_wsgi-4.9.2-cp311-cp311-win_amd64.whl
Installing collected packages: mod-wsgi
Successfully installed mod-wsgi-4.9.2

```
### query info of mod_wsgi
```shell
(venv) I:\PythonProject\flask_demo>mod_wsgi-express module-config
LoadFile "C:/Python311/python311.dll"
LoadModule wsgi_module "i:/pythonproject/flaskproject/flask-tmp/venv/Lib/site-packages/mod_wsgi/server/mod_wsgi.cp311-win_amd64.pyd"
WSGIPythonHome "i:/pythonproject/flaskproject/flask-tmp/venv"

```
copy to httpd.conf file

### build a flask app if not exist:
use the command and run `flask --app app run --debug`


### 找不到表的原因
```shell
[Mon Nov 27 01:41:26.098807 2023] [wsgi:error] [pid 4404:tid 1216] H:\\Programs\\Apache24\\bin\\app.sqlite\r
[Mon Nov 27 01:41:26.106838 2023] [wsgi:error] [pid 4404:tid 1216] [client ::1:57768] H:\\Programs\\Apache24\\bin\\app.sqlite\r
```

- 注意apache用的是bin目录下的sqlite文件，该表是空的，为什么会自动创建，未知。
- 由于官方的做法会导致这样的问题，可以尝试用sqlalchemy配置数据，用配置类加载配置，