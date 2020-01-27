# HTTP Endpoints

### Define Endpoint

{% code title="config.js" %}
```javascript
import Base from 'stylemix-base'

Base.httpEndpoint('$http', {
    baseUrl: 'https://cool.service.com/api',
})
```
{% endcode %}

The function above creates a new [Axios](https://github.com/axios/axios) instance for every endpoint, and registers Vue property with the given name \(`$http` in example above\).

### Use Endpoint

{% code title="BooksComponent.vue" %}
```javascript
export default {
  async mounted() {
    this.books = (await this.$http.get('books')).data
  }
}
```
{% endcode %}

### Configuration

#### Default headers

By default the following headers are applied to all requests:

```javascript
{
  headers: {
    common: {
      'Accept': 'application/json',
    },
  },
}
```

To add extra common headers \(will be merged with defaults\):

{% code title="config.js" %}
```javascript
Base.httpEndpoint('$http', {
  // ...
  commonHeaders: {
    'Authorization': credentials
  },
  //...and so on
})
```
{% endcode %}

#### Axios config

You can pass any configuration options available for [Axios](https://github.com/axios/axios) while defining endpoint:

{% code title="config.js" %}
```javascript
import Base from 'stylemix-base'

Base.httpEndpoint('$http', {
  baseUrl: '...',
  headers: {},
  timeout: 1000,
  maxRedirects: 5,
  paramSerializer: function (params) {/*...*/},
  //...and so on
})
```
{% endcode %}

#### Modify existing endpoint

To modify previously defined endpoint:

```javascript
// in component scope
this.$http.defaults.headers = {}
this.$http.interceptors.request.use(function () {/*...*/})

// global scope
import Base from 'stylemix-base'
Base.endpoints.$http.defaults.headers = {}
Base.endpoints.$http.interceptors.request.use(function () {/*...*/})

// Via Vue
Vue.$http.defaults.headers = {}
Vue.$http.interceptors.request.use(function () {/*...*/})

```

