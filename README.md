# AMAZONA

Si bien ya tengo login y pagos con PayPal funcionando (secciones 7 y 8), hago un repaso de lo realizado en las primeras seis secciones del curso.

## BackEnd Sección 1 a 6
### package.json
```json
{  
  "name": "backend",
  "type": "module",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```
Nada destacable para observar, sólo que se usa el framework Express.js.

### data.js
```js
const data = {
    products: [
      {
        _id: '1',
        name: 'Nike Slim shirt',
        slug: 'nike-slim-shirt',
        category: 'Shirts',
        image: '/images/p1.jpg', // 679px × 829px
        price: 120,
        countInStock: 10,
        brand: 'Nike',
        rating: 4.5,
        numReviews: 10,
        description: 'high quality shirt',
      },
      {
        _id: '2',
        name: 'Adidas Fit Shirt',
        slug: 'adidas-fit-shirt',
        category: 'Shirts',
        image: '/images/p2.jpg',
        price: 250,
        countInStock: 20,
        brand: 'Adidas',
        rating: 4.0,
        numReviews: 10,
        description: 'high quality product',
      },
      {
        _id: '3',
        name: 'Nike Slim Pant',
        slug: 'nike-slim-pant',
        category: 'Pants',
        image: '/images/p3.jpg',
        price: 25,
        countInStock: 15,
        brand: 'Nike',
        rating: 4.5,
        numReviews: 14,
        description: 'high quality product',
      },
      {
        _id: '4',
        name: 'Adidas Fit Pant',
        slug: 'adidas-fit-pant',
        category: 'Pants',
        image: '/images/p4.jpg',
        price: 65,
        countInStock: 5,
        brand: 'Puma',
        rating: 4.5,
        numReviews: 10,
        description: 'high quality product',
      },
    ],
  };
  export default data;
```
El object data contiene en su interior una lista de nombre products. Esa lista products contiene objetos con los siguientes atributos:
   - _id = identificador único del producto (ejemplo: 1).
   - name = nombre del producto (ejemplo: Nike Slim shirt).
   - slug = slug del producto (ejemplo: nike-slim-shirt).
   - category = la categoría a la cual pertenece (ejemplo: Shirts).
   - image = la url donde se encuentra la imagen (ejemplo: '/images/p1.jpg').
   - price = el precio del producto (ejemplo: 120).
   - countInStock = la cantidad que hay en stock del producto (ejemplo: 10).
   - rating = la puntuación que recibió el producto (ejemplo: 4.5).
   - numReviews = la cantidad de reviews escritar que recibió (ejemplo: 10).
   - description = una descripción del producto (ejemplo: 'high quality shirt')
 
     
### server.js
```node
import express from "express";
import data from "./data.js";

const app = express();

app.get("/api/products", (req, res) => {
  res.send(data.products);
});

app.get("/api/products/slug/:slug", (req, res) => {
  const product = data.products.find((x) => x.slug === req.params.slug);
  if (product) {
    res.send(product);
  } else {
    res.status(404).send({ message: "Product Not Found" });
  }
});

app.get("/api/products/:id", (req, res) => {
  const product = data.products.find((x) => x._id === req.params.id);
  if (product) {
    res.send(product);
  } else {
    res.status(404).send({ message: "Product Not Found" });
  }
});

const port = process.env.PORT || 5000;
app.listen(port, () => {
  console.log(`serve at http://localhost:${port}`);
});
```

Se importa el framework Express.js y, seguidamente, se importa el objeto data de data.js.
```node
import express from "express";
import data from "./data.js";
```

Creamos la aplicación Express: 
```node
const app = express();
```

Definimos el puerto que usará la aplicación. Tomamos el que está en el process.env.PORT o sino usamos el puerto 5000. Por último, ponemos a la aplicación a escuchar el puerto, llamando a la función listen, pasando como argumento el puerto y una callback.

```node
const port = process.env.PORT || 5000;
app.listen(port, () => {
  console.log(`serve at http://localhost:${port}`);
});
```

En el medio de la creación de la aplicación Express y la escucha del puerto, vamos a definir los endpoints.

/api/products GET 
```node
app.get("/api/products", (req, res) => {
  res.send(data.products);
});
```

Quien ingrese a, por ejemplo, localhost:5000/api/products recibirá la lista products.

/api/products/slug/:slug GET
```node
app.get("/api/products/slug/:slug", (req, res) => {
  const product = data.products.find((x) => x.slug === req.params.slug);
  if (product) {
    res.send(product);
  } else {
    res.status(404).send({ message: "Product Not Found" });
  }
});
```

Quien ingrese a, por ejemplo, localhost:5000/api/products/slug/nike-slim-shirt recibirá el product siguiente:
```js
{
     _id: '1',
     name: 'Nike Slim shirt',
     slug: 'nike-slim-shirt',
     category: 'Shirts',
     image: '/images/p1.jpg', // 679px × 829px
     price: 120,
     countInStock: 10,
     brand: 'Nike',
     rating: 4.5,
     numReviews: 10,
     description: 'high quality shirt',
}
```

Lo que se hace es bien simple. Se usa el método built-in find de products. Find recibe como parámetro una callback. El parámetro de entrada de la misma va a ser cada product de products y si la condición del retorno se cumple, se retornará el product. La condición es que el slug del product de products sea el mismo que el slug que se pasó como parámetro en la url (se obtiene mediante req.params.slug. Por tanto, si product está inicializado, quiere decir que la condición se cumplió y se hará un send del product: res.send(product). Caso contrario, se hará un send pero de un error 404 advirtiendo que el producto no fue encontrado: res.status(404).send({ message: "Product Not Found" }).

Notese que tanto en el primer send como en el segundo estamos enviando un object {}. En el primero está implícito el res.status(200).send(product). Notese además que en el send del primer endpoint enviamos una lista de objects.

/api/products/:id GET
Mismo código de antes, nada más que ahora se busca por id 

## FrontEnd Sección 1 a 6
### package.json
```json
{
  "name": "frontend",
  "proxy": "http://localhost:5000",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^5.17.0",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^13.5.0",
    "axios": "^1.5.0",
    "bootstrap": "^5.3.1",
    "react": "^18.2.0",
    "react-bootstrap": "^2.8.0",
    "react-dom": "^18.2.0",
    "react-helmet-async": "^1.3.0",
    "react-router-bootstrap": "^0.26.2",
    "react-router-dom": "^6.15.0",
    "react-scripts": "5.0.1",
    "web-vitals": "^2.1.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

Notemos que se setea un proxy, con la url apuntando al server del backend: "proxy": "http://localhost:5000".
Después en dependencies nos encontramos con que se instaló:
   - bootstrap. Proporciona estilos y componentes predefinidos para la creación de interfaces web responsivas y atractivas.
   - react-bootstrap. Biblioteca que integra Bootstrap con React, permitiendo el uso de componentes Bootstrap como componentes React en tu aplicación.
   - react-router-bootstrap. Paquete que facilita la integración de React-Router con React-Bootstrap, permitiendo la navegación entre rutas de una aplicación web.
   - react-router-dom. Proporciona herramientas para la gestión de enrutamiento en aplicaciones React, permitiendo la navegación entre diferentes vistas y componentes.
   - react-helmet-async. Hasta ahora lo usamos para cambiar el title de la pestaña según la página a la que se accede. Permite la gestión asincrónica de las etiquetas meta y de encabezado del documento HTML, útil para optimizar el SEO y la experiencia del usuario.
   - axios. Biblioteca para realizar solicitudes HTTP desde una aplicación React, facilitando la comunicación con servidores y la gestión de datos externos, como API REST.

### public/images
Dentro de este directorio se encuentran p1.jpg, p2.jpg, p3.jpg y p4.jpg. Estas son las imágenes que se usan para los productos.

### public/index.html
Lo único que se puede comentar es que incluí esto:
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@5.15.4/css/all.min.css" />

### src/index.js
```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

import "bootstrap/dist/css/bootstrap.min.css";
import { HelmetProvider } from "react-helmet-async";
import { StoreProvider } from "./Store";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
    <StoreProvider>
      <HelmetProvider>
        <App />
      </HelmetProvider>
    </StoreProvider>
);
```

El componente StoreProvider es un provider y, como tal, permite el acceso a datos compartidos entre distintos componentes que vivan dentro de App. Al mismo tiempo, envolver la App entre el HelmetProvider permite que desde cualquier parte del componente App se pueda acceder a funciones como las que permiten cambiar el title de la página.
Originalmente en los imports teníamos esto, que es lo que trae por defecto cualquier app creada con Create React App:
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
```
Luego le adicionamos esto:
```jsx
import "bootstrap/dist/css/bootstrap.min.css";
import { HelmetProvider } from "react-helmet-async";
import { StoreProvider } from "./Store";
```

### src/index.css
   -

### src/Store.js
```jsx
import { createContext, useReducer } from "react";

export const Store = createContext();

const initialState = {
  cart: {
    cartItems: JSON.parse(localStorage.getItem("cartItems")) ?? []
  },
};

function reducer(state, action) {
  switch (action.type) {
    case "CART_ADD_ITEM":
      const newItem = action.payload;
      const existItem = state.cart.cartItems.find(
        (item) => item._id === newItem._id
      );

      let cartItems;

      if (existItem) {
        cartItems = state.cart.cartItems.map((item) =>
          item._id === existItem._id ? newItem : item
        );
      } else {
        cartItems = [...state.cart.cartItems, newItem];
      }
      localStorage.setItem("cartItems", JSON.stringify(cartItems));
      return { ...state, cart: { ...state.cart, cartItems } };
    case "CART_REMOVE_ITEM": {
      const cartItems = state.cart.cartItems.filter(
        (item) => item._id !== action.payload._id
      );
      localStorage.setItem("cartItems", JSON.stringify(cartItems));
      return { ...state, cart: { ...state.cart, cartItems } };
    }
    default:
      return state;
  }
}

export function StoreProvider(props) {
  const [state, dispatch] = useReducer(reducer, initialState);
  const value = { state, dispatch };
  return <Store.Provider value={value}>{props.children}</Store.Provider>;
}
```

El código anterior define un context para que todos los componentes que sean hijos del StoreProvider puedan acceder al value (que contiene un state y un dispatch). En index.js tenemos esto:
```jsx
root.render(
    <StoreProvider>
      <HelmetProvider>
        <App />
      </HelmetProvider>
    </StoreProvider>
);
```

Es decir, todo componente de la app va a poder acceder al value. Fijate esta parte:
```jsx
<Store.Provider value={value}>{props.children}</Store.Provider>;
```
Claramente el children vendría a ser el componente <HelmetProvider>, que a su vez tiene como children a <App>. Gracias a la LDC anterior, el children podrá acceder a un value que contiene un state y un dispatch, porque value = { state, dispatch}. El state hace referencia al estado del carrito de compras y el dispatch al método que permitirá actualizarlo. Todo ello gracias a que se usa el siguiente hook: const [state, dispatch] = useReducer(reducer, initialState).
Entonces StoreProvider usa un reducer pero además crea un context con export const Store = createContext(); para que después el reducer pueda ser accedido por todos los componentes hijos del StoreProvider. Un reducer sirve para actualizar el estado de un componente. Un context para pasar el estado a componentes hijos.
Para que un componente hijo del StoreProvider pueda usar el contexto, simplemente deberá importar el StoreProvider y hacer uso de la función useContext(), tal como se ve en este ejemplo:

```jsx
import { Store } from "./Store";

function App() {
  const { state } = useContext(Store);
```

Ahora bien. Hagamos un stringify de state para ver su contenido:
 ```json
"cart": {
 "cartItems": [
   {
     "_id": "1",
     "name": "Nike Slim shirt",
     "slug": "nike-slim-shirt",
     "category": "Shirts",
     "image": "/images/p1.jpg",
     "price": 120,
     "countInStock": 10,
     "brand": "Nike",
     "rating": 4.5,
     "numReviews": 10,
     "description": "high quality shirt",
     "quantity": 3
   },
   {
     "_id": "2",
     "name": "Adidas Fit Shirt",
     "slug": "adidas-fit-shirt",
     "category": "Shirts",
     "image": "/images/p2.jpg",
     "price": 250,
     "countInStock": 20,
     "brand": "Adidas",
     "rating": 4,
     "numReviews": 10,
     "description": "high quality product",
     "quantity": 1
   }
 ]
}
```
Notemos que el "state" sólo almacena "cart". Dentro del objeto "cart" tenemos un único atributo, que por ahora es el array llamado "cartItems". Dentro de ese array vemos que hay dos objetos cargados. El "quantity" del primero es 3 y el "quantity" del segundo es 1. Eso hará que se muestre el número 4 encima del ícono del carrito de compras.
El reducer tiene dos "funciones": CART_ADD_ITEM y CART_REMOVE_ITEM. Veamoslas:

CART_ADD_ITEM
Cuando se hace clic en "Add to Cart" o se incrementa en una unidad la cantidad a comprar de un producto, el action.type que se usa es CART_ADD_ITEM y en el action.payload lo único que se pasa es el objeto del producto con el quantity que corresponda.
```jsx
const newItem = action.payload;
const existItem = state.cart.cartItems.find(
    (item) => item._id === newItem._id
);
```

Si el producto ya está presente en cartItems, se lo debe reemplazar por el nuevo (ya que es el que tiene el quantity actualizado):
```jsx
cartItems = state.cart.cartItems.map((item) =>
  item._id === existItem._id ? newItem : item
);
```

Si no está presente, se crea una copia del array cartItems y se le añade el newItem:
```jsx
cartItems = [...state.cart.cartItems, newItem];
```

Se guarda en el localStorage los cambios 
```jsx
localStorage.setItem("cartItems", JSON.stringify(cartItems));
```
Y se procede a retornar el nuevo estado:
```jsx
return { ...state, cart: { ...state.cart, cartItems } };
```

Nótese que se utiliza el spread operator nuevamente. Se retorna una copia del objeto state, pero con un nuevo estado para cart. El objeto cart es una copia de state.cart pero con cartItems actualizado.

## src/App.js
```jsx
import { BrowserRouter, Route, Link, Routes } from "react-router-dom";
import HomeScreen from "./screens/HomeScreen";
import ProductScreen from "./screens/ProductScreen";
import Navbar from "react-bootstrap/Navbar";
import Container from "react-bootstrap/Container";
import Nav from "react-bootstrap/Nav";
import { LinkContainer } from "react-router-bootstrap";
import { Badge } from "react-bootstrap";
import { useContext } from "react";
import CartScreen from "./screens/CartScreen";
import SigninScreen from "./screens/SigninScreen";
import { Store } from "./Store";

function App() {
  const { state } = useContext(Store);
  const { cart } = state;

  return (
    <BrowserRouter>
      <div className="d-flex flex-column site-container">
        <header>
          <Navbar bg="dark" variant="dark">
            <Container>
              <LinkContainer to="/">
                <Navbar.Brand>Amazona</Navbar.Brand>
              </LinkContainer>
              <Nav className="me-auto">
                <Link to="/cart" className="nav-link">
                  Cart
                  {cart.cartItems.length > 0 && (
                    <Badge pill bg="danger">
                      {cart.cartItems.reduce((a,c) => a + c.quantity, 0)}
                    </Badge>
                  )}
                </Link>
              </Nav>
            </Container>
          </Navbar>
        </header>
        <main>
          <Container className="mt-3">
            <Routes>
              <Route path="/product/:slug" element={<ProductScreen />} />
              <Route path="/cart" element={<CartScreen />} />
              <Route path="/signin" element={<SigninScreen />} />
              <Route path="/" element={<HomeScreen />} />
            </Routes>
          </Container>
        </main>
        <footer>
          <div className="text-center">All rights reserved</div>
        </footer>
      </div>
    </BrowserRouter>
  );
}

export default App;
```

Nótese que se importa el provider con import { Store } from "./Store" y luego se hace:
```
const { state } = useContext(Store);
const { cart } = state;
```

Lo anterior permite que desde el componente se pueda acceder al objeto cart y todo su contenido. Básicamente tenemos un header, un main y un footer. El header contiene a la Navbar. El main contiene un Container que a su vez contiene las Routes. Y el footer simplemente muestra una leyenda.
- header 
```jsx
<Navbar bg="dark" variant="dark">
 <Container>
   <LinkContainer to="/">
     <Navbar.Brand>Amazona</Navbar.Brand>
   </LinkContainer>
   <Nav className="me-auto">
     <Link to="/cart" className="nav-link">
       Cart
       {cart.cartItems.length > 0 && (
         <Badge pill bg="danger">
           {cart.cartItems.reduce((a,c) => a + c.quantity, 0)}
         </Badge>
       )}
     </Link>
   </Nav>
 </Container>
</Navbar>
```

En la Navbar podemos setear el bg y el variant con el valor "light" y eso nos daría este resultado:

![navbar-light](https://remnote-user-data.s3.amazonaws.com/_Hed87-IPTBELhxXnh4A7v3Txxff0BQpeTE3wAxgxOuQ18bjgWNxeFwtxViDSKbPMQNHsZz1GNPHJQR7pwE20c4uylaxBRezcjIiowgpm2ipCg87_UbweilsSA80-o5y.png)

Actualmente lo tenemos en "dark" y se ve así:
![navbar-dark](https://remnote-user-data.s3.amazonaws.com/gAw-Yy40RJCHhk_b0dluv0MDPHGyBs1CE7psIJ95mrZG1SZVHAOlWvYEDKg9pqGj5JW3sWH4uM-AqQEKKOJce4IwBpw3TTqLHnRhPqssKqvjXbfHmeK1N3MVw7YncyqT.png)

Dentro de la Navbar tenemos un Container y luego dos elementos: LinkContainer y Nav. 
- En LinkContainer en la propiedad "to" tenemos "/" y dentro del elemento tenemos esto <Navbar.Brand>Amazona</Navbar.Brand>. Aquello permite que si uno hace clic en el nombre de la empresa "Amazona", te redirija al home.
- Luego tenemos el Nav con className "me-auto" (que permite tomar todo el ancho de pantalla) y contiene un link a "/cart", o sea, la página del carrito de compras. Se muestra la leyenda Cart acompañada de un Badge que muestra el número de items en el carrito. Se usa una función de ES6:
```jsx
{cart.cartItems.reduce((a,c) => a + c.quantity, 0)}
```

Sobre el array cartItems, se llama a la función reduce. Esta recibe como argumento un callback y un valor inicial para el acumulador. En la callback tenemos las entradas a (acumulador) y c (el current item). El retorno será la suma de las cantidades de todos los productos.

### main
```jsx
   <Container className="mt-3">
    <Routes>
      <Route path="/product/:slug" element={<ProductScreen />} />
      <Route path="/cart" element={<CartScreen />} />
      <Route path="/" element={<HomeScreen />} />
    </Routes>
  </Container>
```

El container tiene un margin-top de 3px. En su interior tenemos a Routes y dentro del componente cada una de las Route, con su path y componente asociado.


### src/screens/
#### HomeScreen.jsx
![homescreen](https://remnote-user-data.s3.amazonaws.com/caUlbBrb01u4dVsRMLrvuN4xMGenJm1aIyM3hGG8JG9H-P6rhMNPRYQgqjp2rM3U8zbyFaGrHjZqWZro4j9qelw0NxdAJeJpRY3TIa0jlZ_bC6ZDn028mMxxxU4fiKB2.png)

```jsx
import { useEffect, useReducer } from "react";
import axios from "axios";
import Row from "react-bootstrap/Row";
import Col from "react-bootstrap/Col";
import { Helmet } from "react-helmet-async";
import { getError } from "../utils";
import Product from "../components/Product";
import LoadingBox from "../components/LoadingBox";
import MessageBox from "../components/MessageBox";

const reducer = (state, action) => {
  switch (action.type) {
    case "FETCH_REQUEST":
      return { ...state, loading: true };
    case "FETCH_SUCCESS":
      return { ...state, products: action.payload, loading: false };
    case "FETCH_FAIL":
      return { ...state, error: action.payload, loading: false };
    default:
      return state;
  }
};

const initialState = {
  products: [],
  loading: true,
  error: "",  
}

export default function HomeScreen() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const { products, loading, error } = state;

  useEffect(() => {
    const fetchData = async () => {
      dispatch({ type: "FETCH_REQUEST" });
      try {
        const result = await axios.get("/api/products");
        dispatch({ type: "FETCH_SUCCESS", payload: result.data });
      } catch (err) {
        dispatch({ type: "FETCH_FAIL", payload: getError(err)});
      }
    };
    fetchData();
  }, []);

  return (
    <div>
      <Helmet>
        <title>Amazona</title>
      </Helmet>
      <h1>Featured Products</h1>
      <div className="products">
        {loading ? (
          <div>
            <LoadingBox />
          </div>
        ) : error ? (
          <MessageBox variant="danger">{error}</MessageBox>
        ) : (
          <Row>
            {products.map((product) => (
              <Col key={product.slug} sm={6} md={4} lg={3} className="mb-3">
                <Product product={product}></Product>
              </Col>
            ))}
          </Row>
        )}
      </div>
    </div>
  );
}
```

Tenemos un componente funcional llamado HomeScreen, que se encargará de mostrar productos en la landing page de Amazona. El componente contiene un Reducer cuyo estado inicial es el siguiente:
```jsx
const initialState = {
  products: [],
  loading: true,
  error: "",
};
```

Tiene un array de productos, un valor lógico para indicar si el componente está cargando o no y un string para almacenar un mensaje de error si hubiera. Acá está la definición del reducer:

```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case "FETCH_REQUEST":
      return { ...state, loading: true };
    case "FETCH_SUCCESS":
      return { ...state, products: action.payload, loading: false};
    case "FETCH_FAIL":
      return { ...state, error: action.payload, loading: false };
    default:
      return state;
  }
};
```

Hay 3 action.type posibles:
    - FETCH_REQUEST: se hace el dispatch de este action.type justo antes de hacer un GET a la API del server para que traiga los productos. Lo único que hace este action.type es cambiar el estado de loading a True, para que el componente sepa que se van a estar cargando productos y que una operación asincrónica está en camino.
    - FETCH_SUCCESS: se hace el dispatch cuando ya se obtuvieron con éxito los productos desde el servidor. El loading pasa a valer Falso y products pasa a valer lo que tiene el action.payload.
    - FETCH_FAIL: se hace el dispatch cuando el server respondió con un mensaje de error. En error se guardar el payload y, obviamente, loading pasa a valer Falso.

Apenas comienza la definición del componente, encontramos esto:
```jsx  
const [state, dispatch] = useReducer(reducer, initialState);
const { products, loading, error } = state;

useEffect(() => {
  const fetchData = async () => {
    dispatch({ type: "FETCH_REQUEST" });
    try {
      const result = await axios.get("/api/products");
      dispatch({ type: "FETCH_SUCCESS", payload: result.data });
    } catch (err) {
      dispatch({ type: "FETCH_FAIL", payload: getError(err) });
    }
  };
  fetchData();
}, []);
```

Se indica que se hará uso de un reducer con la función reducer e initialState que vimos antes. Además, se hace un destructuring del state para tener a mano products, loading y error, que serán usados en el componente.
En cuanto al useEffect, dado que el segundo argumento es una lista vacía, se ejecutará sólo una única vez su callback. El mismo tiene la definición de una función asíncrona que será ejecutada gracias al llamado fetchData();. ¿Por qué se define la función fetchData? Porque la llamada al endpoint del back es asíncrona, por tanto, se debe usar el async ... await. 
Lo primero que hace fetchData es hacer un dispatch con un objeto con el atributo type igual a "FETCH_REQUEST". Luego se hace un const result = await axios.get("/api/products") para, finalmente, llamar al dispatch pasando en type el string "FETCH_SUCCESS" y en el payload result.data.
Si algo va mal, se hace un dispatch con FETCH_FAIL y un payload con el mensaje de error. Cuando se está renderizando el mismo, el título de la pestaña del navegador web pasa a ser "Amazona", puesto que hay un title definido dentro de un componente Helmet.

```jsx
<Helmet>
   <title>Amazona</title>
</Helmet>
```

Luego tenemos un <div className="products"></div>, dentro del cual se mostrará una de tres cosas posibles:
- Un componente <LoadingBox />. No es más que un Spinner para indicar un proceso de carga (se muestra sólo si loading está en True).
- Un componente <MessageBox> que se muestra si y sólo si se produce un error.
- Un componente <Row></Row> en cuyo interior se crearán tantas  <Col></Col> como productos haya y en el interior de la columna se mostrará el producto. Si el tamaño de pantalla es small, se mostrarán dos productos por fila, tres si es medium y cuatro si es large.

El código se reduce a esto:
```jsx
<div className="products">
 {loading ? (
   <div>
     <LoadingBox />
   </div>
 ) : error ? (
   <MessageBox variant="danger">{error}</MessageBox>
 ) : (
   <Row>
     {products.map((product) => (
       <Col key={product.slug} sm={6} md={4} lg={3} className="mb-3">
         <Product product={product}></Product>
       </Col>
     ))}
   </Row>
 )}
</div>
```

 Nótese que el componente de producto recibe como props información sobre el mismo: <Product product={product}></Product>.

#### ProductScreen.jsx
![productscreen](https://remnote-user-data.s3.amazonaws.com/K-uUE1WbBTID94FNbWFXFrtbL3BTKZFSy-w2SDFwGsVPPvZNLFDNTaUlza5B3bVlc3KKoY_pCxCZuwhQJAO37FASJ5b-YH1VBw8SgxNpO46CnN59XoxgLd_LDE9e64pU.png)

```jsx
import { useContext, useEffect, useReducer } from "react";
import { useNavigate, useParams } from "react-router-dom";
import axios from "axios";
import { Badge, Button, Card, Col, ListGroup, Row } from "react-bootstrap";
import Rating from "../components/Rating";
import { Helmet } from "react-helmet-async";
import { getError } from "../utils";
import LoadingBox from "../components/LoadingBox";
import MessageBox from "../components/MessageBox";
import { Store } from "../Store";

const reducer = (state, action) => {
  switch (action.type) {
    case "FETCH_REQUEST":
      return { ...state, loading: true };
    case "FETCH_SUCCESS":
      return { ...state, product: action.payload, loading: false };
    case "FETCH_FAIL":
      return { ...state, error: action.payload, loading: false };
    default:
      return state;
  }
};

const initialState = {
  product: {},
  loading: true,
  error: "",
};

const ProductScreen = () => {
  const navigate = useNavigate();
  const params = useParams();
  const { slug } = params;

  const [state, dispatch] = useReducer(reducer, initialState);
  const { product, loading, error } = state;

  useEffect(() => {
    const fetchData = async () => {
      dispatch({ type: "FETCH_REQUEST" });
      try {
        const result = await axios.get(`/api/products/slug/${slug}`);
        dispatch({ type: "FETCH_SUCCESS", payload: result.data });
      } catch (err) {
        dispatch({ type: "FETCH_FAIL", payload: getError(err) });
      }
    };
    fetchData();
  }, [slug]);

  const { state: ctxState, dispatch: ctxDispatch } = useContext(Store);
  const { cart } = ctxState;
  
  const addToCartHandler = async () => {
    const existItem = cart.cartItems.find((p) => p._id === product._id);
    const quantity = existItem ? existItem.quantity + 1 : 1;
    const { data } = await axios.get(`/api/products/${product._id}`);
    if (data.countInStock < quantity) {
      window.alert("Sorry. Product is out of stock");
      return;
    }

    ctxDispatch({ type: "CART_ADD_ITEM", payload: { ...product, quantity } });
    navigate("/cart");
  };

  return loading ? (
    <div>
      <LoadingBox />
    </div>
  ) : error ? (
    <MessageBox variant="danger">{error}</MessageBox>
  ) : (
    <div>
      <Row>
        <Col md={6}>
          <img className="img=large" src={product.image} alt={product.name} />
        </Col>
        <Col md={3}>
          <ListGroup variant="flush">
            <ListGroup.Item>
              <Helmet>
                <title>{product.name}</title>
              </Helmet>
              <h1>{product.name}</h1>
            </ListGroup.Item>
            <ListGroup.Item>
              <Rating
                rating={product.rating}
                numReviews={product.numReviews}
              ></Rating>
            </ListGroup.Item>
            <ListGroup.Item>Price: $ {product.price}</ListGroup.Item>
            <ListGroup.Item>
              Description: <p>{product.description}</p>
            </ListGroup.Item>
          </ListGroup>
        </Col>
        <Col md={3}>
          <Card>
            <Card.Body>
              <ListGroup variant="flush">
                <ListGroup.Item>
                  <Row>
                    <Col>Price:</Col>
                    <Col>$ {product.price}</Col>
                  </Row>
                </ListGroup.Item>
                <ListGroup.Item>
                  <Row>
                    <Col>Status:</Col>
                    <Col>
                      {product.countInStock > 0 ? (
                        <Badge bg="success">In Stock</Badge>
                      ) : (
                        <Badge bg="danger">Unavailable</Badge>
                      )}
                    </Col>
                  </Row>
                </ListGroup.Item>

                {product.countInStock > 0 && (
                  <ListGroup.Item>
                    <div className="d-grid">
                      <Button onClick={addToCartHandler} variant="primary">
                        Add to Cart
                      </Button>
                    </div>
                  </ListGroup.Item>
                )}
              </ListGroup>
            </Card.Body>
          </Card>
        </Col>
      </Row>
    </div>
  );
};
export default ProductScreen;
```

El módulo incorpora un reducer. Es similar al reducer del HomeScreen, pero este en lugar de gestionar productos, gestiona un único producto.
```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case "FETCH_REQUEST":
      return { ...state, loading: true };
    case "FETCH_SUCCESS":
      return { ...state, product: action.payload, loading: false };
    case "FETCH_FAIL":
      return { ...state, error: action.payload, loading: false };
    default:
      return state;
  }
};

const initialState = {
  product: {},
  loading: true,
  error: "",
};

const ProductScreen = () => {
  const navigate = useNavigate();
  const params = useParams();
  const { slug } = params;

  const [state, dispatch] = useReducer(reducer, initialState);
  const { product, loading, error } = state;
  
  useEffect(() => {
    const fetchData = async () => {
      dispatch({ type: "FETCH_REQUEST" });
      try {
        const result = await axios.get(`/api/products/slug/${slug}`);
        dispatch({ type: "FETCH_SUCCESS", payload: result.data });
      } catch (err) {
        dispatch({ type: "FETCH_FAIL", payload: getError(err) });
      }
    };    
    fetchData();
  }, [slug]);
```

Notese esta parte:
```jsx
const params = useParams();
const { slug } = params;
```

En slug se obtiene el slug de la URL. Esto es posible gracias a que en Product.jsx se agrega esto:
```jsx
<Link to={`product/${product.slug}`}>
  <Card.Title>{product.name}</Card.Title>
</Link>
```

Ese product.slug es el que se obtiene en slug acá: const { slug } = params;. Por otra parte, con la función fetchData() se trae información del producto.
Luego vemos que se usa el StoreProvider, de forma tal que quede accesible el carrito de compras y el dispatch para el componente ProductScreen.

```jsx
const { 
    state: ctxState, dispatch: ctxDispatch 
} = useContext(Store);
const { cart } = ctxState;
```

¿Por qué queremos que ProductScreen tenga acceso al carrito? ¿Y por qué queremos que pueda usar el dispatch? Como se ve en la imagen y en el código, tenemos un botón "Add to cart". En el StoreProvider tenemos la función reducer que define un action.type "CART_ADD_ITEM". Eso vamos a querer utilizar.
```jsx
const addToCartHandler = async () => {
 const existItem = cart.cartItems.find(
     (p) => p._id === product._id
 );
 const quantity = existItem ? existItem.quantity + 1 : 1;
 const { 
     data 
 } = await axios.get(`/api/products/${product._id}`);
 if (quantity > data.countInStock) {
   window.alert("Sorry. Product is out of stock");
   return;
 }

 ctxDispatch({ 
     type: "CART_ADD_ITEM", 
     payload: { ...product, quantity } 
 });
 navigate("/cart");
};
```

De hecho, cuando se hace clic en el botón "Add to cart", se llama al método asíncrono addToCartHandler. Primero se chequea si el producto a existe en el carrito. Luego en quantity se almacena 1, a menos que exista el producto, en cuyo caso se almacenará la cantidad que ya había de ese producto más una unidad extra. Luego se hace un llamado a la API para consultar si la cantidad que se desea agregar al carrito es mayor que la cantidad en stock del producto. De ser así, se muestra un mensaje de error y se cancela el proceso. De lo contrario, se hace un dispatch al StoreProvider de tipo "CART_ADD_ITEM" y pasando un objeto que es una copia del producto pero con el nuevo valor de cantidad. Por último, se redirige al usuario a la pantalla del carrito de compras.

#### CartScreen.jsx
![cartscreen](https://remnote-user-data.s3.amazonaws.com/KCMpi87E-JdYIp2oQJBs0w6FjkiKRAZ0e7GGlDsD9xHhC1nYBVFvrln_onXml6xkiFhNWxCQCbvRfE1YUvBeJ7pE0NUCsc2aIBrJ0Yd9bPk0LoJRE4MfLOAHpOmB1PCk.png)

```jsx
import { useContext } from "react";
import { Link, useNavigate } from "react-router-dom";
import axios from "axios";
import { Button, Card, Col, ListGroup, Row } from "react-bootstrap";
import { Helmet } from "react-helmet-async";
import MessageBox from "../components/MessageBox";
import { Store } from "../Store";

export default function CartScreen() {
  const navigate = useNavigate();
  const { state, dispatch: ctxDispatch } = useContext(Store);

  const {
    cart: { cartItems },
  } = state;

  const updateCartHandler = async (product, quantity) => {
    const { data } = await axios.get(`/api/products/${product._id}`);

    if (quantity > data.countInStock) {
      window.alert("Sorry. Product is out of stock");
      return;
    }

    ctxDispatch({ type: "CART_ADD_ITEM", payload: { ...product, quantity } });
  };

  const removeProductHandler = (product) => {
    ctxDispatch({ type: "CART_REMOVE_ITEM", payload: product });
  }

  return (
    <div>
      <Helmet>
        <title>Shopping Cart</title>
      </Helmet>
      <h1>Shopping Cart</h1>
      <Row>
        <Col md={8}>
          {cartItems.length === 0 ? (
            <MessageBox>
              Cart is empty. <Link to="/">Go Shopping</Link>
            </MessageBox>
          ) : (
            <ListGroup>
              {cartItems.map((product) => (
                <ListGroup.Item key={product._id}>
                  <Row className="align-items-center">
                    <Col md={4}>
                      <img
                        src={product.image}
                        alt={product.name}
                        className="img-fluid rounded img-thumbnail"
                      />
                      <Link to={`/product/${product.slug}`}>
                        {product.name}
                      </Link>
                    </Col>
                    <Col md={3}>
                      <Button
                        variant="light"
                        onClick={() =>
                          updateCartHandler(product, product.quantity - 1)
                        }
                        disabled={product.quantity === 1}
                      >
                        <i className="fas fa-minus-circle"></i>
                      </Button>
                      <span>{product.quantity}</span>
                      <Button
                        variant="light"
                        onClick={() =>
                          updateCartHandler(product, product.quantity + 1)
                        }
                        disabled={product.quantity === product.countInStock}
                      >
                        <i className="fas fa-plus-circle"></i>
                      </Button>
                    </Col>
                    <Col md={3}>${product.price}</Col>
                    <Col md={2}>
                      <Button
                        onClick={() => removeProductHandler(product)}
                        variant="light"
                      >
                        <i className="fas fa-trash"></i>
                      </Button>
                    </Col>
                  </Row>
                </ListGroup.Item>
              ))}
            </ListGroup>
          )}
        </Col>
        <Col md={4}>
          <Card>
            <Card.Body>
              <ListGroup variant="flush">
                <ListGroup.Item>
                  <h3>
                    Subtotal ({cartItems.reduce((a, c) => a + c.quantity, 0)}{" "}
                    items) : ${" "}
                    {cartItems.reduce((a, c) => a + c.price * c.quantity, 0)})
                  </h3>
                </ListGroup.Item>
                <ListGroup.Item>
                  <div className="d-grid">
                    <Button
                      type="button"
                      variant="primary"
                      onClick={checkoutHandler}
                      disabled={cartItems.length === 0}
                    >
                      Proceed to Checkout
                    </Button>
                  </div>
                </ListGroup.Item>
              </ListGroup>
            </Card.Body>
          </Card>
        </Col>
      </Row>
    </div>
  );
}
```

Vemos que usa el StoreProvider, tomando de el su estado y el dispatch. Luego tiene los eventos updateCartHandler y removeProductHandler. El update tiene este código:
```jsx
const updateCartHandler = async (product, quantity) => {
 const { data } = await axios.get(`/api/products/${product._id}`);

 if (quantity > data.countInStock) {
   window.alert("Sorry. Product is out of stock");
   return;
 }

 ctxDispatch({ type: "CART_ADD_ITEM", payload: { ...product, quantity } });
};
```

Primero se consume la API para traer información actualizada del producto, para luego consultar si la cantidad de stock que se desea agregar al carrito excede a la que está disponible en stock y, de ser así, lanzar un error. Si no hay error, se ejecuta el dispatch para agregar al carrito.
El resto del código es sencillo. Sólo se expondrá de ejemplo esta parte de la vista:

```jsx
<Button
 variant="light"
 onClick={() =>
   updateCartHandler(product, product.quantity - 1)
 }
 disabled={product.quantity === 1}
>
   <i className="fas fa-minus-circle"></i>
</Button>
```

### src/components/
#### Product.jsx
![product](https://remnote-user-data.s3.amazonaws.com/jRDXbHTWd0wDN_B8WqOyOjKnr2SCZjBiF70eqhZPOLvKtB_kiq3b2oQYduYVm05PphkaiNFm0qfz_e26O9E6gB08QuYeSOEGji-N2BoiR9ZNuYt5tcFUa29QQ7WXvneU.png)

```jsx
import { Link } from "react-router-dom";
import Card from "react-bootstrap/Card";
import Button from "react-bootstrap/Button";
import Rating from "./Rating";
import axios from "axios";
import { useContext } from "react";
import { Store } from "../Store";

function Product(props) {
  const { product } = props;

  const { state, dispatch: ctxDispatch } = useContext(Store);

  const {
    cart: { cartItems },
  } = state;

  const addToCartHandler = async (product) => {
    const existItem = cartItems.find((p) => p._id === product._id);
    const quantity = existItem ? existItem.quantity + 1 : 1;
    const { data } = await axios.get(`/api/products/${product._id}`);

    if (quantity > data.countInStock) {
      window.alert("Sorry. Product is out of stock");
      return;
    }

    ctxDispatch({ type: "CART_ADD_ITEM", payload: { ...product, quantity } });
  };

  return (
    <Card className="product">
      <Link to={`product/${product.slug}`}>
        <img src={product.image} className="card-img-top" alt={product.name} />
      </Link>
      <Card.Body>
        <Link to={`product/${product.slug}`}>
          <Card.Title>{product.name}</Card.Title>
        </Link>
        <Rating rating={product.rating} numReviews={product.numReviews} />
        <Card.Text id="product-info-price">$ {product.price}</Card.Text>
        {product.countInStock === 0 ? (
          <Button variant="light" disabled>Out of stock</Button>
        ) : (
          <Button onClick={() => addToCartHandler(product)}>Add to cart</Button>
        )}
      </Card.Body>
    </Card>
  );
}
export default Product;
```

#### Rating.jsx

```jsx
function Rating(props) {
  const { rating, numReviews } = props;
  return (
    <div className="rating">
      <span>
        <i
          className={
            rating >= 1
              ? "fas fa-star"
              : rating >= 0.5
              ? "fas fa-star-half-alt"
              : "far fa-star"
          }
        />
      </span>
      <span>
        <i
          className={
            rating >= 2
              ? "fas fa-star"
              : rating >= 1.5
              ? "fas fa-star-half-alt"
              : "far fa-star"
          }
        />
      </span>
      <span>
        <i
          className={
            rating >= 3
              ? "fas fa-star"
              : rating >= 2.5
              ? "fas fa-star-half-alt"
              : "far fa-star"
          }
        />
      </span>
      <span>
        <i
          className={
            rating >= 4
              ? "fas fa-star"
              : rating >= 3.5
              ? "fas fa-star-half-alt"
              : "far fa-star"
          }
        />
      </span>
      <span>
        <i
          className={
            rating >= 5
              ? "fas fa-star"
              : rating >= 4.5
              ? "fas fa-star-half-alt"
              : "far fa-star"
          }
        />
      </span>
      <span> {numReviews} reviews</span>
    </div>
  );
}
export default Rating;
```

#### LoadingBox.jsx
```jsx
import Spinner from 'react-bootstrap/Spinner';

export default function LoadingBox() {
    return (
        <Spinner animation="border" role="status">
            <span className="visually-hidden">Loading...</span>
        </Spinner>
    )
}
```

#### MessageBox.jsx
```jsx
import Alert from 'react-bootstrap/Alert';

export default function MessageBox(props) {
    return (
        <Alert variant={props.variant || "info"}>
            {props.children}
        </Alert>
    )
}
```

#### src/utils.js
```jsx
export const getError = (error) => {
  return error.response && error.response.data.message
    ? error.response.data.message
    : error.message;
};
```
