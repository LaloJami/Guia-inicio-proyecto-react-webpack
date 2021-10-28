<div align="center">
  <h1>Guía para iniciar un proyecto en react con webpack</h1>
</div>

Esta guía la realize con la necesidad de iniciar un nuevo proyecto de react y no recordar los pasos necesarios para hacerlo.

# Archivos y comandos
1. Crear la carpeta del proyecto
```
take nombre_proyecto
```
2. iniciamos paquetes npm
```
npm init -y
```
3. iniciamos git
```
git init
```
4. instalamos react
```
npm install react react-dom
```
cramos el archivo html `public/index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Platzi Conf Merch</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>
```
cramos el archivo `src/index.js`
```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';

ReactDOM.render(<App />, document.getElementById('root'));
```
cramos el archivo `src/App.js`
```js
import React from 'react'

const App = () => {
  return (
    <div>
      <h1>hola mundo</h1>
    </div>
  )
}

export default App;
```
# Instalación de Webpack y Babel: presets, plugins y loaders
## Comando para instalar webpack:
```
npm install webpack webpack-cli webpack-dev-server --save-dev
```
## Comando para instalar el plugin de html:
```
npm install html-webpack-plugin html-loader --save-dev
```
## Comando para instalar babel:
```
npm install babel-loader  @babel/preset-env @babel/preset-react @babel/core --save-dev
```
# Configuración de webpack 5 y webpack dev server
Primero creamos un archivo llamado `webpack.config.js`
```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
  },
  resolve: {
    extensions: [".js", ".jsx"],
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
          },
        ],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./public/index.html",
      filename: "./index.html",
    }),
  ],
  devServer: {
    static: path.join(__dirname, "dist"),
    compress: true,
    port: 3000,
    open: true
  },
};
```
Crear archivo `.babelrc`
```
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```
en el `package.json` agregar en scripts
```js
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack serve --mode development",
    "build": "webpack --mode production"
  },
```
# Correr el proyecto
Si tienes todos los pasos realizados lo unico que falta es que corras tus comandos 
```
# solo la primera vez para que muestre los archivos creados en dist
npm run build
```
y el comando 
```
npm run start
```
# configuracion de webpack 5 con loaders y estilos
instalamos dependencias
```
npm install css-loader mini-css-extract-plugin --save-dev
```
configuramos nuestro archivo `webpack.config.js`
```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module: {
    rules: [
      {
        ...,
        test: /\.css$/,
        use: [ 
          {

            loader: MiniCssExtractPlugin.loader,
          },
          'css-loader',
        ],
      }
    ],
  },
  plugins: [
    ...,
    new MiniCssExtractPlugin({
      filename: 'assets/[name].css',
    })
  ],
```
Lo que nos queda es aplicarlo en nuestro proyecto, para ver si nuestra configuracion es la adecuada vamos a estilar el componente App, nos creamos el archivo `src/styles/components/app.css`
```css
h1 {
  font-size: 40px;
  color: blue;
}
```

# Loaders de Webpack para Preprocesadores CSS

## Loaders de Webpack para preprocesadores CSS
**¿Quieres utilizar tu preprocesador favorito (como Sass, Less o Stylus) para crear los estilos en tus aplicaciones con React.js?** En esta lectura aprenderás cómo implementarlos dentro de tu proyecto con Webpack.

## Configuración de tu proyecto con Sass
Primero debemos de instalar las dependencias necesarias para darle soporte a Sass dentro de nuestro proyecto:
```
npm install --save-dev sass-loader node-sass
```
Una vez agregadas las dependencias necesarias, debemos agregar una nueva regla a la configuración de Webpack en la parte de rules:
```js
{
	test: /\.scss$/,
	loader: [
		MiniCSSExtractPlugin.loader,
		'css-loader',
		'sass-loader'
	]
}
```
Ahora puedes agregar archivos Sass a cada componente y tendrás el mismo resultado que configurar directamente CSS en tu proyecto

## Configuración de tu proyecto con Less
Para darle soporte a Less dentro del proyecto debemos repetir los pasos anteriores, pero con la configuración apropiada para utilizar Less.
```
npm install --save-dev less less-loader
```
Agregar la configuración de Less a Webpack
```js
{
	test: /\.less$/,
	loader: [
		MiniCSSExtractPlugin.loader,
		'css-loader',
		'less-loader'
	]
}
```
## Configuración de tu proyecto con Stylus
Siguiendo el ejemplo de las configuraciones previas para Sass y Less vamos a repetir los pasos para agregar soporte a Stylus.
```
npm install --save-dev stylus stylus-loader
```
Ahora agregamos la configuración de Stylus a Webpack:
```js
{
	test: /\.styl$/,
	loader: [
		MiniCSSExtractPlugin.loader,
		'css-loader',
		'stylus-loader'
	]
}
```

