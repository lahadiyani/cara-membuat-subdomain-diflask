# 🚀 Modul Pembelajaran Flask: Subdomain Routing dengan Application Factory dan MVC

Modul ini membahas bagaimana cara membuat dan mengatur **subdomain routing** di Flask menggunakan pola **Application Factory** dan struktur **MVC**. Studi kasus yang digunakan adalah membuat subdomain `chat.cognify.com`.

---

## 🧱 1. Menambahkan Domain dan Subdomain ke Hosts Lokal

Agar `cognify.com` dan `chat.cognify.com` dikenali di lokal, kita harus menambahkan entri ke file `hosts`:

### 🪟 Windows

File: `C:\Windows\System32\drivers\etc\hosts`

Tambahkan baris berikut:

```txt
127.0.0.1   cognify.com
127.0.0.1   chat.cognify.com
```

### 🐧 Linux / MacOS

File: `/etc/hosts`

Tambahkan baris berikut:

```txt
127.0.0.1   cognify.com
127.0.0.1   chat.cognify.com
```

---

## 🔐 2. Buat File `.env`

```env
SERVER_NAME=cognify.com:5000
```

---

## ⚙️ 3. Konfigurasi Flask di `config/config.py`

```python
from dotenv import load_dotenv
import os

load_dotenv()

class Config:
    SERVER_NAME = os.getenv('SERVER_NAME')
    DEBUG = True

class DevelopmentConfig(Config):
    ENV = 'development'
```

---

## 🏗️ 4. Gunakan Application Factory

```python
# app/__init__.py
from flask import Flask
from flask_cors import CORS
from config.config import DevelopmentConfig
from app.blueprints.blueprint import home, chats

def create_app():
    app = Flask(__name__, static_folder='static', template_folder='templates', subdomain_matching=True)
    app.config.from_object(DevelopmentConfig)

    app.register_blueprint(home)
    app.register_blueprint(chats, url_prefix='/', subdomain='chat')
    CORS(app)

    return app
```

---

## 🔗 5. Register Blueprint dengan Subdomain

```python
# app/blueprints/blueprint.py
from flask import Blueprint

home = Blueprint('home', __name__)
chats = Blueprint('chats', __name__, subdomain='chat')
```

---

## 📦 6. Tambahkan File `__init__.py` di Setiap Folder

```python
# app/routes/__init__.py
from app.routes.home import home
from app.routes.chats import chat

# app/controllers/__init__.py
from app.controllers.home import home
from app.controllers.chats import chat_controls
```

---

## 🚀 7. Aktifkan `subdomain_matching`

```python
app = Flask(__name__, static_folder='static', template_folder='templates', subdomain_matching=True)
```

---

## 🧩 8. Load Config dari `DevelopmentConfig`

```python
app.config.from_object(DevelopmentConfig)
```

---

## 🧼 9. Rapi dan Teratur di `create_app()`

Urutan yang disarankan:

1. Inisialisasi Flask
2. Load config
3. Register blueprint
4. Setup ekstensi (CORS, dsb)
5. Return app

---

## 🔍 10. Debug Route Aktif di `main.py`

```python
# main.py
from app import create_app

app = create_app()

for rule in app.url_map.iter_rules():
    print(f"Route: {rule} -> Endpoint: {rule.endpoint}")

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

---

## 🧪 11. Contoh Route Subdomain

```python
# app/routes/chats/chat.py
from flask import request
from app.controllers.chats.chat_controls import Chat
from app.blueprints.blueprint import chats

@chats.route('/')
def index():
    # return f"INI SUBDOMAIN CHAT<br>Host: {request.host}"
    return Chat.chat_base()
```

---

## 📂 12. Wajib: `__init__.py` di Semua Folder Package

Pastikan setiap folder (`routes`, `controllers`, `blueprints`, dsb) ada file `__init__.py` agar bisa diimpor.

---

## ✅ Penutup

Follow me on facebook: [Me](https://web.facebook.com/profile.php?id=100078252237871)
Donate me if you wanna support indie developer: [Saweria](https://saweria.co/hadiani)

---

Happy coding! 🚀
