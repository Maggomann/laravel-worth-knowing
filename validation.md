# Validation Tips

- [Rule::unique](#ruleunique-with-ignore-method-on-update)
- [prepareForValidation](#prepareForValidation)
- [passedValidation](#passedValidation)


## Rule::unique with ignore method on update

__Adding a record__:


```php
use Illuminate\Validation\Rule;
use Smake\Common\Http\Requests\Request;

class AddUserRequest extends Request
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

__Update an existing record__:

```php
use Illuminate\Validation\Rule;
use Smake\Common\Http\Requests\Request;

class EditUserRequest extends Request
{
    public function rules()
    {
        return [
            'email' => [
                'required',
                'email:strict',
                'max:255',
                Rule::unique('users', 'email')->ignore($this->route('userId')),
            ],
        ];
    }

//...
```

## prepareForValidation

```php
use Illuminate\Validation\Rule;
use Smake\Common\Http\Requests\Request;

class EditUserRequest extends Request
{
    protected function prepareForValidation()
    {
        // do something before the data has been validated
        // Example:
        parent::prepareForValidation();

        $this->merge([
            'nickname' => $this->input('nickname', '') . ' with Love',
        ]);
    }

//...
```

## passedValidation

```php
use Illuminate\Validation\Rule;
use Smake\Common\Http\Requests\Request;

class EditUserRequest extends Request
{
    protected function passedValidation()
    {
        // do something after the data has been validated
        // Example:
        $this->merge([
            'nickname' => $this->input('nickname', '') . '❤️', 
        ]);


        // override the data for the request()->validated() method hack
        $this->getValidatorInstance()->setData(
            collect($this->input())
                ->only(collect($this->rules()]->keys())
                ->all()
        );
    }
//...
```