# book
API para gerenciamento de uma biblioteca pessoal

## Instalação

1. Clone o repositório:
    ```bash
    git clone https://github.com/seu-usuario/seu-repositorio.git
    ```
2. Navegue até o diretório do projeto:
    ```bash
    cd seu-repositorio
    ```
3. Crie um ambiente virtual:
    ```bash
    python3 -m venv venv
    ```
4. Ative o ambiente virtual:
    - No Windows:
        ```bash
        venv\Scripts\activate
        ```
    - No Unix ou MacOS:
        ```bash
        source venv/bin/activate
        ```
5. Instale as dependências:
    ```bash
    pip install -r requirements.txt
    ```

## Uso

1. Execute o arquivo `main.py`:
    ```bash
    python main.py
    ```

2. Acesse a API em `http://localhost:5000`.

## Endpoints

- `GET /books`: Retorna a lista de todos os livros.
- `POST /books`: Adiciona um novo livro.
- `GET /books/{id}`: Retorna um livro específico pelo ID.
- `PUT /books/{id}`: Atualiza as informações de um livro pelo ID.
- `DELETE /books/{id}`: Remove um livro pelo ID.

## Exemplo de `main.py`

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

books = []

@app.route('/books', methods=['GET'])
def get_books():
    return jsonify(books)

@app.route('/books', methods=['POST'])
def add_book():
    book = request.get_json()
    books.append(book)
    return jsonify(book), 201

@app.route('/books/<int:book_id>', methods=['GET'])
def get_book(book_id):
    book = next((b for b in books if b['id'] == book_id), None)
    if book is None:
        return jsonify({'error': 'Book not found'}), 404
    return jsonify(book)

@app.route('/books/<int:book_id>', methods=['PUT'])
def update_book(book_id):
    book = next((b for b in books if b['id'] == book_id), None)
    if book is None:
        return jsonify({'error': 'Book not found'}), 404
    data = request.get_json()
    book.update(data)
    return jsonify(book)

@app.route('/books/<int:book_id>', methods=['DELETE'])
def delete_book(book_id):
    global books
    books = [b for b in books if b['id'] != book_id]
    return '', 204

if __name__ == '__main__':
    app.run(debug=True)
```

## Licença

Este projeto está licenciado sob a Licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.