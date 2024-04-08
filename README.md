# dev.api
Esse é um projeto exemplo de REST API com Flask e com Flask-RestFull

from flask import Flask, jsonify, request
import json

app = Flask (__name__)

desenvolvedores = [
    {'nome':'Adeildo', 'habilidades':['Python', 'JavaScript']
     },
    {'nome': 'Silva', 'habilidades':['Python', 'HTML']
     },
    {'nome':'Pedro', 'habilidade': ['JavaScript', 'Django']
     },
    {'nome':'Felipe', 'habilidades': ['Python', 'HTML']
     }
]
# devolve um desenvolvedores pelo ID, também altera e delete um desenvolvedor
@app.route('/dev/<int:id>/', methods=['GET', 'PUT', 'DELETE'])
def desenvolvedor (id):
    if request.method == 'GET':
        try:
            response = desenvolvedores[id]
        except IndexError:
            mensagem = 'Desenvolvedor de ID () não existe'.format(id)
            response = {'status':'erro','mensagem':mensagem}
        except Exception:
            mensagem = 'Erro desconhecido. Procure o administrador de API'
            response = {'status':'erro', 'mensagem':mensagem}
        return jsonify(desenvolvedor)
    elif request.method == 'PUT':
        dados = json.loads(request.data)
        desenvolvedores[id] = dados
        return jsonify(dados)
    elif request.method == 'DELETE':
        desenvolvedores.pop(id)
        return jsonify({'status': 'sucesso', 'mensagem': 'registro excluído'})

# Lista todos os desenvolvedores e permite registrar um novo desenvolvedor
@app.route('/dev/', methods=['POST', 'GET'])
def lista_desenvolvedores():
    if request.method == 'POST':
        dados = json.loads(request.data)
        posicao = len(desenvolvedores)
        dados['id'] = posicao
        desenvolvedores.append(dados)
        return jsonify(desenvolvedores[posicao])
    elif request.method == 'GET':
        return jsonify(desenvolvedores)

if __name__ == '__main__':
    app.run(debug=True)
