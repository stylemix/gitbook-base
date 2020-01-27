# Eloquent date format problem

### The problem

If you have a DateTime casting attribute, it is required to assign string value in format strictly matching `Model::$dateFormat`. For example:

```php
use Illuminate\Database\Eloquent\Model;

class Book extends Model
{
    protected $dateFormat = 'Y/m/d H:i:s';
}

$post = new Post();

$post->created_at = '2020/01/06 16:00:00'; // <- correct

$post->created_at = '2020-01-06 16:00:00'; // <- will fail
```

### Resolution

`DateFixes` trait gives you ability to pass a date value in any format that acceptable for `Carbon::parse()` constructor.

```php
use Illuminate\Database\Eloquent\Model;
use Stylemix\Base\Eloquent\DateFixes;

class Book extends Model
{
    use DateFixes;
}

$post = new Post();
$post->created_at = '2020/01/06 16:00:00';
$post->created_at = '2020-01-06 16:00:00';
$post->created_at = '2020-01-06T16:00:00';
$post->created_at = 'Mon, 06 Jan 2020 11:29:15 GMT';
```

