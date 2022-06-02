# Apuntes fetch

## Realizar peticiones

GET sencillo:
```js
fetch("https://google.com")
```

POST con JSON:
```js
fetch(
    "https://midominio.com/newarticle/",
    {
        method: "POST",
        body: jsonContent
        headers: {
            "Content-Type": "application/json"
        }
    }
)
```

GET con autorización JWT:
```js
fetch(
    "https://midominio.com/newarticle/",
    {
        headers: {
            Authorization: "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.dyt0CoTl4WoVjAHI9Q_CwSKhl6d_9rhM3NrXuJttkao",
            "Content-Type": "application/json"
        }
    }
)
```

POST JSON con autorización JWT:
```js
fetch(
    "https://midominio.com/newarticle/",
    {
        method: "POST",
        body: jsonContent,
        headers: {
            "Content-Type": "application/json",
            Authorization: "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.dyt0CoTl4WoVjAHI9Q_CwSKhl6d_9rhM3NrXuJttkao"
        }
    }
)
```

## Gestionar las respuestas (promesas)

Recuerda que `fetch` es una función asíncrona, así que invariablemente su resultado es una promesa que tendremos que gestionar.

La promesa entregada por `fetch` puede resolver en un error (como cualquier promesa) o en un objeto [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response).

El objeto response disponer de varios métodos para decodificar algunos de los posibles contenidos de la respuesta `.text`, `.json`, `.blob`, `.arrayBuffer`, `.formData`. Todos estos métodos son asíncronos, así que nos proporcionarán una promesa que resolverá en un error o en el tipo de dato correspondiente al método.

Lo anterior quiere decir que si queremos acceder a los datos contenidos en la respuesta de una petición realizada con `fetch` tendremos que gestionar dos promesas:
1. La entregada por `fetch`.
2. La entregada por el método que decodifique el contenido.

### Obteniendo los datos de la respuesta con `then/catch`.
Para el ejemplo suponemos que hemos escrito una fución `fetchDataHandler` que procesará los datos una vez recibidos.
```js
fetch(url)
    .then(
        (response) => {
            return response.json()
        }
    )
    .then(
        (data) => {
            fetchDataHandler(data)
        }
    )
    .catch(
        (error) => {
            console.error(error)
        }
    )
```
Podemos expresar de forma más compacta las funciones flecha empleadas en el ejemplo anterior:
```js
fetch(url)
    .then(response => response.json())
    .then(data => fetchDataHandler(data))
    .catch(error => console.error(error))
```

### Obteniendo los datos de la respuesta con `async/await`.
```js
async function get(url) {
    const response = await fetch(url)
    return await response.json()
}
```