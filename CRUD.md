```py
from flask import Flask, jsonify, request

app = Flask(__name__)

stores = [
    {
        'name': 'first',
        'items': [
            {
                'name': 'Chips',
                'Price': 15.99
            }
        ]
    }
]


# Return a list of elements with no route args
@app.route("/store", methods=['GET'])
def list_stores():
    # You cannot return a py list to json, need to create a dict, assign it a name, then pass your list as the value
    return jsonify({'stores': stores})


# Return a single element using a GET with a single arg passed
@app.route("/store/<string:name>", methods=['GET'])
def get_store(name):
    for i in stores:
        if i['name'] == name:
            # Can return element with jsonify without the dict syntax as it is not a list.
            return jsonify(i)
    return jsonify({'message': 'Store not found'})


# Post to create
@app.route("/store", methods=['POST'])
def create_store():
    # request.get_json() will get the body of the json submitted to the endpoint.
    request_data = request.get_json()
    # Here we get the elements within the json body.
    new_store = {
        'name': request_data['name'],
        'items': []
    }
    stores.append(new_store)
    for i in stores:
        if i['name'] == request_data['name']:
            return jsonify(i)
    return jsonify({'message': 'Problem creating store'})


# Delete an element
@app.route("/store/<string:name>", methods=['DELETE'])
def delete_store(name):
    for i in stores:
        if i['name'] == name:
            stores.remove(i)
            return jsonify({'stores': stores})


app.run()

```
