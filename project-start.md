# project-start

## 1. React 설치

```bash
$ creat-react-app 프로젝트 디렉토리
```

---

## 2. npm 초기화

```bash
$ npm init -y
```

---

## 3. react-script를 루트폴더로 이동시키는 작업

create-react-app의 설정을 커스텀하기 위한 작업
node_modules/react-script를 삭제하고 프로젝트의 루트로 script npm run build, npm start 등의 script 파일을 옮기는 작업

```bash
$ yarn eject 
$ npm run eject
```

---

## 4. config/webpack.config.dev.js 수정

### 4.1 webpack CSS Style'만' compile시

```js
// config/webpack.config.dev.js
// config/webpack.config.prod.js
...
{
        test: /\.css$/,
        use: [
          require.resolve('style-loader'),
          {
            loader: require.resolve('css-loader'),
            options: {
              importLoaders: 1,
              modules: true, // 여기 추가
              localIdentName: '[path][name]__[local]--[hash:base64:5]' // 여기 추가
            },
          },
          {
            loader: require.resolve('postcss-loader'),
            options: {
              (...)
            },
          },
        ],
      },
...

```

---

### 4.2 webpack SASS Style'만' compile시

```bash
$ yarn add node-sass sass-loader
$ npm i node-sass sass-loader
```

```js
// config/webpack.config.dev.js
// config/webpack.config.prod.js
// file-loader를 검색해서 나오는 스크립트를 아래와 같이 수정
...
{
  exclude: [
    /\.html$/,
    /\.(js|jsx)$/,
    /\.css$/,
    /\.json$/,
    /\.bmp$/,
    /\.gif$/,
    /\.jpe?g$/,
    /\.png$/,
    /\.scss$/, // 여기 추가
  ],
  loader: require.resolve('file-loader'),
  options: {
    name: 'static/media/[name].[hash:8].[ext]',
  },
},
...
```

```js
// config/webpack.config.dev.js
// config/webpack.config.prod.js
{
  test: /\.scss$/, // 여기 수정
  use: [
    require.resolve('style-loader'),
    {
      loader: require.resolve('css-loader'),
      options: {
        importLoaders: 1,
        modules: true,
        localIdentName: '[name]__[local]__[hash:base64:5]'
      },
    },
    {
      loader: require.resolve('postcss-loader'),
      options: {
        // Necessary for external CSS imports to work
        // https://github.com/facebookincubator/create-react-app/issues/2677
        ident: 'postcss',
        plugins: () => [
          require('postcss-flexbugs-fixes'),
          autoprefixer({
            browsers: [
              '>1%',
              'last 4 versions',
              'Firefox ESR',
              'not ie < 9', // React doesn't support IE8 anyway
            ],
            flexbox: 'no-2009',
          }),
        ],
      },
    },
    { // 여기 부터 아래 끝까지 추가
      loader: require.resolve('sass-loader'),
      options: {
        // 나중에 입력
      }
    }
  ],
},
```