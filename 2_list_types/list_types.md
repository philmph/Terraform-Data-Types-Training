# List Types

Available list types are

- `list`
- `set`
- `tuple` \*

\* Note that a `tuple` is not really a list type but uses the same syntax and referencing mechanism as a `list` does. A `tuple` is per definition a structural type used to group elements that belong together without the use of key value pairs. An example would be `["Philipp", 31, "Vienna"]`.

## Key characteristics

- They are collections and all of them contain zero or more elements
- `list` and `set` both require all elements to be of the same type
- `tuple` can have a different type per element
- Elements in `list` and `tuple` can be referenced with consecutive whole numbers starting with `0`
- `list` and `tuple` are ordered while a `set` is not

| Type    | Syntax for Elements         | Type in Variables | Identifier            | Ordered | Usable in `for_each` |
| ------- | --------------------------- | ----------------- | --------------------- | ------- | -------------------- |
| `list`  | `[element1, element2, ...]` | `list(type)`      | `[0]`, `[1]`, `[...]` | Yes     | No \*\*              |
| `set`   | `[element1, element2, ...]` | `set(type)`       | Element itself        | No      | Yes                  |
| `tuple` | `[element1, element2, ...]` | `tuple([type])`   | `[0]`, `[1]`, `[...]` | Yes     | No \*\*              |

\*\* `list` and `tuple` can be used in `for_each` by converting them to a set beforehand using `toset()`.

## Key Take-Aways

A list `[]` f.e. `[value1, value2]` definition defaults to a `tuple`. Only enclosing the `[]` in a conversion function f.e. `tolist()`, `toset()` will result in the expected type. However, `tuple` is sufficient for the most usecases.

`list` and `set` require to be of the same type for all elements. Terraform will try to convert to `string` if this is not the case.

Both `lists` and `tuples` can be referenced with `[0..n]` starting at count `0`.

`sets` are unique values and unordered, they cannot be referenced with `[0]` or `["key"]`. Instead the element itself is it's identifier.

## List & Set

Syntax wise, elements of a `list` and `set` are defined using `[element1, element2, ...]`. The function `tolist()` / `toset()` is used to convert another type to a `list` / `set`. In variables, `list(type)` / `set(type)` is used for the `type` argument.

`list` and `set` requires all elements to be of the same type.

When `list(any)` / `set(any)` is used, all cases that do not use `number` or `bool` exclusively will convert to `list(string)` / `set(string)`. This is because all values except `null` can be a `string`.

Examples:

```hcl
my_list = tolist([1, 10, 100]) # Will be of type list(number)
my_list = tolist([true, false]) # Will be of type list(bool)
my_list = tolist(["1", 10, 100, false]) # Will be of type list(string)

my_set = toset([1, 10, 100]) # Will be of type set(number)
my_set = toset([true, false]) # Will be of type set(bool)
my_set = toset(["1", 10, 100, false]) # Will be of type set(string)
```

When a `list(type)` / `set(type)` is defined for a variable `type` arguement it will be of the exact type.

When a `list` / `set` is defined in a `locals` block without using `tolist()` / `toset()`, it will be a `tuple` as default behaviour.

Using a function like `max()` on a `list` / `set`, to retrieve the highest number, requires you to expand the `list` / `set` using `...` syntax. F.e. `max(var.my_list...)` / `max(var.my_set...)`.

## Tuple
