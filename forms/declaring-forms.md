# Declaring forms

### Define a form class

Create a class which extends `Stylemix\Base\FormResource` abstract class. To meet Laravel convention of directory structure, we will save that class in `app/Http/Forms` directory.

{% code title="ContactForm.php" %}
```php
namespace App\Http\Forms;

use Stylemix\Base\Fields\TextField;
use Stylemix\Base\Fields\EmailField;
use Stylemix\Base\Fields\TextareaField;


class ContactForm extends FormResource
{

    /**
     * Field definitions for contact form
     *
     * @return \Stylemix\Base\Fields\Base[]
     */
    public function fields()
    {
        return [
            TextField::make('name')
                ->label('Your name')
                ->required(),
            EmailField::make('email')
                ->label('Your email')
                ->required(),
            TextareaField::make('message')
                ->label('Your message'),
        ];
    }
}
```
{% endcode %}

List of field types is described in [Field types section](field-types/).

### Display form in front-end

To return form structure to client side in controller just return it instance. Since `FormResource` extends Laravel's `JsonResource`, it will be transformed to JSON.

```php
namespace App\Http\Controllers;

use App\Http\Forms\ContactForm;

class ContactController extends Controller
{

    public function view()
    {
        return ContactForm::make();
    }
}
```

View [Vue Base package form implementation](http://docs.stylemix.biz/manuals/vue-base/#/).

### Initial form values

To pass initial form values just pass an object or array of values as first argument:

```php
ContactForm::make([
    'email' => $user->email
]);
```

### Process and validate form input

We use Laravel's native [Form requests](https://laravel.com/docs/5.8/validation#form-request-validation) as base functionality with some minor enhancements. Define a request class that extends `Stylemix\Base\FormRequest`, and implement `formResource` method that returns appropriate form component class:

```php
namespace App\Http\Requests;

use App\Http\Forms\ContactForm;
use Stylemix\Base\FormRequest;

class ContactRequest extends FormRequest
{

    /**
     * @return  \Stylemix\Base\FormResource
     */
    protected function formResource()
    {
        return ContactForm::make();
    }
}
```

The request class will automatically take values only for defined fields and validate them.

```php
use App\Http\Requests\ContactRequest;

/**
 * Process the incoming contact form post.
 *
 * @param ContactRequest $request
 * @return Response
 */
public function store(ContactRequest $request)
{
    // Retrieve the validated input data...
    $validated = $request->validated();
}
```

If you want the request to fill your model, use `fill` method passing your model.

```php
public function store(ContactRequest $request)
{
    $model = new MyModel();
    $request->fill($model);
}
```

{% hint style="info" %}
Only form field values from request will be filled into given resource. All other request values will be ignored.
{% endhint %}

In some case there is required to fill only specified fields:

```php
public function store(ContactRequest $request)
{
    $user = new User();
    $post = new Post();
    // Fill values only for email, password
    $request->fillOnly($user, ['email', 'password']);
    // Fill all values except email, password
    $request->fillExcept($post, ['email', 'password']);
}
```

