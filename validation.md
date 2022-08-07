# Validation Tips

- [Rule::unique](#rule::unique-with-ignore-method-on-update)

## Rule::unique with ignore method on update

Adding a record:


```php
use Illuminate\Validation\Rule;
use Smake\Common\Http\Requests\Request;

class AddRequest extends Request
{
    public function rules()
    {
        return [
            'email' => [
                'required',
                'email:strict',
                'max:255',
                Rule::unique('users', 'email'),
            ],
        ];
    }
//...
```

Update an existing record:

```php
use Illuminate\Validation\Rule;
use Smake\Common\Http\Requests\Request;

class UpdateRequest extends Request
{
    public function rules()
    {
        return [
            'email' => [
                'required',
                'email:strict',
                'max:255',
                Rule::unique('users', 'email')
                    ->ignore($this->route('userId')),
            ],
        ];
    }

//...
```

