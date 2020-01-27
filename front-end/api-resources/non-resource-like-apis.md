# Non-Resource like APIs

To define API classes that with custom methods that are not convenient with resource-like methods \(get, find, store, update, delete\) extend your class from `ApiBase`class and declare your own methods.

A good example could be authentication methods:

{% code title="Auth.js" %}
```javascript
import { ApiBase } from 'stylemix-base' 

export default class Auth extends ApiResource {

  login(credentials) {
    return this.request('post', 'login', credentials)
  }

  register(data) {
    return this.request('post', 'register', data)
  }
}
```
{% endcode %}



