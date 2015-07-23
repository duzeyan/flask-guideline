# Flask ����

��һЩ�����Ǵ��������Ӧ�ö����õ��ġ��������Ӧ�ö���ʹ�ù�ϵ�����ݿ���û� ��֤��������֮ǰ�������ݿⲢ�õ���ǰ��¼�û�����Ϣ��������֮��ر����ݿ����ӡ�

�����û����׵Ĵ���Ƭ�Ϻͷ����μ� [Flask ����Ƭ�Ϲ鵵](http://flask.pocoo.org/snippets/) ��

## ����Ӧ��

���ڴ���Ӧ����˵ʹ�ð�����ģ����һ�������⡣ʹ�ð��ǳ��򵥡�������һ��СӦ���� ��:

```
/yourapplication
    /yourapplication.py
    /static
        /style.css
    /templates
        layout.html
        index.html
        login.html
        ...
```

### �򵥵İ�

Ҫ�������е�СӦ��װ��Ϊ����Ӧ��ֻҪ������Ӧ���д���һ����Ϊ yourapplication �����ļ��У��������ж������ƶ�������ļ����ڡ�Ȼ��� yourapplication.py ���� Ϊ __init__.py ����������ɾ������ .pyc �ļ�����������ϻ�����⣩

�޸����Ӧ��������:

```
/yourapplication
    /yourapplication
        /__init__.py
        /static
            /style.css
        /templates
            layout.html
            index.html
            login.html
            ...
```

���������������Ӧ���أ�ԭ���� python yourapplication/__init__.py �޷����� �ˡ���Ϊ Python ��ϣ�����ڵ�ģ���Ϊ�����ļ��������ⲻ��һ�������⣬ֻҪ�� yourapplication �ļ��������һ�� runserver.py �ļ��Ϳ����ˣ�����������:

```
from yourapplication import app
app.run(debug=True)
```

���Ǵ���ѧ����ʲô�������������ع�һ��Ӧ������Ӧ��ģ�顣ֻҪ��ס���¼��㣺

1. Flask Ӧ�ö������λ�� __init__.py �ļ��С�����ÿ��ģ��Ϳ��԰�ȫ�ص��� �ˣ��� __name__ �������������ȷ�İ���
2. ������ͼ�������ڶ����� route() �ģ������� __init__.py �ļ��б����롣���ǵ�����������ǵ�����ͼģ�顣�� ��Ӧ�ö��󴴽�֮�� ������ͼ����

__init__.py ʾ��:

```
from flask import Flask
app = Flask(__name__)

import yourapplication.views
```

views.py ��������:

```
from yourapplication import app

@app.route('/')
def index():
    return 'Hello World!'
```

����ȫ����������:

```
/yourapplication
    /runserver.py
    /yourapplication
        /__init__.py
        /views.py
        /static
            /style.css
        /templates
            layout.html
            index.html
            login.html
            ...
```

#### �ػ�����

�ػ�������ָ����ģ�黥�ർ�룬������������ӵ� views.py ���� __init__.py �໥������ÿ�� Python ����Ա������ػ����롣һ������»ػ������Ǹ������⣬�� ������һ�����ⶼû�С�ԭ��������û������ʹ�� __init__.py �е���ͼ��ֻ�� ��֤ģ�鱻���룬�����������ļ��ײ�����������

�������ַ�ʽ������Щ���⣬��Ϊû�а취ʹ��װ������Ҫ�ҵ����������������� ����Ӧ�� һ�ڡ�

### ʹ����ͼ

���ڴ���Ӧ���Ƽ���Ӧ�÷ָ�ΪС�飬ÿ��С��ʹ����ͼ����ִ�С������������Ľ��� �����[ʹ����ͼ��ģ�黯Ӧ��](blueprint-model.md)һ�� ��


## Ӧ�ù���

������Ѿ���Ӧ����ʹ���˰�����ͼ�� ʹ����ͼ��ģ�黯Ӧ�� ������ô������෽�����Ը� ��һ���ظĽ����Ӧ�á����õķ����ǵ�����ͼ�󴴽�Ӧ�ö��󣬵��������һ�������� ����������ô�Ϳ��Դ������ʵ����

��ô��������ʲô���أ�

1. ���ڲ��ԡ�������Բ�ͬ�����ʹ�ò�ͬ������������Ӧ�á�
2. ���ڶ�ʵ�����������Ҫ����ͬһ��Ӧ�õĲ�ͬ�汾�Ļ�����Ȼ������ڷ������� ʹ�ò�ͬ�������ж����ͬӦ�ã��������ʹ��Ӧ�ù�������ô�����ֻʹ��һ�� Ӧ�ý��̶��õ����Ӧ��ʵ�������������ײٿء�

��ô������أ�

### ��������

��������һ������������Ӧ�ã���������:

```
def create_app(config_filename):
    app = Flask(__name__)
    app.config.from_pyfile(config_filename)

    from yourapplication.model import db
    db.init_app(app)

    from yourapplication.views.admin import admin
    from yourapplication.views.frontend import frontend
    app.register_blueprint(admin)
    app.register_blueprint(frontend)

    return app
```

���������ȱ�����ڵ���ʱ�޷�����ͼ��ʹ��Ӧ�ö��󡣵����������һ��������ʹ������ ���ͨ������������Ӧ�ã�ʹ�� [current_app](http://dormousehole.readthedocs.org/en/latest/api.html#flask.current_app):

```
from flask import current_app, Blueprint, render_template
admin = Blueprint('admin', __name__, url_prefix='/admin')

@admin.route('/')
def index():
    return render_template(current_app.config['INDEX_TEMPLATE'])
```

���������������в���ģ������ơ�

��չ�����ʼ��ʱ����󶨵�һ��Ӧ�ã�Ӧ�ÿ���ʹ�� db.init_app ��������չ�� ��չ�����в��ᴢ���ض�Ӧ�õ�״̬�����һ����չ���Ա����Ӧ��ʹ�á�������չ��� �ĸ�����Ϣ����� [Flask ��չ����](app-extend.md) ��

��ʹ�� Flask-SQLAlchemy ʱ����� model.py ������������:

```
from flask.ext.sqlalchemy import SQLAlchemy
# no app object passed! Instead we use use db.init_app in the factory.
db = SQLAlchemy()

# create some models
```

### ʹ��Ӧ��

��ˣ�Ҫʹ��������Ӧ�þͱ����ȴ�������������һ������Ӧ�õ�ʾ�� run.py �ļ�:

```
from yourapplication import create_app
app = create_app('/path/to/config.cfg')
app.run()
```

### �Ľ�����

����Ĺ��������������㹻�ã����ԸĽ��ĵط���Ҫ�����¼��㣺

1. Ϊ�˵�Ԫ���ԣ�Ҫ��취�������ã������Ͳ������ļ�ϵͳ�д��������ļ���
2. ������Ӧ��ʱ����ͼ����һ�������������Ϳ����л����޸����ԣ���ҽ�����ǰ/�� �������ȣ���
4. ����б�Ҫ�Ļ���������һ��Ӧ��ʱ����һ�� WSGI �м����


## Ӧ�õ���

Ӧ�õ������� WSGI ������϶�� WSGI Ӧ�õĹ��̡�������϶�� Flask Ӧ�ã�Ҳ���� ��� Flask Ӧ�ú����� WSGI Ӧ�á�ͨ��������ϣ�����б�Ҫ�Ļ�������������ͬһ�� ��������һ������ Django ��һ������ Flask ��������ϵĺô�ȡ����Ӧ���ڲ������ �����ġ�

Ӧ�õ�����ģ�黯�����ͬ����Ӧ�õ����е�ÿ�� Ӧ������ȫ�����ģ������Ը��Ե��������У����� WSGI ���汻���ȡ�

### ˵��

�������еļ���˵���;����������һ�������������κ� WSGI �������� application ���󡣶��������������μ� ����ʽ �����ڿ��������� Werkzeug �ṩ��һ���ڽ���������������ʹ�� werkzeug.serving.run_simple() ������:

```
from werkzeug.serving import run_simple
run_simple('localhost', 5000, application, use_reloader=True)
```

ע�� [run_simple](http://werkzeug.pocoo.org/docs/serving/#werkzeug.serving.run_simple) ���������������������� �����������μ������ WSGI ������ ��

Ϊ��ʹ�ý�����������Ӧ�úͼ򵥷�������Ӧ�����ڵ���ģʽ��������һ���򵥵� �� hello world ��ʾ����ʹ���˵���ģʽ�� run_simple:

```
from flask import Flask
from werkzeug.serving import run_simple

app = Flask(__name__)
app.debug = True

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    run_simple('localhost', 5000, app,
               use_reloader=True, use_debugger=True, use_evalex=True)
```

### ���Ӧ��

���������ͬһ�� Python �����������ж��������Ӧ�ã���ô�����ʹ�� [werkzeug.wsgi.DispatcherMiddleware](http://werkzeug.pocoo.org/docs/middlewares/#werkzeug.wsgi.DispatcherMiddleware) ����ԭ���ǣ�ÿ�������� Flask Ӧ�ö� ��һ���Ϸ��� WSGI Ӧ�ã�����ͨ�������м�����Ϊһ������ǰ׺���ȵĴ�Ӧ�á�

���������Ӧ�������� / ����̨�ӿ�λ�� /backend:

```
from werkzeug.wsgi import DispatcherMiddleware
from frontend_app import application as frontend
from backend_app import application as backend

application = DispatcherMiddleware(frontend, {
    '/backend':     backend
})
```

### �����������

��ʱ���������Ҫʹ�ò�ͬ������������ͬһ��Ӧ�õĶ��ʵ�������԰�Ӧ�ô������� ����һ�������У�����������������Ϳ��Դ���һ��Ӧ�õ�ʵ��������ʵ�ֲμ� Ӧ�ù��� ������

�����������ÿ�����򴴽�һ��Ӧ�ã����÷��������������������Ӧ������ʹ�� �����������û��Զ����ʵ����һ����ķ��������Լ�������������ô�Ϳ���ʹ��һ�� �ܼ򵥵� WSGI Ӧ������̬����Ӧ���ˡ�

WSGI ���������ĳ���㣬��˿���дһ�����Լ��� WSGI Ӧ�����������󣬲���������� ����� Flask Ӧ�á�����������Ӧ�û�û�д�������ô�ͻᶯ̬����Ӧ�ò����Ǽ� ����:

```
from threading import Lock

class SubdomainDispatcher(object):

    def __init__(self, domain, create_app):
        self.domain = domain
        self.create_app = create_app
        self.lock = Lock()
        self.instances = {}

    def get_application(self, host):
        host = host.split(':')[0]
        assert host.endswith(self.domain), 'Configuration error'
        subdomain = host[:-len(self.domain)].rstrip('.')
        with self.lock:
            app = self.instances.get(subdomain)
            if app is None:
                app = self.create_app(subdomain)
                self.instances[subdomain] = app
            return app

    def __call__(self, environ, start_response):
        app = self.get_application(environ['HTTP_HOST'])
        return app(environ, start_response)
```

������ʾ��:

```
from myapplication import create_app, get_user_for_subdomain
from werkzeug.exceptions import NotFound

def make_app(subdomain):
    user = get_user_for_subdomain(subdomain)
    if user is None:
        # �������û�ж�Ӧ���û�����ô���ǵ÷���һ�� WSGI Ӧ��
        # ���ڴ��������������ǰ� NotFound() �쳣��ΪӦ�÷��أ�
        # ���ᱻ��ȾΪһ��ȱʡ�� 404 ҳ�档Ȼ�󣬿��ܻ���Ҫ��
        # �û��ض�����ҳ��
        return NotFound()

    # ����Ϊ�ض��û�����Ӧ��
    return create_app(user)

application = SubdomainDispatcher('example.com', make_app)
```

### ����·������


���� URL ��·�����ȷǳ��򵥡����棬����ͨ������ Host ͷ���ж��������� ֻҪ��������·���ĵ�һ��б��֮ǰ��·���Ϳ�����:

```
from threading import Lock
from werkzeug.wsgi import pop_path_info, peek_path_info

class PathDispatcher(object):

    def __init__(self, default_app, create_app):
        self.default_app = default_app
        self.create_app = create_app
        self.lock = Lock()
        self.instances = {}

    def get_application(self, prefix):
        with self.lock:
            app = self.instances.get(prefix)
            if app is None:
                app = self.create_app(prefix)
                if app is not None:
                    self.instances[prefix] = app
            return app

    def __call__(self, environ, start_response):
        app = self.get_application(peek_path_info(environ))
        if app is not None:
            pop_path_info(environ)
        else:
            app = self.default_app
        return app(environ, start_response)
```

������������������Ĳ�ͬ�ǣ�����·������ʱ����������������� None ����ô �ͻ���䵽��һ��Ӧ��:

```
from myapplication import create_app, default_app, get_user_for_prefix

def make_app(prefix):
    user = get_user_for_prefix(prefix)
    if user is not None:
        return create_app(user)

application = PathDispatcher(default_app, make_app)
```

## ʵ�� API �쳣����

�� Flask �Ͼ�����ִ�� RESTful API �����������Ȼ�����������֮һ������ API ���ڽ� �쳣�������������������ݲ��Ǻ����á�

���ڷǷ�ʹ�� API ����ʹ�� abort ���õĽ����ʽ��ʵ�����Լ����쳣�������ͣ� ����װ��Ӧ�������������û���ʽҪ��ĳ�����Ϣ��

### �򵥵��쳣��

������˼·������һ���µ��쳣������һ�����ʵĿɶ��Ըߵ���Ϣ��һ��״̬���һЩ ��ѡ�ĸ��أ��������ṩ����Ļ������ݡ�

������һ���򵥵�ʾ��:

```
from flask import jsonify

class InvalidUsage(Exception):
    status_code = 400

    def __init__(self, message, status_code=None, payload=None):
        Exception.__init__(self)
        self.message = message
        if status_code is not None:
            self.status_code = status_code
        self.payload = payload

    def to_dict(self):
        rv = dict(self.payload or ())
        rv['message'] = self.message
        return rv
```

����һ����ͼ�Ϳ����׳����г�����Ϣ���쳣�ˡ����⣬������ͨ�� payload ������ �ֵ����ʽ�ṩһЩ����ĸ��ء�

### ע��һ����������

���ڣ���ͼ�����׳��쳣�����ǻ���������һ���ڲ��������������Ϊû��Ϊ������� ������ע������������Ӻ����ף�����:

```
@app.errorhandler(InvalidAPIUsage)
def handle_invalid_usage(error):
    response = jsonify(error.to_dict())
    response.status_code = error.status_code
    return response
```

### ����ͼ�е��÷�

�������������ͼ��ʹ�øù���:

```
@app.route('/foo')
def get_foo():
    raise InvalidUsage('This view is gone', status_code=410)
```

## URL ������

New in version 0.7.

Flask 0.7 ������ URL ����������������Ϊ�㴦�����������ͬ���ֵ� URL ���������� ��� URL ���������Դ��룬�����ֲ�����ÿ�������ж��ظ�����������Դ��룬��ô�Ϳ� ����ʹ�� URL ��������

������ͼ���ʹ��ʱ�� URL �������������á��������Ƿֱ���ʾ��Ӧ���к���ͼ��ʹ�� URL ��������

### ���ʻ�Ӧ�õ� URL

������Ӧ������:

```
from flask import Flask, g

app = Flask(__name__)

@app.route('/<lang_code>/')
def index(lang_code):
    g.lang_code = lang_code
    ...

@app.route('/<lang_code>/about')
def about(lang_code):
    g.lang_code = lang_code
    ...
```

�����г����˴������ظ���������ÿһ�������а����Դ��븳ֵ�� g ���󡣵�Ȼ�����ʹ��һ��װ�������Լ�������������ǣ�������Ҫ������һ������ ָ����һ�������� URL ʱ�����ǵ���ʽ���ṩ���Դ��룬�൱�鷳��

����ʹ�� url_defaults() ��������������⡣������������Զ� ��ֵע�뵽 url_for() �����´������� URL �ֵ����Ƿ�������Դ��룬 �˵��Ƿ���Ҫһ����Ϊ 'lang_code' ��ֵ:

```
@app.url_defaults
def add_language_code(endpoint, values):
    if 'lang_code' in values or not g.lang_code:
        return
    if app.url_map.is_endpoint_expecting(endpoint, 'lang_code'):
        values['lang_code'] = g.lang_code
```

URL ӳ��� [is_endpoint_expecting()](http://werkzeug.pocoo.org/docs/routing/#werkzeug.routing.Map.is_endpoint_expecting) ���������ڼ�� �˵��Ƿ���Ҫ�ṩһ�����Դ��롣

�������������� [url_value_preprocessor()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.Flask.url_value_preprocessor) ����Щ���������� ƥ����������� URL ��ִֵ�д��롣���ǿ��Դ� URL �ֵ���ȡ��ֵ������ȡ����ֵ���� �����ط�:

```
@app.url_value_preprocessor
def pull_lang_code(endpoint, values):
    g.lang_code = values.pop('lang_code', None)
```

�����Ͳ�����ÿ�������а� lang_code ��ֵ�� g �ˡ��㻹������ ��һ���Ľ���дһ��װ���������Դ�����Ϊ URL ��ǰ׺�����Ǹ��õĽ����ʽ��ʹ�� ��ͼ��һ�� 'lang_code' ��ֵ���ֵ��е��������Ͳ��ٴ��͸���ͼ�����ˡ������Ĵ�������:

```
from flask import Flask, g

app = Flask(__name__)

@app.url_defaults
def add_language_code(endpoint, values):
    if 'lang_code' in values or not g.lang_code:
        return
    if app.url_map.is_endpoint_expecting(endpoint, 'lang_code'):
        values['lang_code'] = g.lang_code

@app.url_value_preprocessor
def pull_lang_code(endpoint, values):
    g.lang_code = values.pop('lang_code', None)

@app.route('/<lang_code>/')
def index():
    ...

@app.route('/<lang_code>/about')
def about():
    ...
```

### ���ʻ�����ͼ URL

��Ϊ��ͼ�����Զ������� URL ����һ��ͳһ��ǰ׺������Ӧ�õ�ÿ�������ͷǳ������ˡ� ����һ������Ϊ��ͼ URL Ԥ����������Ҫ��� URL �Ƿ������ҪҪһ�� 'lang_code' ���������Կ���ȥ�� [url_defaults()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.Flask.url_defaults))�����е� �߼��ж�:

```
from flask import Blueprint, g

bp = Blueprint('frontend', __name__, url_prefix='/<lang_code>')

@bp.url_defaults
def add_language_code(endpoint, values):
    values.setdefault('lang_code', g.lang_code)

@bp.url_value_preprocessor
def pull_lang_code(endpoint, values):
    g.lang_code = values.pop('lang_code')

@bp.route('/')
def index():
    ...

@bp.route('/about')
def about():
    ...
```

## ʹ�� Distribute ����

[distribute](http://pypi.python.org/pypi/distribute) ��ǰ���� setuptools ������һ����չ�⣬ͨ�����ڷַ� Python ��� ��չ������Ӣ�����Ƶľ��ǡ��ַ�������˼������չ�� Python �Դ���һ������ģ�鰲װ ϵͳ distutils ��֧�ֶ��ָ����ӵĽṹ�������˴���Ӧ�õķַ�����������Ҫ��ɫ��

* ֧������ ��һ�������Ӧ�ÿ�������������������������б������⽫���Զ� ��װ��
* ��ע�� �������ڰ�װ������ע����������Ϳ���ͨ��һ������ѯ����������Ϣ�� ����ϵͳ�������Ĺ����ǡ�����㡱����һ�������Զ���һ����ڣ��Ա����������ҽӣ� ������չ����
* ��װ���� �� distribute �е� easy_install ����Ϊ�㰲װ�����⡣��Ҳ���� ʹ���������� easy_install �� [pip](http://pypi.python.org/pypi/pip) �������˰�װ����������������¡�

Flask �����Լ����������� cheeseshop �п����ҵ��Ŀ�Ҫô���� distribute �ַ��ģ� Ҫô�����ϵ� setuptools �� distutils �ַ��ġ�

���������Ǽ������Ӧ�������� yourapplication.py ����û��ʹ��ģ�飬����һ��[��](http://dormousehole.readthedocs.org/en/latest/patterns/packages.html#larger-applications) �� distribute ��֧�ַַ���׼ģ�飬������ǲ� ����ģ������⡣������ΰ�ģ��ת��Ϊ������Ϣ�μ�[����Ӧ�÷���](huge-app.md)��

ʹ�� distribute ��ʹ���������ӣ�Ҳ�����Զ������������Ҫ��ȫ�Զ���������ͬʱ �Ķ� ʹ�� Fabric ���� һ�ڡ�

### �������ýű�

��Ϊ���Ѿ���װ�� Flask ��������Ӧ���Ѿ���װ�� setuptools �� distribute ����� û�а�װ�������£���һ�� [distribute_setup.py](http://python-distribute.org/distribute_setup.py) �ű����԰����㰲װ��ֻҪ������� �ű���������� Python �����������оͿ����ˡ�

��׼����: [���ʹ�� virtualenv](http://dormousehole.readthedocs.org/en/latest/installation.html#virtualenv) ��

������ô���Ӧ�÷��� setup.py �ļ��У�����ļ�Ӧ��λ��Ӧ���Աߡ�����ļ���ֻ�� һ��Լ����������ò�Ҫ�ı䣬��Ϊ��Ҷ���ȥ������ļ���

�ǵģ���ʹ��ʹ�� distribute ���㵼��İ�Ҳ�� setuptools �� distribute ��ȫ �������� setuptools �������ʹ����ͬ�ĵ������ơ�

Flask Ӧ�õĻ��� setup.py �ļ�ʾ������:

```
from setuptools import setup

setup(
    name='Your Application',
    version='1.0',
    long_description=__doc__,
    packages=['yourapplication'],
    include_package_data=True,
    zip_safe=False,
    install_requires=['Flask']
)
```

���ס���������ʽ���г��Ӱ��������Ҫ distribute �Զ�Ϊ���������������ʹ�� find_packages ����:

```
from setuptools import setup, find_packages

setup(
    ...
    packages=find_packages()
)
```

����� setup �Ĳ��������������壬���� include_package_data �� zip_safe ���Բ�������⡣ include_package_data ���� distribute Ҫ����һ�� MANIFEST.in �ļ������ļ�������ƥ���������Ŀ��Ϊ�����ݰ�װ������ͨ��ʹ����� �����ַ� Python ģ��ľ�̬�ļ���ģ�壨�μ� �ַ���Դ ���� zip_safe ��־������ǿ�ƻ��ֹ���� zip ѹ������ͨ���㲻����Ҫ�Ѱ���װΪ zip ѹ���ļ�����ΪһЩ���߲�֧��ѹ���ļ�������ѹ���ļ��Ƚ����Ե��ԡ�

### �ַ���Դ

����㳢�԰�װ���Ĵ����İ�����ᷢ������ static �� templates ֮����ļ��� û�б���װ��ԭ���� distribute ��֪��ҪΪ�������Щ�ļ�����Ҫ�����ǣ������ setup.py �ļ��Աߴ���һ�� MANIFEST.in �ļ�������ļ��г�������Ӧ����ӵ� tar ѹ�������ļ�:

```
recursive-include yourapplication/templates *
recursive-include yourapplication/static *
```

��Ҫ���˰� setup ������ include_package_data ��������Ϊ True ������ʹ�� ������ MANIFEST.in �ļ���ȫ���г���Ҳû���á�

### ��������

�������� install_requires �����������ģ����������һ���б��б��е�ÿһ��� һ����Ҫ�ڰ�װʱ�� PyPI ��õİ���ȱʡ����£����ǻ������°汾�İ���������� ָ����߰汾����Ͱ汾��ʾ��:

```
install_requires=[
    'Flask>=0.2',
    'SQLAlchemy>=0.6',
    'BrokenPackage>=0.7,<=1.0'
]
```

��ǰ���ᵽ������������ PyPI ��õġ��������Ҫ�ӱ�ĵط���ð���ô���أ���ֻҪ ���ǰ�����������д��Ȼ���ṩһ����ѡ��ַ�б������:

```
dependency_links=['http://example.com/yourfiles']
```

��ȷ��ҳ������һ��Ŀ¼�б���ҳ���ϵ�����ָ����ȷ�� tar ѹ���������� distribute �ͻ��ҵ��ļ��ˡ������İ��ڹ�˾�ڲ������ϣ����ṩָ��������� URL ��

### ��װ / ����

Ҫ��װ���Ӧ�ã�����������ǰ�װ��һ�� virtualenv ����ֻҪ���д� install ���� �� setup.py �ű��Ϳ����ˡ����Ὣ���Ӧ�ð�װ�� virtualenv �� site-packages �ļ����£�ͬʱ���ز���װ����:

```
$ python setup.py install
```

������������������ͬʱҲϣ�������������װ����ô����ʹ�� develop ������:

```
$ python setup.py develop
```

�������ĺô���ֻ��װһ��ָ�� site-packages �����ӣ������ǰ����ݸ��Ƶ�������� �ڿ��������оͲ���ÿ���޸��Ժ������� install �ˡ�


## ʹ�� Fabric ����

Fabric ��һ�� Python ���ߣ��� Makefiles ���ƣ������ܹ���Զ�̷�������ִ�� ���������ʵ��� Python ���� ����Ӧ�� �������������ã� ���ù��� ��������ô Fabric �������ⲿ�������ϲ��� Flask ��������

�����Ŀ�ʼ֮ǰ���м�����Ҫ��ȷ��

* Fabric 1.0 ��ҪҪ����װ�����ء����̳̼���ʹ�õ������°汾�� Fabric ��
* Ӧ���Ѿ���һ����������һ�����õ� setup.py �ļ��� ʹ�� Distribute ���� ����
* ������������У����Ǽ���Զ�̷�����ʹ�� mod_wsgi ����Ȼ�������ʹ�����Լ� ϲ���ķ�������������ʾ��������ѡ�� Apache + mod_wsgi ����Ϊ�������÷��㣬 ����û�� root Ȩ������¿��Է��������Ӧ�á�

### ������һ�� Fabfile

fabfile �ǿ��� Fabric �Ķ��������ļ���Ϊ fabfile.py ���� fab ����ִ�С��� ����ļ��ж�������к������ᱻ���� fab �������Щ�������һ������������ ���С���Щ���������� fabfile �ж��壬Ҳ�������������ж��塣�������� fabfile �� ����������

�����ǵ�һ�����ӣ��Ƚϼ��������԰ѵ�ǰ��Դ�����ϴ���������������װ��һ��Ԥ�ȴ��� �� virtual ����:

```
from fabric.api import *

# ʹ��Զ��������û���
env.user = 'appuser'
# ִ������ķ�����
env.hosts = ['server1.example.com', 'server2.example.com']

def pack():
    # ����һ���µķַ�Դ����ʽΪ tar ѹ����
    local('python setup.py sdist --formats=gztar', capture=False)

def deploy():
    # ����ַ��汾�����ƺͰ汾��
    dist = local('python setup.py --fullname', capture=True).strip()
    # �� tar ѹ������ʽ��Դ�����ϴ�������������ʱ�ļ���
    put('dist/%s.tar.gz' % dist, '/tmp/yourapplication.tar.gz')
    # ����һ�����ڽ�ѹ�����ļ��У���������ļ���
    run('mkdir /tmp/yourapplication')
    with cd('/tmp/yourapplication'):
        run('tar xzf /tmp/yourapplication.tar.gz')
        # ����ʹ�� virtual ������ Python ����������װ��
        run('/var/www/yourapplication/env/bin/python setup.py install')
    # ��װ��ɣ�ɾ���ļ���
    run('rm -rf /tmp/yourapplication /tmp/yourapplication.tar.gz')
    # ��� touch .wsgi �ļ����� mod_wsgi ����Ӧ������
    run('touch /var/www/yourapplication.wsgi')
```

�����е�ע����ϸ��Ӧ�����������ġ������� fabric �ṩ���������ļ�Ҫ˵����

* run - ��Զ�̷�������ִ��һ������
* local - �ڱ��ػ�����ִ��һ������
* put - �ϴ��ļ���Զ�̷�������
* cd - �ڷ������˸ı�Ŀ¼�������� with �������ʹ�á�

### ���� Fabfile

��ô������� fabfile �أ�����ʹ�� fab ���Ҫ��Զ�̷������ϲ���ǰ�汾�� �������ʹ���������:

```
$ fab pack deploy
```

�������������ҪԶ�̷��������Ѿ������� /var/www/yourapplication �ļ��У��� /var/www/yourapplication/env ��һ�� virtual ����������һ�����������ϻ�û�� ���������ļ��� .wsgi �ļ�����ô�����������һ���µķ������ϴ���һ���������� �أ�

�������ȡ������Ҫ���ö���̨�����������ֻ��һ̨Ӧ�÷���������������£�����ô �� fabfile �д���������һ����ࡣ��Ȼ���������ô�������������Գ�֮Ϊ setup �� bootstrap ����ʹ������ʱ��ʽ���ݷ���������:

```
$ fab -H newserver.example.com bootstrap
```

����һ���·��������������¼������裺

1. �� /var/www ����Ŀ¼�ṹ:

```
$ mkdir /var/www/yourapplication
$ cd /var/www/yourapplication
$ virtualenv --distribute env
```

2. �ϴ�һ���µ� application.wsgi �ļ���Ӧ�������ļ����� application.cfg �� ���������ϡ�

3. ����һ���µ����� yourapplication �� Apache ���ò���������Ҫȷ������ .wsgi �ļ��䶯���ӣ������� touch ��ʱ������Զ�����Ӧ�á��� ������Ϣ�μ� [mod_wsgi (Apache)](http://dormousehole.readthedocs.org/en/latest/deploying/mod_wsgi.html#mod-wsgi-deployment) ��

���ڵ������ǣ� application.wsgi �� application.cfg �ļ�����������

### WSGI �ļ�

WSGI �ļ����뵼��Ӧ�ã����һ���������һ�������������ڸ���Ӧ�õ�����ȥ�������á� ʾ��:

```
import os
os.environ['YOURAPPLICATION_CONFIG'] = '/var/www/yourapplication/application.cfg'
from yourapplication import app
```

Ӧ�ñ������������������ʼ���Լ��Ż���ݻ���������������:

```
app = Flask(__name__)
app.config.from_object('yourapplication.default_config')
app.config.from_envvar('YOURAPPLICATION_CONFIG')
```

���������[���ù���](config-manange.md)һ����������ϸ�Ľ��ܡ�

### �����ļ�

������̸����Ӧ�û���� YOURAPPLICATION_CONFIG ���������ҵ���ȷ�������ļ��� �������Ӧ���������ļ�����Ӧ�ÿ����ҵ��ĵط����ڲ�ͬ�ĵ����������ļ��ǲ�ͬ�ģ� ����һ�����ǲ��������ļ����汾����

һ�����еķ�������һ�������İ汾���Ʋֿ�Ϊ��ͬ�ķ��������治ͬ�������ļ���Ȼ�� �����з��������м����Ȼ������Ҫ�ĵط�ʹ�������ļ��ķ������ӣ����磺 /var/www/yourapplication ����

������Σ���������ֻ��һ����̨��������������ǿ���Ԥ���ֶ��ϴ������ļ���

### ��һ�β���

�������ǿ��Խ��е�һ�β����ˡ����Ѿ����ú��˷���������˷�������Ӧ���Ѿ����� virtual �������Ѽ���� apache ���á��������ǿ��Դ��Ӧ�ò���������:

```
$ fab pack deploy
```

Fabric ���ڻ��������з����������� fabfile �е������������������Ӧ�õõ�һ�� tar ѹ������Ȼ���ִ�зַ�����Դ�����ϴ������з���������װ����л setup.py �ļ�������Ҫ����������Զ���װ�� virtual ������

### ��һ��

��ǰ�ĵĻ����ϣ����и���ķ�������ȫ�������������ɣ�

* ����һ����ʼ���·������� bootstrap ��������Գ�ʼ��һ���µ� virtual ��������ȷ���� apache �ȵȡ�
* �������ļ�����һ�������İ汾���У��ѻ���õķ������ӷ����ʵ��ĵط���
* �����԰�Ӧ�ô������һ���汾���У��ڷ������ϼ�����°汾��װ����������� ����Ļع����ϰ汾��
* �ҽӲ��Թ��ܣ����㲿���ⲿ���������в��ԡ�
ʹ�� Fabric ��һ����Ȥ�����顣��ᷢ���ڵ����ϴ�� fab deploy �Ƿǳ�����ġ� ����Կ������Ӧ�ñ�����һ����һ���������ϡ�


## �� Flask ��ʹ�� SQLite 3

�� Flask �У�����Է���İ�������ݿ����ӣ������ڻ�����ɢʱ�ر�������ӣ� ͨ�������������ʱ�򣩡�

������һ���� Flask ��ʹ�� SQLite 3 ������:

```
import sqlite3
from flask import g

DATABASE = '/path/to/database.db'

def get_db():
    db = getattr(g, '_database', None)
    if db is None:
        db = g._database = connect_to_database()
    return db

@app.teardown_appcontext
def close_connection(exception):
    db = getattr(g, '_database', None)
    if db is not None:
        db.close()
```

Ϊ��ʹ�����ݿ⣬����Ӧ�ö�����׼����һ�����ڼ���״̬�Ļ�����ʹ�� get_db �������Եõ����ݿ����ӡ���������ɢʱ�����ݿ����ӻᱻ�жϡ�

ע�⣺�����ʹ�õ��� Flask 0.9 ������ǰ�İ汾����ô�����ʹ�� flask._app_ctx_stack.top �������� g ����Ϊ [flask.g](http://dormousehole.readthedocs.org/en/latest/api.html#flask.g) �����ǰ󶨵� ����ģ�������Ӧ�û�����

ʾ��:

```
@app.route('/')
def index():
    cur = get_db().cursor()
    ...
```

> Note ���ס����ɢ�����Ӧ�û����ĺ�����һ���ᱻִ�еġ���ʹ����ǰ������ִ��ʧ�ܻ����û��ִ�У���ɢ����Ҳ�ᱻִ�С���ˣ����Ǳ��뱣֤�ڹر����ݿ�����֮ǰ ���ݿ������Ǵ��ڵġ�


### ��������

������ʽ���ڵ�һ��ʹ��ʱ�������ݿ⣩���ŵ���ֻ����������Ҫʱ�Ŵ����ݿ����ӡ� �������Ҫ��һ�����󻷾�֮��ʹ�����ݿ����ӣ���ô������ֶ��� Python �������� Ӧ�û���:

```
with app.app_context():
    # now you can use get_db()
```

### �򻯲�ѯ

���ڣ���ÿ�����������п���ͨ������ g.db ���õ���ǰ�򿪵����ݿ����ӡ�Ϊ�� �� SQLite ��ʹ�ã�������һ�����õ��й����������ú�����ת��ÿ�δ����ݿⷵ�ص� ��������磬Ϊ�˵õ��ֵ����Ͷ�����Ԫ�����͵ķ��ؽ������������:

```
def make_dicts(cursor, row):
    return dict((cur.description[idx][0], value)
                for idx, value in enumerate(row))

db.row_factory = make_dicts
```

���߸��򵥵�:

```
db.row_factory = sqlite3.Row
```

���⣬�ѵõ��αִ꣬�в�ѯ�ͻ�ý����ϳ�һ����ѯ������ʧΪһ���ð취:

```
def query_db(query, args=(), one=False):
    cur = get_db().execute(query, args)
    rv = cur.fetchall()
    cur.close()
    return (rv[0] if rv else None) if one else rv
```

�����ķ����С�������й�������ʹ����ʹ��ԭʼ�����ݿ��α���������Ҫ������ˡ�

��������ʹ����������:

```
for user in query_db('select * from users'):
    print user['username'], 'has the id', user['user_id']
```

ֻ��Ҫ�õ���һ������÷�:

```
user = query_db('select * from users where username = ?',
                [the_username], one=True)
if user is None:
    print 'No such user'
else:
    print the_username, 'has the id', user['user_id']
```

���Ҫ�� SQL ��䴫�ݲ��������������ʹ���ʺ���������������Ѳ�������һ���б��� һ�𴫵ݡ���Ҫ���ַ�����ʽ���ķ�ʽֱ�ӰѲ������� SQL ����У��������Ӧ�ô��� [SQL ע��](http://en.wikipedia.org/wiki/SQL_injection)�ķ��ա�

### ��ʼ��ģʽ

��ϵ���ݿ�����Ҫģʽ�ģ����һ��Ӧ�ó�����Ҫһ�� schema.sql �ļ������� ���ݿ⡣���������Ҫʹ��һ������������ģʽ�������ݿ⡣��������������������� ����:

```
def init_db():
    with app.app_context():
        db = get_db()
        with app.open_resource('schema.sql', mode='r') as f:
            db.cursor().executescript(f.read())
        db.commit()
```

����ʹ������������ Python �������д������ݿ⣺

```
>>> from yourapplication import init_db
>>> init_db()
```

## �� Flask ��ʹ�� SQLAlchemy

�����ϲ��ʹ�� [SQLAlchemy](http://www.sqlalchemy.org/) ���������ݿ⡣��������� Flask Ӧ����ʹ�ð������� ģ�飬����ģ�ͷ���һ��������ģ���У��μ�[����Ӧ��](huge-app.md) ������Ȼ�� ���Ǳ���ģ����Ǻ����á�

������ SQLAlchemy �ĳ��÷���������һһ������

### Flask-SQLAlchemy ��չ

��Ϊ SQLAlchemy ��һ�����õ����ݿ����㣬������Ҫһ�������ò���ʹ�ã�������� Ϊ������һ������ SQLAlchemy ����չ���������Ҫ���ٵĿ�ʼʹ�� SQLAlchemy ����ô �Ƽ���ʹ�������չ��

����Դ� [PyPI](http://pypi.python.org/pypi/Flask-SQLAlchemy) ���� [Flask-SQLAlchemy](http://packages.python.org/Flask-SQLAlchemy/) ��

### ����

SQLAlchemy �е�������չ��ʹ�� SQLAlchemy �����·��������������� Django һ���� ��һ���ط�������ģ��Ȼ�󵽴�ʹ�á������������ݣ��ҽ������Ķ� ���� �Ĺٷ� �ĵ���

������ʾ�� database.py ģ��:

```
from sqlalchemy import create_engine
from sqlalchemy.orm import scoped_session, sessionmaker
from sqlalchemy.ext.declarative import declarative_base

engine = create_engine('sqlite:////tmp/test.db', convert_unicode=True)
db_session = scoped_session(sessionmaker(autocommit=False,
                                         autoflush=False,
                                         bind=engine))
Base = declarative_base()
Base.query = db_session.query_property()

def init_db():
    # �����ﵼ�붨��ģ������Ҫ������ģ�飬�������Ǿͻ���ȷ��ע����
    # Ԫ�����ϡ�������ͱ����ڵ��� init_db() ֮ǰ�������ǡ�
    import yourapplication.models
    Base.metadata.create_all(bind=engine)
```

Ҫ����ģ�͵Ļ���ֻҪ�̳����洴���� Base ��Ϳ����ˡ�����ܻ��������Ϊʲô ��������̣߳����������� SQLite3 ��������һ��ʹ�� g ���󣩡� ԭ���� SQLAlchemy �Ѿ��� scoped_session Ϊ���������˴� �๤����

���Ҫ��Ӧ������������ʽʹ�� SQLAlchemy ����ôֻҪ�����д������Ӧ��ģ��Ϳ��� �ˡ� Flask ���Զ����������ʱ����Ӧ�ùر�ʱɾ�����ݿ�Ự:

```
from yourapplication.database import db_session

@app.teardown_appcontext
def shutdown_session(exception=None):
    db_session.remove()
```

������һ��ʾ��ģ�ͣ����� models.py �У�:

```
from sqlalchemy import Column, Integer, String
from yourapplication.database import Base

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True)
    name = Column(String(50), unique=True)
    email = Column(String(120), unique=True)

    def __init__(self, name=None, email=None):
        self.name = name
        self.email = email

    def __repr__(self):
        return '<User %r>' % (self.name)
```

����ʹ�� init_db �������������ݿ⣺

```
>>> from yourapplication.database import init_db
>>> init_db()
```

�����ݿ��в�����Ŀʾ����

```
>>> from yourapplication.database import db_session
>>> from yourapplication.models import User
>>> u = User('admin', 'admin@localhost')
>>> db_session.add(u)
>>> db_session.commit()
```

��ѯ�ܼ򵥣�

```
>>> User.query.all()
[<User u'admin'>]
>>> User.query.filter(User.name == 'admin').first()
<User u'admin'>
```

### �˹������ϵӳ��

�˹������ϵӳ������������������ʽ���ŵ�Ҳ��ȱ�㡣��Ҫ�������˹������ϵӳ�� �ֱ������ಢӳ�����ǡ����ַ�ʽ��������Ҫ��Щ���롣ͨ�������ַ�ʽ������ ��ʽһ�����У������ȷ�������Ӧ���ڰ��з�Ϊ���ģ�顣

ʾ�� database.py ģ��:

```
from sqlalchemy import create_engine, MetaData
from sqlalchemy.orm import scoped_session, sessionmaker

engine = create_engine('sqlite:////tmp/test.db', convert_unicode=True)
metadata = MetaData()
db_session = scoped_session(sessionmaker(autocommit=False,
                                         autoflush=False,
                                         bind=engine))
def init_db():
    metadata.create_all(bind=engine)
```

������������һ��������Ҫ����������Ӧ�û�����ɢ��رջỰ�������´��������� Ӧ��ģ��:

```
from yourapplication.database import db_session

@app.teardown_appcontext
def shutdown_session(exception=None):
    db_session.remove()
```

������һ��ʾ�����ģ�ͣ����� models.py �У�:

```
from sqlalchemy import Table, Column, Integer, String
from sqlalchemy.orm import mapper
from yourapplication.database import metadata, db_session

class User(object):
    query = db_session.query_property()

    def __init__(self, name=None, email=None):
        self.name = name
        self.email = email

    def __repr__(self):
        return '<User %r>' % (self.name)

users = Table('users', metadata,
    Column('id', Integer, primary_key=True),
    Column('name', String(50), unique=True),
    Column('email', String(120), unique=True)
)
mapper(User, users)
```

��ѯ�Ͳ�����������ʽ��һ����

### SQL �����

�����ֻ��Ҫʹ�����ݿ�ϵͳ���� SQL ������㣬��ô������ֻҪʹ������:

```
from sqlalchemy import create_engine, MetaData

engine = create_engine('sqlite:////tmp/test.db', convert_unicode=True)
metadata = MetaData(bind=engine)
```

Ȼ����Ҫô��ǰ����һ���ڴ�����������Ҫô�Զ���������:

```
users = Table('users', metadata, autoload=True)
```

����ʹ�� insert �����������ݡ�Ϊ��ʹ���������Ǳ����ȵõ�һ�����ӣ�

```
>>> con = engine.connect()
>>> con.execute(users.insert(), name='admin', email='admin@localhost')
```

SQLAlchemy ���Զ��ύ��

����ֱ��ʹ���������������ѯ���ݿ⣺

```
>>> users.select(users.c.id == 1).execute().first()
(1, u'admin', u'admin@localhost')
```

��ѯ���Ҳ�����ֵ�Ԫ�飺

```
>>> r = users.select(users.c.id == 1).execute().first()
>>> r['name']
u'admin'
```

��Ҳ���԰� SQL �����Ϊ�ַ������ݸ� execute() ������

```
>>> engine.execute('select * from users where id = :1', [1]).first()
(1, u'admin', u'admin@localhost')
```

���� SQLAlchemy �ĸ�����Ϣ���Ʋ���[�ٷ���վ](http://sqlalchemy.org/) ��


## �ϴ��ļ�

�ǵģ�����Ҫ̸����һ�������⣺�ļ��ϴ����ļ��ϴ��Ļ���ԭ��ʵ���Ϻܼ򵥣��������ǣ�

1. һ������ enctype=multipart/form-data �� <form> ��ǣ�����к��� һ�� <input type=file> ��
2. Ӧ��ͨ���������� files �ֵ��������ļ���
3. ʹ���ļ��� [save()](http://werkzeug.pocoo.org/docs/datastructures/#werkzeug.datastructures.FileStorage.save) �������ļ����� �ر������ļ�ϵͳ�С�

### ���

�����Ǵ�һ��������Ӧ�ÿ�ʼ�����Ӧ���ϴ��ļ���һ��ָ��Ŀ¼�������ļ���ʾ���û��� ������Ӧ�õ���������:

```
import os
from flask import Flask, request, redirect, url_for
from werkzeug import secure_filename

UPLOAD_FOLDER = '/path/to/the/uploads'
ALLOWED_EXTENSIONS = set(['txt', 'pdf', 'png', 'jpg', 'jpeg', 'gif'])

app = Flask(__name__)
app.config['UPLOAD_FOLDER'] = UPLOAD_FOLDER
```

�������ǵ�����һ�Ѷ������������ǳ���׶��ġ�werkzeug.secure_filename() �� ���Ժ���͡�UPLOAD_FOLDER ���ϴ��ļ�Ҫ�����Ŀ¼��ALLOWED_EXTENSIONS ������ �ϴ����ļ���չ���ļ��ϡ��������Ǹ�Ӧ���ֶ������һ�� URL ����һ�����ڲ����� ���������Ϊʲô��ô�����أ�ԭ����������Ҫ�������������ǵĿ�����������Ϊ�����ṩ �����������ֻ������Щ�ļ��� URL �Ĺ���

ΪʲôҪ�����ļ�������չ���أ����ֱ����ͻ��˷������ݣ���ô����ܲ��������û� �ϴ������ļ������������ȷ���û������ϴ� HTML �ļ�����Ϊ HTML �������� XSS ���⣨�μ���վ�ű�������XSS�� �����������������ִ�� PHP �ļ�����ô������ȷ���������ϴ� .php �ļ�������˭�ֻ��ڷ������ϰ�װ PHP �أ��Բ��� :)

��һ�����������չ���Ƿ�Ϸ����ϴ��ļ������û��ض������ϴ��ļ��� URL:

```
def allowed_file(filename):
    return '.' in filename and \
           filename.rsplit('.', 1)[1] in ALLOWED_EXTENSIONS

@app.route('/', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        file = request.files['file']
        if file and allowed_file(file.filename):
            filename = secure_filename(file.filename)
            file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
            return redirect(url_for('uploaded_file',
                                    filename=filename))
    return '''
    <!doctype html>
    <title>Upload new File</title>
    <h1>Upload new File</h1>
    <form action="" method=post enctype=multipart/form-data>
      <p><input type=file name=file>
         <input type=submit value=Upload>
    </form>
    '''
```

��ô [secure_filename()](http://werkzeug.pocoo.org/docs/utils/#werkzeug.utils.secure_filename) ������������ʲô�ã���һ��ԭ���ǡ� ��Զ��Ҫ�����û����롱������ԭ��ͬ�����������ϴ��ļ����ļ����������ύ�ı����� ������α��ģ��ļ���Ҳ������Σ�յġ���ʱҪ���ǣ��ڰ��ļ����浽�ļ�ϵͳ֮ǰ����Ҫ ʹ������������ļ������а��졣

#### ��һ��˵��

����Ի���� secure_filename() ������Щ�����������ʹ�� ������ʲô������������˰��������Ϣ��Ϊ filename ���ݸ����Ӧ��:

```
filename = "../../../../home/username/.bashrc"
```

���� ../ �ĸ�������ȷ�ģ��������� UPLOAD_FOLDER �����һ����ô�û� �Ϳ����������޸�һ���������ϵ��ļ�������ļ��������û���Ȩ�޸ĵġ�����Ҫ�˽� Ӧ����������еģ������������ң��ڿͶ��ǲ�̬�� :)

������������������ι����ģ�

```
>>> secure_filename('../../../../home/username/.bashrc')
'home_username_.bashrc'
```

���ڻ�ʣ��һ���£�Ϊ���ϴ����ļ��ṩ���� Flask 0.5 �汾��ʼ���ǿ���ʹ��һ�� ����������������:

```
from flask import send_from_directory

@app.route('/uploads/<filename>')
def uploaded_file(filename):
    return send_from_directory(app.config['UPLOAD_FOLDER'],
                               filename)
```

���⣬���԰� uploaded_file ע��Ϊ build_only ���򣬲�ʹ�� SharedDataMiddleware �����ַ�ʽ������ Flask �ϰ汾�� ʹ��:

```
from werkzeug import SharedDataMiddleware
app.add_url_rule('/uploads/<filename>', 'uploaded_file',
                 build_only=True)
app.wsgi_app = SharedDataMiddleware(app.wsgi_app, {
    '/uploads':  app.config['UPLOAD_FOLDER']
})
```

�������������Ӧ�ã���ôӦ��һ�ж�Ӧ�ð�Ԥ������������

### �Ľ��ϴ�

New in version 0.6.

Flask ��������δ����ļ��ϴ����أ�����ϴ����ļ���С����ô������Ǵ������ڴ��С� ����ͻ�����Ǳ��浽һ����ʱ��λ�ã�ͨ�� tempfile.gettempdir() ���Եõ� ���λ�ã������ǣ���������ϴ��ļ��ĳߴ��أ�ȱʡ����£� Flask �ǲ������ϴ��ļ� �ĳߴ�ġ�����ͨ���������õ� MAX_CONTENT_LENGTH �������ļ��ߴ�:

```
from flask import Flask, Request

app = Flask(__name__)
app.config['MAX_CONTENT_LENGTH'] = 16 * 1024 * 1024
```

����Ĵ����ѳߴ�����Ϊ 16 M ������ϴ��˴�������ߴ���ļ��� Flask ���׳�һ�� RequestEntityTooLarge �쳣��

Flask 0.6 �汾�������������ܡ�����ͨ���̳���������ڽ��ϵİ汾��Ҳ����ʵ�� ������ܡ�������Ϣ����� Werkzeug �����ļ�������ĵ���

### �ϴ�������

�ڲ�����ǰ����࿪����������ʵ���ϴ��������ģ��ֿ��ȡ�ϴ����ļ��������ݿ��д��� �ϴ��Ľ��ȣ�Ȼ���ڿͻ���ͨ�� JavaScript ��ȡ���ȡ������֮���ͻ���ÿ 5 ������ ������ѯ��һ���ϴ����ȡ����÷���𣿿ͻ�������֪���ʡ�

�������˸��õĽ�������������Ҹ��ɿ������緢���˺ܴ�仯��������ڿͻ���ʹ�� HTML5 �� JAVA �� Silverlight �� Flash ��ø��õ��ϴ����顣��鿴���¿⣬ѧϰ һЩ������ϴ���ʾ����

* [Plupload](http://www.plupload.com/) - HTML5, Java, Flash
* [SWFUpload](http://www.swfupload.org/) - Flash
*  [JumpLoader](http://jumploader.com/) - Java

###һ�������ķ���

��Ϊ����Ӧ�����ϴ��ļ��ķ���������ͬ����˿���ʹ�� Flask-Uploads ��չ��ʵ�� �ļ��ϴ��������չʵ�����������ϴ����ƣ������а��������ܡ������������Լ����� ���ܡ�


## ����

�����Ӧ�ñ�����ʱ�򣬿��Կ��Ǽ��뻺�档����������򵥵ļ��ٷ�����������ʲô�ã� ������һ��������ʱ�ϳ���������������������ǰ���صĽ��������ȷ�ġ���ô���Ǿ� ���Կ��ǰ���������Ľ���ڻ����д��һ��ʱ�䡣

Flask �����ṩ���棬�������Ļ�����֮һ Werkzeug ��һЩ�ǳ������Ļ���֧�֡��� ֧�ֶ��ֻ����ˣ�ͨ������Ҫʹ�õ��� memcached ��������

### ����һ������

�봴�� [Flask](http://dormousehole.readthedocs.org/en/latest/api.html#flask.Flask) �������ƣ�����Ҫ����һ��������󲢱������������ ����ʹ�ÿ�������������ô����Դ���һ�� [SimpleCache](http://werkzeug.pocoo.org/docs/contrib/cache/#werkzeug.contrib.cache.SimpleCache) ������������ṩ�򵥵Ļ��棬���� ������Ŀ������ Python ���������ڴ���:

```
from werkzeug.contrib.cache import SimpleCache
cache = SimpleCache()
```

�����Ҫʹ�� memcached ����ô��ȷ���� memcache ģ��֧�֣�����Դ� PyPI ���ģ�飩��һ���������е� memcached �������� ���� memcached ������ʾ��:

```
from werkzeug.contrib.cache import MemcachedCache
cache = MemcachedCache(['127.0.0.1:11211'])
```

���������ʹ�� App Engine ����ô����Է�������ӵ� App Engine memcache ������:

```
from werkzeug.contrib.cache import GAEMemcachedCache
cache = GAEMemcachedCache()
```

### ʹ�û���

���ڵ����������ʹ�û����أ��������ǳ���Ҫ�Ĳ����� [get()](http://werkzeug.pocoo.org/docs/contrib/cache/#werkzeug.contrib.cache.BaseCache.get) �� [set()](http://werkzeug.pocoo.org/docs/contrib/cache/#werkzeug.contrib.cache.BaseCache.set) ������չʾ���ʹ�����ǣ�

get() ���ڴӻ����л����Ŀ������ʱʹ�� һ���ַ�����Ϊ�����������Ŀ���ڣ���ô�ͻ᷵�������Ŀ�����򷵻� None:

```
rv = cache.get('my-item')
```

set() ���ڰ���Ŀ��ӵ������С���һ������ �Ǽ������ڶ��������Ǽ�ֵ���������ṩһ����ʱ������������ʱ�����Ŀ���Զ�ɾ����

������һ������������:

```
def get_my_item():
    rv = cache.get('my-item')
    if rv is None:
        rv = calculate_value()
        cache.set('my-item', rv, timeout=5 * 60)
    return rv
```

## ��ͼװ����

Python ��һ���ǳ���Ȥ�Ĺ��ܣ�����װ������������ܿ���ʹ����Ӧ�øɾ����ࡣ Flask �е�ÿ����ͼ����һ��װ�����������Ա�ע�����Ĺ��ܡ�������Ѿ��ù��� [route()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.Flask.route) װ���������ǣ����п�����Ҫʹ�����Լ���װ������������ һ����ͼ��ֻ���Ѿ���¼���û�����ʹ�á�����û�����ʱû�е�¼����ᱻ�ض��� ��¼ҳ�档��������¾���ʹ��װ�����ľ��ѻ��ᡣ

### ����¼װ����

��������ʵ�����װ������װ������һ�����غ����ĺ���������ȥ���ӣ���ʵ�ܼ򵥡�ֻҪ ��סһ���£�װ�������ڸ��º����� __name__ �� __module__ ���������ԡ���һ��� �������ǣ������㲻���˹����º������ԣ�����ʹ��һ��������װ�����ĺ����� [functools.wraps()](http://docs.python.org/dev/library/functools.html#functools.wraps) ����

�����Ǽ���¼װ���������ӡ������¼ҳ��Ϊ 'login' ����ǰ�û��������� g.user �У������û�е�¼����ֵΪ None:

```
from functools import wraps
from flask import g, request, redirect, url_for

def login_required(f):
    @wraps(f)
    def decorated_function(*args, **kwargs):
        if g.user is None:
            return redirect(url_for('login', next=request.url))
        return f(*args, **kwargs)
    return decorated_function
```

���ʹ�����װ�����أ������װ����������������ĵط������ˡ���ʹ�ø���һ���� װ����ʱ�����סҪ�� route() װ��������������:

```
@app.route('/secret_page')
@login_required
def secret_page():
    pass
```

### ����װ����

������һ����ͼ������Ҫ���İ���ļ���ɱ����������Ҫ��һ��ʱ���ڻ��������ͼ�� �����������������װ������һ���õ�ѡ�����Ǽ������� ���� ������һ�������˻��档

������һ��ʾ�����溯����������һ���ض���ǰ׺��ʵ������һ����ʽ�ַ������������ ��ǰ·�����ɻ������ע�⣬������ʹ����һ������������װ���������װ��������װ�� �������������ֿڰɣ�ȷʵ��һ�㸴�ӣ����������ʾ�����뻹�Ǻ����׶����ġ�

��װ�δ��밴���²��蹤��

1. ���ڻ���·����õ�ǰ�����Ψһ�������
2. �ӻ����л�ȡ��ֵ�������ȡ�ɹ��򷵻ػ�ȡ����ֵ��
3. �������ԭ���ĺ��������ѷ���ֵ����ڻ����У�ֱ�����ڣ�ȱʡֵΪ����ӣ���

����:

```
from functools import wraps
from flask import request

def cached(timeout=5 * 60, key='view/%s'):
    def decorator(f):
        @wraps(f)
        def decorated_function(*args, **kwargs):
            cache_key = key % request.path
            rv = cache.get(cache_key)
            if rv is not None:
                return rv
            rv = f(*args, **kwargs)
            cache.set(cache_key, rv, timeout=timeout)
            return rv
        return decorated_function
    return decorator
```

ע�⣬���ϴ���������һ�����õ�ʵ������ cache ���󣬸�����Ϣ�μ� ���� ������

### ģ��װ����

����ǰ�� TurboGear ���˷�����ģ��װ�������ͨ��ģʽ���乤��ԭ���Ƿ���һ���ֵ䣬 ����ֵ��������ͼ���ݸ�ģ���ֵ��ģ���Զ�����Ⱦ�������������ӵĹ�������ͬ��:

```
@app.route('/')
def index():
    return render_template('index.html', value=42)

@app.route('/')
@templated('index.html')
def index():
    return dict(value=42)

@app.route('/')
@templated()
def index():
    return dict(value=42)
```

���������������û���ṩģ�����ƣ���ô�ͻ�ʹ�� URL ӳ��Ķ˵㣨�ѵ�ת��Ϊб�ܣ� ���� '.html' ������ṩ�ˣ���ô�ͻ�ʹ�����ṩ��ģ�����ơ���װ������������ ʱ�����ص��ֵ�ͱ����͵�ģ����Ⱦ������������ص��� None ���ͻ�ʹ�ÿ��ֵ䡣��� ���صĲ����ֵ䣬��ô�ͻ�ֱ�Ӵ���ԭ�ⲻ���ķ���ֵ�������Ϳ�����Ȼʹ���ض������� ���ؼ򵥵��ַ�����

������װ�����Ĵ���:

```
from functools import wraps
from flask import request

def templated(template=None):
    def decorator(f):
        @wraps(f)
        def decorated_function(*args, **kwargs):
            template_name = template
            if template_name is None:
                template_name = request.endpoint \
                    .replace('.', '/') + '.html'
            ctx = f(*args, **kwargs)
            if ctx is None:
                ctx = {}
            elif not isinstance(ctx, dict):
                return ctx
            return render_template(template_name, **ctx)
        return decorated_function
    return decorator
```

### �˵�װ����

������Ҫʹ�� werkzeug ·��ϵͳ���Ա��ڻ�ø�ǿ�������ʱ����Ҫ�� Rule �ж����һ�����Ѷ˵�ӳ�䵽��ͼ��������������Ҫ �õ�װ�����ˡ�����:

```
from flask import Flask
from werkzeug.routing import Rule

app = Flask(__name__)
app.url_map.add(Rule('/', endpoint='index'))

@app.endpoint('index')
def my_index():
    return "Hello world"
```

## ʹ�� WTForms ���б���֤

������봦��������ύ�ı�����ʱ����ͼ����ܿ���������Ķ�����һЩ����� ���������������֮һ���� [WTForms](http://wtforms.simplecodes.com/) ���������ǽ���������⡣�������봦�� ��������ôӦ������ʹ������⡣

���Ҫʹ�� WTForms ����ô����Ҫ�ѱ�����Ϊ�ࡣ���Ƽ���Ӧ�÷ָ�Ϊ���ģ�飨 [����Ӧ��](huge-app.md)������Ϊ�����һ��������ģ�顣

####  ʹ��һ����չ��ô󲿷� WTForms �Ĺ���

Flask-WTF ��չ����ʵ�ֱ����������й��ܣ����һ��ṩһЩС�ĸ������ߡ�ʹ�� �����չ���Ը��õ�ʹ�ñ��� Flask ������Դ� PyPI ��������չ��


### ��

������һ�����͵�ע��ҳ���ʾ��:

```
from wtforms import Form, BooleanField, TextField, PasswordField, validators

class RegistrationForm(Form):
    username = TextField('Username', [validators.Length(min=4, max=25)])
    email = TextField('Email Address', [validators.Length(min=6, max=35)])
    password = PasswordField('New Password', [
        validators.Required(),
        validators.EqualTo('confirm', message='Passwords must match')
    ])
    confirm = PasswordField('Repeat Password')
    accept_tos = BooleanField('I accept the TOS', [validators.Required()])
```

### ��ͼ

����ͼ�����У����÷�ʾ������:

```
@app.route('/register', methods=['GET', 'POST'])
def register():
    form = RegistrationForm(request.form)
    if request.method == 'POST' and form.validate():
        user = User(form.username.data, form.email.data,
                    form.password.data)
        db_session.add(user)
        flash('Thanks for registering')
        return redirect(url_for('login'))
    return render_template('register.html', form=form)
```

ע�⣬��������Ĭ����ͼʹ���� SQLAlchemy ���� Flask ��ʹ�� SQLAlchemy������Ȼ�ⲻ�Ǳ���ġ���������ʵ������޸Ĵ��롣

���ס���¼��㣺

1. ���������ͨ�� HTTP POST �����ύ�ģ������ form �� ֵ�������������ͨ�� GET �����ύ�ģ�����Ӧ���� args ��
2. ���� validate() ��������֤���ݡ������֤ͨ������ �������� True �����򷵻� False ��
3. ͨ�� form.<NAME>.data ���Է��ʱ��е���ֵ��

### ģ���еı�

��������������ģ�塣�ѱ����ݸ�ģ���Ϳ���������Ⱦ�����ˡ���һ�������ʾ�� ģ��Ϳ���֪���ж������ˡ� WTForms �����������һ������ɹ�����Ϊ�����ø��ã� ���ǿ���дһ���꣬ͨ���������Ⱦ����һ����ǩ���ֶκʹ����б�����еĻ�����

������һ��ʹ�ú��ʾ�� _formhelpers.html ģ�壺

```
{% macro render_field(field) %}
  <dt>{{ field.label }}
  <dd>{{ field(**kwargs)|safe }}
  {% if field.errors %}
    <ul class=errors>
    {% for error in field.errors %}
      <li>{{ error }}</li>
    {% endfor %}
    </ul>
  {% endif %}
  </dd>
{% endmacro %}
```

�����еĺ����һ�Ѵ��ݸ� WTForm �ֶκ����Ĳ�����Ϊ������Ⱦ�ֶΡ���������Ϊ HTML ���Բ��롣��������Ե��� render_field(form.username, class='username') �� Ϊ����Ԫ�����һ���ࡣע�⣺ WTForms ���ر�׼�� Python unicode �ַ������������ ����ʹ�� |safe ���������� Jinja2 ��Щ�����Ѿ����� HTML ת���ˡ�

������ʹ��������� _formhelpers.html �� register.html ģ�壺

```
{% from "_formhelpers.html" import render_field %}
<form method=post action="/register">
  <dl>
    {{ render_field(form.username) }}
    {{ render_field(form.email) }}
    {{ render_field(form.password) }}
    {{ render_field(form.confirm) }}
    {{ render_field(form.accept_tos) }}
  </dl>
  <p><input type=submit value=Register>
</form>
```

������� WTForms ����Ϣ���Ʋ� [WTForms �ٷ���վ](http://wtforms.simplecodes.com/) ��


## ģ��̳�

Jinja �������Ĳ��־���ģ��̳С�ģ��̳������㴴��һ���������Ǽܡ�ģ�塣���ģ�� �а���վ��ĳ���Ԫ�أ�������Ա���ģ��̳е� �� ��

�������ܸ�����ʵ�������򵥣�������������Ӿ���������ˡ�

### ����ģ��

���ģ��������� layout.html ����������һ���򵥵� HTML �Ǽܣ�������ʾһ�� �򵥵�����ҳ�档���ӡ�ģ������������������յĿ飺

```
<!doctype html>
<html>
  <head>
    {% block head %}
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
    <title>{% block title %}{% endblock %} - My Webpage</title>
    {% endblock %}
  </head>
  <body>
    <div id="content">{% block content %}{% endblock %}</div>
    <div id="footer">
      {% block footer %}
      &copy; Copyright 2010 by <a href="http://domain.invalid/">you</a>.
      {% endblock %}
    </div>
  </body>
</html>
```

����������У� {% block %} ��Ƕ������ĸ����Ա���ģ�����Ŀ顣 block ��Ǹ���ģ����������һ�����Ա���ģ�����صĲ��֡�

### ��ģ��

��ģ��ʾ����

```
{% extends "layout.html" %}
{% block title %}Index{% endblock %}
{% block head %}
  {{ super() }}
  <style type="text/css">
    .important { color: #336699; }
  </style>
{% endblock %}
{% block content %}
  <h1>Index</h1>
  <p class="important">
    Welcome on my awesome homepage.
{% endblock %}
```

���� {% extends %} ����ǹؼ���������ģ���������ģ�塰��չ������һ��ģ�壬 ��ģ��ϵͳ�������ģ��ʱ�����ҵ���ģ�塣�����չ��Ǳ�����ģ���еĵ�һ����ǡ� ���Ҫʹ�ø�ģ���еĿ����ݣ���ʹ�� {{ super() }} ��


## ��Ϣ����

һ���õ�Ӧ�ú��û����涼��Ҫ���õķ���������û��ò����㹻�ķ�������ôӦ������ �ᱻ�û������� Flask ������ϵͳ�ṩ��һ�����õķ�����ʽ������ϵͳ�Ļ���������ʽ �ǣ�����ֻ����һ�������з�����һ���������ʱ��¼����Ϣ��һ�����ǽ�ϲ���ģ���� ʹ������ϵͳ��

### �򵥵�����

������һ��������ʾ��:

```
from flask import Flask, flash, redirect, render_template, \
     request, url_for

app = Flask(__name__)
app.secret_key = 'some_secret'

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/login', methods=['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != 'admin' or \
           request.form['password'] != 'secret':
            error = 'Invalid credentials'
        else:
            flash('You were successfully logged in')
            return redirect(url_for('index'))
    return render_template('login.html', error=error)

if __name__ == "__main__":
    app.run()
```

������ʵ�����ֵ� layout.html ģ�壺

```
<!doctype html>
<title>My Application</title>
{% with messages = get_flashed_messages() %}
  {% if messages %}
    <ul class=flashes>
    {% for message in messages %}
      <li>{{ message }}</li>
    {% endfor %}
    </ul>
  {% endif %}
{% endwith %}
{% block body %}{% endblock %}
```

������ index.html ģ�壺

```
{% extends "layout.html" %}
{% block body %}
  <h1>Overview</h1>
  <p>Do you want to <a href="{{ url_for('login') }}">log in?</a>
{% endblock %}
```

login ģ�壺

```
{% extends "layout.html" %}
{% block body %}
  <h1>Login</h1>
  {% if error %}
    <p class=error><strong>Error:</strong> {{ error }}
  {% endif %}
  <form action="" method=post>
    <dl>
      <dt>Username:
      <dd><input type=text name=username value="{{
          request.form.username }}">
      <dt>Password:
      <dd><input type=password name=password>
    </dl>
    <p><input type=submit value=Login>
  </form>
{% endblock %}
```

### ������Ϣ�����

New in version 0.3.

������Ϣ������ָ��������û��ָ������ôȱʡ�����Ϊ 'message' ����ͬ�� �����Ը��û��ṩ���õķ��������������Ϣ����ʹ�ú�ɫ������

ʹ�� flash() ��������ָ����Ϣ�����:

```
flash(u'Invalid password provided', 'error')
```

ģ���е� [get_flashed_messages()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.get_flashed_messages) ����ҲӦ�����������ʾ��Ϣ��ѭ�� ҲҪ�����ı䣺

```
{% with messages = get_flashed_messages(with_categories=true) %}
  {% if messages %}
    <ul class=flashes>
    {% for category, message in messages %}
      <li class="{{ category }}">{{ message }}</li>
    {% endfor %}
    </ul>
  {% endif %}
{% endwith %}
```

����չʾ��θ��������Ⱦ��Ϣ�������Ը���Ϣ����ǰ׺���� <strong>Error:</strong> ��

### ����������Ϣ

New in version 0.9.

����������ͨ������һ������б������� [get_flashed_messages()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.get_flashed_messages) �� �������������������ڲ�ͬλ����ʾ��ͬ������Ϣ��

```
{% with errors = get_flashed_messages(category_filter=["error"]) %}
{% if errors %}
<div class="alert-message block-message error">
  <a class="close" href="#">��</a>
  <ul>
    {%- for msg in errors %}
    <li>{{ msg }}</li>
    {% endfor -%}
  </ul>
</div>
{% endif %}
{% endwith %}
```

## ͨ�� jQuery ʹ�� AJAX

[jQuery](http://jquery.com/) ��һ��С�͵� JavaScript �⣬ͨ�����ڼ� DOM �� JavaScript ��ʹ�á��� ��һ���ǳ��õĹ��ߣ�����ͨ���ڷ���˺Ϳͻ��˽��� JSON ��ʹ����Ӧ�ø����ж�̬�ԡ�

JSON ��һ�ַǳ����ɵĴ����ʽ���ǳ������� Python ԭ����֡��ַ������ֵ���б� ���� JSON ���㷺֧�֣����ڽ����� JSON �ڼ���֮ǰ��ʼ���У�������Ӧ����Ѹ��ȡ�� �� XML ��

### ���� jQuery

Ϊ��ʹ�� jQuery ��������Ȱ�����������������Ӧ�õľ�̬Ŀ¼�У���ȷ���������롣 �������������һ����������ҳ��Ĳ���ģ�塣�����ģ��� <body> �ĵײ����һ�� script ��������� jQuery ��

```
<script type=text/javascript src="{{
  url_for('static', filename='jquery.js') }}"></script>
```

��һ��������ʹ�� Google �� AJAX �� API ������ jQuery��

```
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
<script>window.jQuery || document.write('<script src="{{
  url_for('static', filename='jquery.js') }}">\x3C/script>')</script>
```

�����ַ�ʽ�У�Ӧ�û��ȳ��Դ� Google ���� jQuery �����ʧ�������þ�̬Ŀ¼�е� ���� jQuery ���������ĺô�������û��Ѿ�ȥ��ʹ���� Google ��ͬ�汾�� jQuery �� ��վ�󣬷��������վʱ��ҳ����ܻ��������룬��Ϊ������Ѿ������� jQuery ��

### �ҵ���վ�����

�ҵ���վ�����������Ӧ�û��ڿ����У���ô�𰸺ܼ򵥣����ڱ�����ĳ���˿��ϣ� ���ڷ������ĸ�·���¡���������Ժ�Ҫ��Ӧ���Ƶ�����λ�ã�����< http://example.com/myapp> �����أ��ڷ���ˣ�������ⲻ��Ϊ���⣬����ʹ�� url_for() �������õ��𰸡������������ʹ�� jQuery ����ô�Ͳ���Ӳ��Ӧ�õ�·����ֻ��ʹ�ö�̬·������ô�죿

һ���򵥵ķ�������ҳ�������һ�� script ��ǣ�����һ��ȫ�ֱ�������ʾӦ�õĸ� ·����ʾ����

```
<script type=text/javascript>
  $SCRIPT_ROOT = {{ request.script_root|tojson|safe }};
</script>
```

�� Flask 0.10 �汾��ǰ�汾�У�ʹ�� |safe ���б�Ҫ�ģ���Ϊ��ʹ Jinja ��Ҫ ת�� JSON ������ַ�����ͨ�����������Ǳ���ģ������� script �ڲ�������Ҫ��ô ����

#### ��һ��˵��

�� HTML �У� script ������������� CDATA �ģ�Ҳ����˵���������ݲ��ᱻ ������<script> �� </script> ֮������ݶ��ᱻ��Ϊ�ű�������Ҳ��ζ�� �� script ���֮�䲻������κ� </ �������� |tojson ����ȷ�������⣬ ��Ϊ��ת��б�ܣ� {{ "</script>"|tojson|safe }} �ᱻ��ȾΪ "<\/script>" ����

�� Flask 0.10 �汾�и�����һ���������� HTML ��Ƕ��� unicode ת���ˣ�����ʹ Flask �Զ��� HTML ת��Ϊ��ȫ��ǡ�

## JSON ��ͼ����

����������������һ������˺�������������������� URL ������������Ҫ��ӵ����� ����Ȼ����Ӧ�÷���һ�� JSON ����������������Ƿǳ���ʵ�õģ���Ϊһ����� �ͻ���������ƹ�������������ӿ��Լ����˵�չʾ���ʹ�� jQuery �� Flask:

```
from flask import Flask, jsonify, render_template, request
app = Flask(__name__)

@app.route('/_add_numbers')
def add_numbers():
    a = request.args.get('a', 0, type=int)
    b = request.args.get('b', 0, type=int)
    return jsonify(result=a + b)

@app.route('/')
def index():
    return render_template('index.html')
```

�������������һ������һ�� index ��������Ⱦģ�塣���ģ��ᰴǰ���������� jQuery ��ģ������һ����������������ӵı���һ����������˺��������ӡ�

ע�⣬��������ʹ���� [get()](http://werkzeug.pocoo.org/docs/datastructures/#werkzeug.datastructures.MultiDict.get) �������� �������ʧ�ܡ�����ֵ�ļ������ڣ��ͻ᷵��һ��ȱʡֵ�������� 0 ��������һ�� �������ֵת��Ϊָ���ĸ�ʽ�������� int �����ڽű��� API �� JavaScript �ȣ� �����Ĵ�����ʹ�����ر𷽱㣬��Ϊ����������²���Ҫ����Ĵ��󱨸档

## HTML

��� index.html ģ��Ҫô�̳�һ���Ѿ����� jQuery �����ú� $SCRIPT_ROOT ������ layout.html ģ�壬Ҫô��ģ�忪ͷ�������������¡��������Ӧ�õ� HTML ʾ�� �� index.html ����ע�⣬���ǰѽű�ֱ�ӷ����� HTML �С�ͨ�����õķ�ʽ�Ƿ��� �����Ľű��ļ��У�

```
<script type=text/javascript>
  $(function() {
    $('a#calculate').bind('click', function() {
      $.getJSON($SCRIPT_ROOT + '/_add_numbers', {
        a: $('input[name="a"]').val(),
        b: $('input[name="b"]').val()
      }, function(data) {
        $("#result").text(data.result);
      });
      return false;
    });
  });
</script>
<h1>jQuery Example</h1>
<p><input type=text size=5 name=a> +
   <input type=text size=5 name=b> =
   <span id=result>?</span>
<p><a href=# id=calculate>calculate server side</a>
```

���ﲻ���� jQuery ������ϸ���������������һ����˵����

1. $(function() { ... }) �����������ҳ��Ļ�������������ɺ�����ִ�е�. ���롣
2. $('selector') ѡ��һ��Ԫ�ع��������
3. element.bind('event', func) ����һ���û����Ԫ��ʱ���еĺ������������ ���� false ����ôȱʡ��Ϊ�Ͳ��������ã�����Ϊת�� # URL ����
4. \$.getJSON(url, data, func) �� url ����һ�� GET ���󣬲��� data �����������Ϊ��ѯ������һ�������ݷ��أ���������ָ���ĺ��������ѷ���ֵ��Ϊ �����Ĳ�����ע�⣬���ǿ���������ʹ����ǰ����� $SCRIPT_ROOT ������

�����û��һ�������ĸ����� github ����[ʾ��Դ����](http://github.com/mitsuhiko/flask/tree/master/examples/jqueryexample) ��

## �Զ������ҳ��

Flask ��һ������� [abort()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.abort) ������������ͨ��һ�� HTTP ��������˳� һ�����������ṩһ����������˵���ĳ���ҳ�棬ҳ����ʾ�ڰ׵��ı��������ء�

�û����Ը��ݴ�����������֪��������ʲô����

### �����������

���³���������û������ģ���ʹӦ������Ҳ�������Щ������룺

**404 Not Found**
����һ�����ϵġ����ѣ���ʹ����һ������� URL ����Ϣ�������Ϣ���ֵ���� Ƶ�����������������������ֶ�֪�� 404 ���������ģ���Ҫ���Ķ��������ˡ� һ���õ�������ȷ�� 404 ҳ������һЩ�������õĶ���������Ҫ��һ��������ҳ �����ӡ�
**403 Forbidden**
��������վ����ĳ��Ȩ�޿��ƣ���ô���û�����δ����Ȩ����ʱӦ������ 403 ���롣�����ȷ�����û����Է���δ����Ȩ����ʱ�õ���ȷ�ķ�����
**410 Gone**
��֪�� ��404 Not Found�� ��һ������ ��410 Gone�� ���ֵ��𣿺�������ʹ����� ���롣�����Դ��ǰ�������ڹ������������Ѿ���ɾ���ˣ���ô��Ӧ��ʹ�� 410 ���룬������ 404 ������㲻�������ݿ��а��ĵ����õ�ɾ������ֻ�Ǹ��ĵ��� ��һ��ɾ����ǣ���ô��Ϊ�û����ǣ�Ӧ��ʹ�� 410 ���룬����ʾ��Ϣ��֪�û� Ҫ�ҵĶ����Ѿ�ɾ����
**500 Internal Server Error**
�������ͨ����ʾ����������������ء�ǿ�ҽ�������ҳ��Ū���Ѻ�һ�㣬 ��Ϊ���Ӧ�� ���� ����ֹ��ϵģ��μ�[����Ӧ�ô���](http://dormousehole.readthedocs.org/en/latest/errorhandling.html#application-errors)����

### ��������

һ������������һ������������һ����ͼ����һ��������ͼ������֮ͬ�����ڳ������� �ڳ��ִ���ʱ�����ã��Ҵ��ݴ��󡣴���������һ�� [HTTPException](http://werkzeug.pocoo.org/docs/exceptions/#werkzeug.exceptions.HTTPException) ��������һ�����⣺�������ڲ����������� ʱ����쳣ʵ�����ݸ�����������

��������ʹ�� [errorhandler()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.Flask.errorhandler) װ����ע�ᣬע��ʱӦ�ṩ�쳣�� �����롣���ס�� Flask ���� Ϊ�����ó�����룬�����ȷ���ڷ�����Ӧʱ��ͬʱ�ṩ HTTP ״̬���롣

������һ������ ��404 Page Not Found�� �쳣��ʾ��:

```
from flask import render_template

@app.errorhandler(404)
def page_not_found(e):
    return render_template('404.html'), 404
```

ʾ��ģ�壺

```
{% extends "layout.html" %}
{% block title %}Page Not Found{% endblock %}
{% block body %}
  <h1>Page Not Found</h1>
  <p>What you were looking for is just not there.
  <p><a href="{{ url_for('index') }}">go somewhere nice</a>
{% endblock %}
```

## ����������ͼ

Flask ͨ��ʹ��װ������װ���������ã�ֻҪ�� URL ������Ӧ�ĺ�����ǰ��Ϳ����ˡ� �������ַ�ʽ��һ��ȱ�㣺ʹ��װ�����Ĵ������Ԥ�ȵ��룬���� Flask ���޷������ҵ� ��ĺ�����

���������ٵ���Ӧ��ʱ����ͻ��Ϊһ�����⡣�� Google App Engine ������ϵͳ�У� ������ٵ���Ӧ�á���ˣ�������Ӧ�ô���������⣬��ô����ʹ�ü��� URL ӳ�䡣

[add_url_rule()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.Flask.add_url_rule) �������ڼ��� URL ӳ�䣬��ʹ��װ������ͬ������ ��Ҫһ������Ӧ������ URL ��ר���ļ���

### ת��Ϊ���� URL ӳ��

����������Ӧ��:

```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
    pass

@app.route('/user/<username>')
def user(username):
    pass

```

Ϊ�˼���ӳ�䣬���Ǵ���һ����ʹ��װ�������ļ��� views.py ��:

```
def index():
    pass

def user(username):
    pass
```

����һ���ļ��м���ӳ�亯���� URL:

```
from flask import Flask
from yourapplication import views
app = Flask(__name__)
app.add_url_rule('/', view_func=views.index)
app.add_url_rule('/user/<username>', view_func=views.user)
```

### �ӳ�����

���ˣ�����ֻ�ǰ���ͼ��·�ɷ��룬����ģ�黹��Ԥ�������ˡ�����ķ�ʽ�ǰ������� ��ͼ����������ʹ��һ�����ƺ����ĸ�������ʵ�ְ�������:

```
from werkzeug import import_string, cached_property

class LazyView(object):

    def __init__(self, import_name):
        self.__module__, self.__name__ = import_name.rsplit('.', 1)
        self.import_name = import_name

    @cached_property
    def view(self):
        return import_string(self.import_name)

    def __call__(self, *args, **kwargs):
        return self.view(*args, **kwargs)
```

����������Ҫ������ȷ���� __module__ �� __name__ �����������ڲ��ṩ URL ���� ���������ȷ���� URL ����

Ȼ������������ж��� URL ����:

```
from flask import Flask
from yourapplication.helpers import LazyView
app = Flask(__name__)
app.add_url_rule('/',
                 view_func=LazyView('yourapplication.views.index'))
app.add_url_rule('/user/<username>',
                 view_func=LazyView('yourapplication.views.user'))
```

�����Խ�һ���Ż����룺дһ���������� [add_url_rule()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.Flask.add_url_rule) ������ Ӧ��ǰ׺�͵����:

```
def url(url_rule, import_name, **options):
    view = LazyView('yourapplication.' + import_name)
    app.add_url_rule(url_rule, view_func=view, **options)

url('/', 'views.index')
url('/user/<username>', 'views.user')
```

��һ������Ҫ�μǣ�����ǰ����������������ڵ�һ������ǰ���롣

�����װ��������ͬ��������������д��


## �� Flask ��ʹ�� MongoKit

����ʹ���ĵ������ݿ���ȡ����ϵ�����ݿ���Խ��Խ������������չʾ���ʹ�� MongoKit ,����һ�����ڲ��� MongoDB ���ĵ�ӳ��⡣

��������Ҫһ�������е� MongoDB ���������Ѱ�װ�õ� MongoKit �⡣

ʹ�� MongoKit �����ֳ��õķ�����������һ˵����

### ����

������ MongoKit ��ȱʡ��Ϊ�����˼·������ Django �� SQLAlchemy ��������

������һ��ʾ�� app.py ģ��:

```
from flask import Flask
from mongokit import Connection, Document

# configuration
MONGODB_HOST = 'localhost'
MONGODB_PORT = 27017

# create the little application object
app = Flask(__name__)
app.config.from_object(__name__)

# connect to the database
connection = Connection(app.config['MONGODB_HOST'],
                        app.config['MONGODB_PORT'])
```

���Ҫ����ģ�ͣ���ôֻҪ�̳� MongoKit �� Document ������ˡ�������Ѿ����� SQLAlchemy ��������ô���Ի��������Ϊʲôû��ʹ�ûỰ������û�ж���һ�� init_db ������һ��������Ϊ MongoKit û�����ƻỰ�ڶ�������ʱ���������дһ�� ���룬����ʹ�����ٶȸ��졣��һ��������Ϊ MongoDB ����ģʽ�ġ������ζ�ſ����� �������ݵ�ʱ���޸����ݽṹ�� MongoKit Ҳ����ģʽ�ģ�����ִ��һЩ��֤����ȷ�� ���ݵ������ԡ�

������һ��ʾ���ĵ�����ʾ������Ҳ���� app.py ��:

```
def max_length(length):
    def validate(value):
        if len(value) <= length:
            return True
        raise Exception('%s must be at most %s characters long' % length)
    return validate

class User(Document):
    structure = {
        'name': unicode,
        'email': unicode,
    }
    validators = {
        'name': max_length(50),
        'email': max_length(120)
    }
    use_dot_notation = True
    def __repr__(self):
        return '<User %r>' % (self.name)
```

# �ڵ�ǰ������ע���û��ĵ�
connection.register([User])
����չʾ��ζ���ģʽ�������ṹ�����ַ�����󳤶���֤���������л�ʹ����һ�� MongoKit ������� use_dot_notation ���ܡ�ȱʡ����£� MongoKit ��������ʽ�� Python ���ֵ����ơ�������� use_dot_notation ����Ϊ True ����ô�Ϳɼ����� ���� ORM һ��ʹ�õ�������ָ����ԡ�

������������������Ŀ�������ݿ��У�

```
>>> from yourapplication.database import connection
>>> from yourapplication.models import User
>>> collection = connection['test'].users
>>> user = collection.User()
>>> user['name'] = u'admin'
>>> user['email'] = u'admin@localhost'
>>> user.save()
```

ע�⣬ MongoKit ���������͵�ʹ���ǱȽ��ϸ�ġ����� name �� email �У��㶼 ����ʹ�� str ���ͣ�Ӧ��ʹ�� unicode ��

��ѯ�ǳ��򵥣�

```
>>> list(collection.User.find())
[<User u'admin'>]
>>> collection.User.find_one({'name': u'admin'})
<User u'admin'>
```

### PyMongo ���ݲ�

�����ֻ��Ҫʹ�� PyMongo ��Ҳ����ʹ�� MongoKit �������ַ�ʽ�¿��Ի����ѵ� ���ܡ�ע�⣬����ʾ���У�û�� MongoKit �� Flask ���ϵ����ݣ����ϵķ�ʽ�μ�����:

```
from MongoKit import Connection

connection = Connection()
```

ʹ�� insert �������Բ������ݡ������ȱ����ȵõ�һ�����ӡ�������������� SQL �� �ı�

```
>>> collection = connection['test'].users
>>> user = {'name': u'admin', 'email': u'admin@localhost'}
>>> collection.insert(user)
```

MongoKit ���Զ��ύ��

ֱ��ʹ�ü��ϲ�ѯ���ݿ⣺

```
>>> list(collection.find())
[{u'_id': ObjectId('4c271729e13823182f000000'), u'name': u'admin', u'email': u'admin@localhost'}]
>>> collection.find_one({'name': u'admin'})
{u'_id': ObjectId('4c271729e13823182f000000'), u'name': u'admin', u'email': u'admin@localhost'}
```

��ѯ���Ϊ���ֵ����

```
>>> r = collection.find_one({'name': u'admin'})
>>> r['email']
u'admin@localhost'
```

���� MongoKit �ĸ�����Ϣ�����Ʋ���[�ٷ���վ](https://github.com/namlook/mongokit) ��


## ���һ��ҳ��ͼ��

һ����ҳ��ͼ�ꡱ��������ڱ�ǩ����ǩ��ʹ�õ�ͼ�꣬�����Ը������վ����һ��Ψһ �ı�ʾ������������������վ��

��ô��θ�һ�� Flask Ӧ�����һ��ҳ��ͼ���أ����ȣ��Զ��׼��ģ���Ҫһ��ͼ�ꡣ ͼ��Ӧ���� 16 X 16 ���ص� ICO ��ʽ�ļ����ⲻ�ǹ涨�ģ���ȴ��һ������������� ֧�ֵ���ʵ�ϵı�׼���� ICO �ļ�����Ϊ favicon.ico �����뾲̬ �ļ�Ŀ¼ �С�

��������Ҫ��������ܹ��ҵ����ͼ�꣬��ȷ������������� HTML �����һ�����ӡ� ʾ����

```
<link rel="shortcut icon" href="{{ url_for('static', filename='favicon.ico') }}">
```

���ڴ�����������˵����������������ˣ�����һЩ�ϹŶ���֧�������׼���ϵı�׼ �ǰ���Ϊ�� favicon.ico ����ͼ����ڷ������ĸ�Ŀ¼�¡�������Ӧ�ò��ǹҽ������ ��Ŀ¼�£���ô����Ҫ������ҳ�������ڸ�Ŀ¼���ṩ���ͼ�꣬������޼ƿ�ʩ�ˡ� ������Ӧ��λ�ڸ�Ŀ¼�£���ô����Լ򵥵ؽ����ض���:

```
app.add_url_rule('/favicon.ico',
                 redirect_to=url_for('static', filename='favicon.ico'))
```

�����Ҫ���������ض���������ô������ʹ�� [send_from_directory()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.send_from_directory) ������дһ����ͼ:

```
import os
from flask import send_from_directory

@app.route('/favicon.ico')
def favicon():
    return send_from_directory(os.path.join(app.root_path, 'static'),
                               'favicon.ico', mimetype='image/vnd.microsoft.icon')
```

�����е� MIME ���Ϳ���ʡ�ԣ���������Զ��²����͡�������������������ȷ�����ˣ� ʡȥ�˶���Ĳ²⣬������������ǲ���ġ�

������ͨ�����Ӧ�����ṩͼ�꣬������ܵĻ�������������ר�÷��������ṩͼ�꣬ ���÷����μ���ҳ���������ĵ���

���
Wikipedia �ϵ�[ҳ��ͼ��](http://en.wikipedia.org/wiki/Favicon)����

## ������

��ʱ�������Ҫ�Ѵ������ݴ��͵��ͻ��ˣ������ڴ��б�����Щ���ݡ�������������в��� �����ݲ������ļ�ϵͳ������ֱ�ӷ��͸��ͻ���ʱ��Ӧ����ô���أ�

����ʹ����������ֱ����Ӧ��

### �����÷�

������һ���������в������� CSV ���ݵĻ�����ͼ�������似���ǵ���һ�������������� ���ݣ�������������ݸ�һ����Ӧ����:

```
from flask import Response

@app.route('/large.csv')
def generate_large_csv():
    def generate():
        for row in iter_all_rows():
            yield ','.join(row) + '\n'
    return Response(generate(), mimetype='text/csv')
```

ÿ�� yield ���ʽ��ֱ�Ӵ��͸��������ע�⣬��һЩ WSGI �м�����ܻ����� ���ݣ������ʹ�÷����������������ߵĵ��Ի�����ҪС��һЩ��

### ģ���е�������

Jinja2 ģ������Ҳ֧�ַ�Ƭ��Ⱦģ�塣������ܲ���ֱ�ӱ� Flask ֧�ֵģ���Ϊ��̫ �����ˣ���������Է������������:

```
from flask import Response

def stream_template(template_name, **context):
    app.update_template_context(context)
    t = app.jinja_env.get_template(template_name)
    rv = t.stream(context)
    rv.enable_buffering(5)
    return rv

@app.route('/my-large-page.html')
def render_large_template():
    rows = iter_all_rows()
    return Response(stream_template('the_template.html', rows=rows))
```

�����ļ����Ǵ� Jinja2 �����л��Ӧ�õ�ģ����󣬲����� stream() ������ render() ������ һ��������������һ���ַ��������������ƹ��� Flask ��ģ����Ⱦ����ʹ����ģ����� �������������Ҫ���� [update_template_context()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.Flask.update_template_context) ����ȷ�� ���±���Ⱦ�����ݡ�������ģ����������ݡ�����ÿ�β������ݺ󣬷�������������� ���͸��ͻ��ˣ���˿�����Ҫ�������������ݡ�����ʹ���� rv.enable_buffering(size) �����л��档 5 ��һ���Ƚ����ǵ�ȱʡֵ��

### �����е�������

New in version 0.9.

ע�⣬��������������ʱ�����󻷾��Ѿ��ں���ִ��ʱ��ʧ�ˡ� Flask 0.9 Ϊ���ṩ�� һ���������������������������ڼ䱣�����󻷾�:

```
from flask import stream_with_context, request, Response

@app.route('/stream')
def streamed_response():
    def generate():
        yield 'Hello '
        yield request.args['name']
        yield '!'
    return Response(stream_with_context(generate()))
```

���û��ʹ�� [stream_with_context()](http://dormousehole.readthedocs.org/en/latest/api.html#flask.stream_with_context) ��������ô�ͻ����� [RuntimeError](http://docs.python.org/dev/library/exceptions.html#RuntimeError) ����

## �ӳٵ�����ص�

Flask �����˼·֮һ�ǣ���Ӧ���󴴽��󱻴��ݸ�һ���ص���������Щ�ص����������޸� ���滻��Ӧ���󡣵�������ʼ��ʱ����Ӧ����û�б���������Ӧ��������һ����ͼ ��������ϵͳ�е�����������贴���ġ�

���ǵ���Ӧ����û�д���ʱ����������޸���Ӧ�����أ�������һ������ǰ�����У����� ��Ҫ������Ӧ��������һ�� cookie ��

ͨ������ѡ��ܿ��������Ρ�������Գ��԰�Ӧ���߼��ƶ�����������С����ǣ���ʱ�� ����������˲�ˬ�������ô����úܳ�ª��

��ͨ�ķ����ǰ�һ�ѻص��������� g �����ϣ��������������ʱ������Щ �ص�������������Ϳ�����Ӧ�õ�����ط��ӳٻص�������ִ�С�

### װ����

�����װ������һ���ؼ������� g ������ע��һ�������б�:

```
from flask import g

def after_this_request(f):
    if not hasattr(g, 'after_request_callbacks'):
        g.after_request_callbacks = []
    g.after_request_callbacks.append(f)
    return f
```

### �����ӳٵĻص�����

���ˣ�ͨ��ʹ�� after_this_request װ������ʹ�ú������������ʱ���Ա����á����� ������ʵ��������ù��̡����ǰ���Щ����ע��Ϊ after_request() �ص�����:

```
@app.after_request
def call_after_request_callbacks(response):
    for callback in getattr(g, 'after_request_callbacks', ()):
        callback(response)
    return response
```

### һ��ʵ��

�������ǿ��Է������ʱ���Ϊ�ض�����ע��һ��������������������������ʱ�����á� ���磬�����������ǰ�����а��û��ĵ�ǰ���Լ�¼�� cookie ��:

```
from flask import request

@app.before_request
def detect_user_language():
    language = request.cookies.get('user_lang')
    if language is None:
        language = guess_language_from_request()
        @after_this_request
        def remember_language(response):
            response.set_cookie('user_lang', language)
    g.language = language
 Logo
Table Of Contents
```

## ��� HTTP ��������

һЩ HTTP ����֧������ HTTP �������߲�֧��һЩ���µ� HTTP ���������� PACTH ��������������£�����ͨ��ʹ����ȫ�෴��Э�飬��һ�� HTTP ��������������һ�� HTTP ������

ʵ�ֵ�˼·���ÿͻ��˷���һ�� HTTP POST ���󣬲����� X-HTTP-Method-Override ͷ��Ϊ��Ҫ�� HTTP ���������� PATCH ����

ͨ�� HTTP �м�����Է����ʵ��:

```
class HTTPMethodOverrideMiddleware(object):
    allowed_methods = frozenset([
        'GET',
        'HEAD',
        'POST',
        'DELETE',
        'PUT',
        'PATCH',
        'OPTIONS'
    ])
    bodyless_methods = frozenset(['GET', 'HEAD', 'OPTIONS', 'DELETE'])

    def __init__(self, app):
        self.app = app

    def __call__(self, environ, start_response):
        method = environ.get('HTTP_X_HTTP_METHOD_OVERRIDE', '').upper()
        if method in self.allowed_methods:
            method = method.encode('ascii', 'replace')
            environ['REQUEST_METHOD'] = method
        if method in self.bodyless_methods:
            environ['CONTENT_LENGTH'] = '0'
        return self.app(environ, start_response)
```

ͨ�����´���Ϳ����� Flask һͬ������:

```
from flask import Flask

app = Flask(__name__)
app.wsgi_app = HTTPMethodOverrideMiddleware(app.wsgi_app)
```

## ��������У��

�������ݻ��ɲ�ͬ�Ĵ������������Ԥ�������� JSON ���ݺͱ����ݶ���Դ���Ѿ� ��ȡ�������������󣬵������ǵĴ�������ǲ�ͬ�ġ�����������ҪУ����������� ����ʱ�ͻ������鷳����ˣ���ʱ����б�Ҫʹ��һЩ API ��

���˵��ǿ���ͨ����װ������������ظı����������

�����������ʾ�� WSGI �����¶�ȡ�ʹ����������ݣ��õ����ݵ� SHA1 У��:

```
import hashlib

class ChecksumCalcStream(object):

    def __init__(self, stream):
        self._stream = stream
        self._hash = hashlib.sha1()

    def read(self, bytes):
        rv = self._stream.read(bytes)
        self._hash.update(rv)
        return rv

    def readline(self, size_hint):
        rv = self._stream.readline(size_hint)
        self._hash.update(rv)
        return rv

def generate_checksum(request):
    env = request.environ
    stream = ChecksumCalcStream(env['wsgi.input'])
    env['wsgi.input'] = stream
    return stream._hash
```

Ҫʹ��������࣬��ֻҪ������ʼ��������֮ǰ����Ҫ��������Ϳ����ˡ�������С�� ���� request.form �����ƶ��������� before_request_handlers ��Ӧ��С�Ĳ� Ҫ��������

�÷�ʾ��:

```
@app.route('/special-api', methods=['POST'])
def special_api():
    hash = generate_checksum(request)
    # Accessing this parses the input stream
    files = request.files
    # At this point the hash is fully constructed.
    checksum = hash.hexdigest()
    return 'Hash was: %s' % checksum
```

## ���� Celery �ĺ�̨����

Celery ��һ�� Python ��д����һ���첽�������/���ڷֲ�ʽ��Ϣ���ݵ���ҵ���С� ��ǰ����һ�� Flask �ļ��ɣ����ǴӰ汾 3 ��ʼ����������һЩ�ڲ����ع����Ѿ� ����Ҫ��������ˡ�������Ҫ˵������� Flask ����ȷʹ�� Celery �����ļ����� �Ѿ��Ķ�������ٷ��ĵ��е� Celery ����

### ��װ Celery

Celery �� Python �������� PyPI ���ϰ�����������˿���ʹ�� pip �� easy_install ֮���׼�� Python ��������װ:

```
$ pip install celery
```

### ���� Celery

��������Ҫ��һ�� Celery ʵ�������ʵ����Ϊ celery Ӧ�á����λ���൱�� Flask �� Flask һ�������ʵ������������ Celery ����������ڣ����紴�� ���񡢹����˵ȵȡ������������Ա�����ģ�鵼�롣

���磬����԰�������һ�� tasks ģ���С���������Ҫ�������ã���Ϳ���ʹ�� tasks �����࣬���� Flask Ӧ�û�����֧�֣������� Flask �����á�

ֻҪ���������Ϳ����� Falsk ��ʹ�� Celery ��:

```
from celery import Celery

def make_celery(app):
    celery = Celery(app.import_name, broker=app.config['CELERY_BROKER_URL'])
    celery.conf.update(app.config)
    TaskBase = celery.Task
    class ContextTask(TaskBase):
        abstract = True
        def __call__(self, *args, **kwargs):
            with app.app_context():
                return TaskBase.__call__(self, *args, **kwargs)
    celery.Task = ContextTask
    return celery
```

�������������һ���µ� Celery ����ʹ����Ӧ�������е� broker ������ Flask ���� �������� Celery ���������á�Ȼ�󴴽���һ���������࣬��һ��Ӧ�û����а�װ������ ִ�С�

### ��С������

�������ģ�������һ���� Flask ��ʹ�� Celery ����С����:

```
from flask import Flask

app = Flask(__name__)
app.config.update(
    CELERY_BROKER_URL='redis://localhost:6379',
    CELERY_RESULT_BACKEND='redis://localhost:6379'
)
celery = make_celery(app)


@celery.task()
def add_together(a, b):
    return a + b
```

����������ڿ����ں�̨�����ˣ�

```
>>> result = add_together.delay(23, 42)
>>> result.wait()
65
```

### ���� Celery ����

���ˣ�������Ѿ�������һ��һ��ִ�У����ʧ���ط������ .wait() �������� ���ء�������Ϊ�㻹û������ celery ������������Թ��˷�ʽ���� celery:

```
$ celery -A your_application worker
```

�� your_application �ַ����滻Ϊ�㴴�� celery �����Ӧ�ð���ģ�顣


*? Copyright 2013, Armin Ronacher. Created using [Sphinx](http://sphinx.pocoo.org/).*