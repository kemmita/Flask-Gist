1. Python, pip, and Flask must be installed on your server for this to work.
```py
from flask import Flask

app = Flask(__name__)

# This route will return a simple string.
@app.route("/")
def home():
    return 'Working'

# will run on http://127.0.0.1:5000/
app.run(port=5000)
```
