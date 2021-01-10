```py
from flask import Flask, request
from flask_restful import Resource, Api

app = Flask(__name__)
api = Api(app)
items = [
    {
        'name': 'Soda',
        'price': 1.99,
        'makeup': {
            'style': 'l',
            'w': 'r'
        }
    },
    {
        'name': 'Fat',
        'price': 3.99
    }
]


class Item(Resource):
    def get(self, name):
        if name == 'all':
            return {'items': items}
        item = next(filter(lambda x: x['name'] == name, items), None)
        return item, 200 if item else 404

    def post(self, name):
        data = request.get_json()
        items.append({
            'name': data['name'],
            'price': data['price']
        })
        for i in items:
            if i['name'] == data['name']:
                return i

    def delete(self, name):
        for i in items:
            if i['name'] == name:
                items.remove(i)

    def put(self, name):
        data = request.get_json()
        for i in items:
            if i['name'] == name:
                i['name'] = data['name']


api.add_resource(Item, '/item/<string:name>')

app.run(debug=True)
```
