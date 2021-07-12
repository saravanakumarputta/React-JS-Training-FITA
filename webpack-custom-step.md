# Custom react-typescript webpack setup

## Create node project

## Add react, react-dom as production dependency

npm i -S react react-dom

## Add Typescript as development dependencies

npm i -D typescript @types/react @types/react-dom

## Add Typescript config file (tsconfig.json)

```javascript
{
  "compilerOptions": {
    "target": "ES5" /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */,
    "module": "ESNext" /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
    "moduleResolution": "node" /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */ /* Type declaration files to be included in compilation. */,
    "lib": [
      "DOM",
      "ESNext"
    ] /* Specify library files to be included in the compilation. */,
    "jsx": "react-jsx" /* Specify JSX code generation: 'preserve', 'react-native', 'react' or 'react-jsx'. */,
    "noEmit": true /* Do not emit outputs. */,
    "isolatedModules": true /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */,
    "esModuleInterop": true /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */,
    "strict": true /* Enable all strict type-checking options. */,
    "skipLibCheck": true /* Skip type checking of declaration files. */,
    "forceConsistentCasingInFileNames": true /* Disallow inconsistently-cased references to the same file. */,
    "resolveJsonModule": true
    // "allowJs": true /* Allow javascript files to be compiled. Useful when migrating JS to TS */,
    // "checkJs": true /* Report errors in .js files. Works in tandem with allowJs. */,
  },
  "include": ["src/**/*"]
}
```

## Add babel as devlopment dependencies

npm i -D @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript

## Add babel config file (.babelrc)

```javascript
{
  "presets": [
    "@babel/preset-env",
    [
      "@babel/preset-react",
      {
        "runtime": "automatic"
      }
    ],
    "@babel/preset-typescript"
  ]
}
```

## Add webpack as dev dependecies

npm i -D webpack webpack-cli webpack-dev-server html-webpack-plugin

## Create webpack config file (webpack.config.js)

- This Config for base setup where we use the babel-loader and html-webpack-plugin

```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
	entry: path.resolve(__dirname, "..", "./src/index.tsx"),
	resolve: {
		extensions: [".tsx", ".ts", ".js"]
	},
	module: {
		rules: [
			{
				test: /\.(ts|js)x?$/,
				exclude: /node_modules/,
				use: [
					{
						loader: "babel-loader"
					}
				]
			}
		]
	},
	output: {
		path: path.resolve(__dirname, "..", "./build"),
		filename: "bundle.js"
	},
	plugins: [
		new HtmlWebpackPlugin({
			template: path.resolve(__dirname, "..", "./src/index.html")
		})
	]
};
```

## Add CSS support for imports

npm i -D css-loader style-loader

Add below to the webpack rules section

```javascript
  {
    test: /\.css$/,
    use: ['style-loader', 'css-loader'],
  }
```

## Add images/SVG support for imports

- Add declaration.d.ts - file in src folder

```javascript
declare module '*.png'
declare module '*.svg' {
 const content: string
 export default content
}
```

Add below to the webpack rules section

```javascript
 {
    test: /\.(?:ico|gif|png|jpg|jpeg)$/i,
    type: 'asset/resource',
  },
  {
    test: /\.(woff(2)?|eot|ttf|otf|svg|)$/,
    type: 'asset/inline',
  }
```

## Add webpack config for multiple environments (development, production etc.,)

- Install webpack-merge as development dependency
  npm i -D webpack-merge

- Create a webpack folder
- Rename exisiting webpack.config.js file to webpack.common.js file
- create new files as below

webpack.dev.js

```javascript
const webpack = require("webpack");

module.exports = {
	mode: "development",
	devtool: "cheap-module-source-map",
	devServer: {
		hot: true,
		open: true
	}
};
```

webpack.prod.js

```javascript
const webpack = require("webpack");

module.exports = {
	mode: "production",
	devtool: "source-map"
};
```

webpack.config.js

```javascript
const { merge } = require("webpack-merge");
const commonConfig = require("./webpack.common.js");

module.exports = (envVars) => {
	const { env } = envVars;
	const envConfig = require(`./webpack.${env}.js`);
	const config = merge(commonConfig, envConfig);
	return config;
};
```

## Update the npm scripts

"start": "webpack serve --config webpack/webpack.config.js --env env=dev",
"build": "webpack --config webpack/webpack.config.js --env env=prod"

## ESLint Setup

- Install the ESLint as dev dependencies
  npm i -D eslint
  npm i -D eslint-plugin-react eslint-plugin-react-hooks

- Install typescript eslint packages as dev dependencies
  npm i -D @typescript-eslint/parser @typescript-eslint/eslint-plugin

- Add eslint config file (.eslint.ec.js)

```javascript
module.exports = {
	parser: "@typescript-eslint/parser",
	parserOptions: {
		ecmaVersion: 2020,
		sourceType: "module"
	},
	settings: {
		react: {
			version: "detect"
		}
	},
	extends: [
		"plugin:react/recommended",
		"plugin:react-hooks/recommended",
		"plugin:@typescript-eslint/recommended"
	],
	rules: {
		"no-unused-vars": "off",
		"@typescript-eslint/no-unused-vars": ["error"],
		"@typescript-eslint/no-var-requires": "off",
		"react/prop-types": "off",
		"react/jsx-uses-react": "off",
		"react/react-in-jsx-scope": "off",
		"@typescript-eslint/explicit-module-boundary-types": "off"
	}
};
```

- Add the script to package.json
  ```
  "lint": "eslint --fix \"./src/**/*.{js,jsx,ts,tsx,json}\"",
  ```

## Add prettier config

- Install prettier packages as dev dependencies
  npm i -D prettier eslint-config-prettier eslint-plugin-prettier

- Add prettier config file (.prettierrc.js)

```javascript
module.exports = {
	semi: false,
	trailingComma: "es5",
	singleQuote: true,
	printWidth: 80,
	tabWidth: 2,
	endOfLine: "auto"
};
```

- Update the eslint config extends property with additional config

'prettier/@typescript-eslint',
'plugin:prettier/recommended',

- Add the prettier script to package.json

```
"format": "prettier --write \"./src/**/*.{js,jsx,ts,tsx,json,css,scss,md}\"",
```
