# AMAZONA

# Lessons

1. Introduction
2. Install Tools
3. Create React App
4. Create Git Repository
5. List Products
   a. create products array
   b. add product images
   c. render products
   d. style products
6. Add routing
   a. npm i react-router-dom
   b. create route for home screen
   c. create route for product screen
7. Create Node.JS server
8. Fetch products from backend
   a. set proxy in package.json
   b. npm i axios
   c. use state hook
   d. use effect hook
   e. use reducer hook
9. Manage State By Reducer Hook
   a. define reducer
   b. update fetch data
   c. get state from useReducer
10. Install React Bootstrap.
11. Create Product and Rating Component

# Notes

- Ruta con un slug como parámetro:

```
<Route path="/product/:slug" element={<ProductScreen />} />
```

## Lesson 8

- In the product screen, we are going to get the slug from the URL. To do that, we need to use a Hook from React-Router-Dom. The name of this Hook is useParams().

```
  const params = useParams()
  const { slug } = paramsconst ProductScreen = () => {
  const params = useParams()
  const { slug } = params
  return (
    <div>
      <h1>{slug}</h1>
    </div>
  )
```

![](https://remnote-user-data.s3.amazonaws.com/aJtWG08hVCxZ-IgRbk-5x5JsqNOefx-PqEtgK6vub6clv5PA6e5LjgV70UzhcKh28Q2grX-3a4blp3Lm9fbicy-qVXhOottCrjdH764eo4zVrsLN6wuxZkzDEpG6sxyj.png)

- On an SPA there shouldn't be any page refresh. If we click the amazona logo, it will refresh the page. To fix this issue, we need to use Link instead of Anchor ("a" tag) and use property "to" instead of "href". Link is a component from React-Router-Dom.

## Lesson 9

- Crear un directorio llamado backend. Ejecutar npm init. En el package.json agregar debajo del name "type": "module" porque vamos a usar import en vez de require.
- Instalar express: npm i express.
- Crear un archivo llamado server.js y poner el siguiente código:

```
  import express from 'express';
  import data from './data.js';

  const app = express();

  app.get('/api/products', (req, res) => {
    res.send(data.products);
  });

  const port = process.env.PORT || 5000;

  app.listen(port, () => {
    console.log(`Serve at http://localhost:${port}`);
  });
```

- Instalar extensión JSON viewer en el navegador web para ver mejor los json.
- Instalar nodemon para actualizaciones de cambios en vivo del server.
  Usar ǹpm i nodemon --save-dev. Ponemos el save-dev para que el paquete no se instale en producción. Luego ir a package.json y en scripts agregar "start": "nodemon server.js". Para ejecutar el server, hacer ǹpm start en vez de node server.js.

## Lesson 10

- In the frontend, go to package.json and add a proxy right after the name: "proxy":"http://localhost:5000". With this, we can have access to fetch data from the backend.
- It's time to define a state to save the data (the products) from the backend. In HomeScreen component we can write something like this:

```
  const [products, setProducts] = useState([])
  useEffect(() => {
    const fetchData = async () => {
      const result = await axios.get('/api/products');
      setProducts(result.data);
    }
    fetchData();
  }, [])
```

## Lesson 11

- We're going to replace useState with useReducer to manage complex states in the HomeScreen component. We're going to use reducer and record all changes in the state of UI. So, when there's a change in the state (for example, after getting data from the backend), the new state will show all changes in the state.
- Let's define a reducer function (it should be placed between the imports and the functional component). It accepts two parameters. The first one is current state and the second one is the action that change the state and create a new state.

```
  const reducer = (state, action) => {
    switch (action.type) {
      case 'FETCH_REQUEST':
        return { ...state, loading: true };
      case 'FETCH_SUCCESS':
        return { ...state, products: action.payload, loading: false };
      case 'FETCH_FAIL':
        return { ...state, loading: false, error: action.payload };
      default:
        return state;
    }
  }
```

- Inside the functional component, we replace the useState with this:

```
  const [{ loading, error, products }, dispatch] = useReducer(reducer, {
    products: [],
    loading: true,
    error: ''
  });
```

- And the useEffect should look like this:

```
  useEffect(() => {
    const fetchData = async () => {
      dispatch({ type: 'FETCH_REQUEST' })
      try {
        const result = await axios.get('/api/products');
        dispatch({ type: 'FETCH_SUCCESS', payload: result.data })
      } catch (err) {
        dispatch({ type: 'FETCH_FAIL', payload: err.message })
      }
    }
    fetchData();
  }, [])
```

- Then we can return something like this:

```
  loading ? (
    <div>Loading...</div>
  ) : error ? (
    <div>{error}</div>
  ) : (
    products.map(product => (
```

- Para ver los cambios de estado en la consola, se recomienda instalar "npm i use-reducer-logger ‒force". Luego de instalado, cambiar la parte del useReducer, agregando un logger al reducer:

```
  const [{ loading, error, products }, dispatch] = useReducer(logger(reducer), {
    products: [],
    loading: true,
    error: ''
  });
```

- Luego basta con ejecutar la app, ver la consola y se apreciarán los cambios de estado.
  ![](https://remnote-user-data.s3.amazonaws.com/8hcdy2P0YlqJ9kPBFeiId1NIaNJDlS4tVdXzZqezt5ozoSlMUihVrJu_uwqODZEu4540RjAAeLWtD2PgW5jdSyYN7y-YFAvK56cROwd-pO3b0BWpzX7dwZe_y7Rxgg0j.png)
- Se recomienda ir a network y cambiar "No throttling" por "Slow 3G", así se aprecian los cambios de estado en la propia página web (sin necesidad de ver los logs en la consola).
