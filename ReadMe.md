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

