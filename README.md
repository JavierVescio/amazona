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

# Notes

- Ruta con un slug como parámetro:

```
<Route path="/product/:slug" element={<ProductScreen />} />
```

# Lesson 6

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
