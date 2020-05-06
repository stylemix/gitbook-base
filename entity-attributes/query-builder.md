# Query builder

Laravel Base utilizes [Spatie's Query Builder](https://github.com/spatie/laravel-query-builder) in more convenient way. As soon as you declare your Entity attributes, the `Stylemix\Base\QueryBuilder` can automatically build Eloquent query according to attribute types from incoming Request.

{% code title="BooksController.php" %}
```php
use Stylemix\Base\QueryBuilder;
use App\Listing;

class BooksController extends Controller
{

    public function index()
    {   
        // Book query results filtered, sorted according to request params
        $results = QueryBuilder::for(Book::class)->build()->get();
    }
}
```
{% endcode %}

### Keyword searching

Text like attributes `Text`, `LongText`, `Enum`, `Slug` can be used to build query by keyword. When requesting with special param: `?query=foo`, query builder will generate LIKE where clauses on every text like attribute.

To exclude some attribute from searching use `excludeSearching()`.

### Excluding attributes

You can exclude any defined Entity attributes from being used in query builder in filtering, sorting, searching.

```php
public function attributeDefinitions()
{
    return [
        Number::make('amount')->excludeFilter(),
        Datetime::make('sold')->excludeSort(),
        Text::('secret')->excludeSearching(),
    ];
}
```

### Query specification by attributes

#### Text, LongText

* Filter types: like
* Sorting support: Text \(yes\), LongText \(no\)
* Request query examples: 
  * `?filter[name]=john&filter[email]=gmail`
  * `?filter[name]=seb,freek`

#### Number, Id, Relation

* Filter types: in, exact, range
* Sorting support: Number \(yes\), Id \(yes\), Relation \(No\)
* Request query examples:
  * in: `?filter[id]=1,2,3,4,5`
  * exact number: `?filter[id]=1`
  * exact relation: `?filter[category_id]=3`
  * range: `?filter[id][gt]=1&filter[id][lt]=10`
  * range: `?filter[id][gte]=1&filter[id][lte]=10`

#### Datetime

* Filter types: in, exact, range
* Sorting support: yes
* Request query examples:
  * exact: `?filter[id]=2020-04-04T01:02:00`
  * in: `?filter[id]=2020-04-04T00:00:00,2020-04-05`
  * range: `?filter[id][gt]=2020-04-04&filter[id][lt]=2020-04-30`
  * range: `?filter[id][gte]=2020-04-04&filter[id][lte]=2020-04-30`

Each filter value should be parsable with `Carbon::parse()`. After parsing Carbon date is converted to application timezone \(taken from `config('app.timezone')`\).

#### Boolean, Enum

* Filter types: exact
* Sorting support:  yes
* Request query examples: 
  * `?filter[admin]=true`
  * `?filter[status]=public`

### Custom allowed filters, sorts, includes

You are not strict to Entity attribute implementations. Add any additional criteria for query builder before calling `build()` method:

```php
$query = QueryBuilder::for(Book::class)
    ->allowedSorts([
        // your custom sorts
    ])
    ->allowedFilters([
        // your custom filters
    ])
    ->allowedIncludes([
        // your custom includes
    ])
    ->allowedFields([
        // your custom fields
    ])
    ->build();
```

