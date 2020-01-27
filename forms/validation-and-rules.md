# Validation and rules

Forms are validated using [Laravel validation](https://laravel.com/docs/5.8/validation) rules.

### Required or nullable

All fields are assumed as nullable until you explicitly mark them required or not nullable.

```php
NumberField::make('age') // -> nullable, numeric, NOT required
NumberField::make('age')->required() // -> required, numeric, NON nullable
NumberField::make('age')->nullable(false) // -> numeric, NON nullable
```

### Specific validation rules

You can assign any [Laravel validation rules](https://laravel.com/docs/5.8/validation#available-validation-rules) to a single field:

```php
NumberField::make('age')
    // just one rule 
    ->rules('min:18')
    // or multiple rules
    ->rules(['min:18', 'max:100'])

DatetimeField:make('created')
    // custom rules closures
    ->rules([
        function ($attribute, $value, $fail) {
            if ($value === 'foo') {
                $fail($attribute.' is invalid.');
            }
        }
    ])
    // custom rules objects
    ->rules([
        new CreateDateRule()
    ])
```

