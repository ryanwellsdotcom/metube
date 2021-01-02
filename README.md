# metube

A stripped down version of Youtube using Google's YouTube API: https://developers.google.com/youtube/v3

## Project setup

```
npm install
```

The projects makes use of environment variables using Vue3 Modes.

1.  `.env.development.local` contains an unrestricted API key for development.

2.  `.env.production.local` contains a restricted API key for production.

Both .env files utilize the following syntax: `VUE_APP_API_KEY=secret`.

### Compiles and hot-reloads for development

```
npm run serve
```

### Compiles and minifies for production

```
npm run build
```

### Lints and fixes files

```
npm run lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).
