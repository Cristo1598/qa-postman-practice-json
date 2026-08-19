# API Testing Practice — JSONPlaceholder (Ejercicio 2)

Práctica personal de QA / testing de APIs en Postman: CRUD básico sobre la API pública **JSONPlaceholder**, con validaciones automatizadas (tests) y evidencia de cada resultado.

## Objetivo

Practicar el diseño y ejecución de casos de prueba para operaciones CRUD (Create, Read, Update, Delete), incluyendo un caso positivo y uno negativo en la lectura de un recurso.

## Herramienta y API utilizadas

- **Postman** — creación de requests y scripts de test (`pm.test`)
- **JSONPlaceholder** (`https://jsonplaceholder.typicode.com`) — API pública fake REST para pruebas

---

## Casos de prueba

| Método | Endpoint | Objetivo | Resultado |
|---|---|---|---|
| GET | `/posts` | Listar posts | 200 OK |
| GET | `/posts/1` | Post válido | 200 OK |
| GET | `/posts/404` | Post inexistente | 404 Not Found (body vacío `{}`) |
| POST | `/posts` | Crear post | 201 Created |
| PUT | `/posts/1` | Actualizar post | 200 OK |
| DELETE | `/posts/1` | Eliminar post | 200 OK (body vacío `{}`) |

## Scripts de test

**GET `/posts` — listar posts**
```javascript
pm.test("Status code es 200", function () {
    pm.response.to.have.status(200);
});

pm.test("La lista contiene posts", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.length).to.be.above(0);
});
```

**GET `/posts/1` — post válido**
```javascript
pm.test("Status code es 200", function () {
    pm.response.to.have.status(200);
});

pm.test("El id devuelto coincide con 1", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.id).to.eql(1);
});
```

**GET `/posts/404` — post inexistente**
```javascript
pm.test("Status code es 404", function () {
    pm.response.to.have.status(404);
});
```

**POST `/posts` — crear post**

Body enviado:
```json
{
    "title": "Prueba QA",
    "body": "Este es un post de práctica",
    "userId": 1
}
```

Test:
```javascript
pm.test("Status code es 201", function () {
    pm.response.to.have.status(201);
});

pm.test("El titulo coincide con el enviado", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.title).to.eql("Prueba QA");
});
```

**PUT `/posts/1` — actualizar post**

Body enviado:
```json
{
    "id": 1,
    "title": "Título actualizado",
    "body": "Contenido actualizado",
    "userId": 1
}
```

Test:
```javascript
pm.test("Status code es 200", function () {
    pm.response.to.have.status(200);
});

pm.test("El titulo fue actualizado correctamente", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.title).to.eql("Título actualizado");
});
```

**DELETE `/posts/1` — eliminar post**
```javascript
pm.test("Status code es 200", function () {
    pm.response.to.have.status(200);
});
```

---

## Hallazgo importante

A diferencia de otras APIs mock (como reqres.in, que responde **204 No Content** sin body en un DELETE), **JSONPlaceholder responde 200 OK con un body vacío `{}`**. Esto confirma que cada API define su propio contrato de respuesta para operaciones equivalentes, y que no debe asumirse un comportamiento "estándar" sin comprobarlo explícitamente con tests.

---

## Estructura sugerida del repositorio

```
jsonplaceholder-api-practice/
├── README.md
├── collections/
│   └── jsonplaceholder-api.postman_collection.json
└── screenshots/
    ├── 01-get-posts.png
    ├── 02-get-post-valid.png
    ├── 03-get-post-404.png
    ├── 04-post-create-201.png
    ├── 05-put-update-200.png
    └── 06-delete-200.png
```


## Autor

Cristo — práctica personal de QA / API Testing.
