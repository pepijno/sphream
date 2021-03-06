# Sphream

Sphream is a functional php library. It is inspired by Haskell Lists, Java8 Streams and underscore.js.

Sphream allows you to manipulate arrays and Traversables in a more intuitive way.

```php
$integers = [ 3, -19, 9, 1, 5, 392, 29, -13, 29, -2, -4, 234999 ];
$newIntegers = Sphream\Sphream::of($integers)
    ->filter(function ($item) { return 0 < $item && $item < 10; })
    ->takeWhile(function ($item) { return $item > 1; })
    ->map(function ($item) { return $item * 100; })
    ->toArray();

// $newIntegers is equal to [ 300, 900 ]
```

## Installation
```bash
composer require pepijno/sphream
```

## Documentation

### Creating Sphreams

#### of
Creates a Sphream from an array or a generator.

Using an array:
```php
$array = [ 3, 5, 9 ];
$sphream = Sphream\Sphream::of($array);
// $sphream contains items 3, 5 and 9.
```
When using generators:
```php
$generator = function () {
    yield 3;
    yield 5;
    yield 9;
};
$sphream = Sphream\Sphream::of($generator());
// $sphream contains items 3, 5 and 9.
```
#### mempty
Creates an empty Sphream.
```php
$sphream = Sphream\Sphream::mempty();
//$sphream is empty
```
#### range
Creates a Sphream containing a range of integers between a start and an end.
The end is not inclusive in the Sphream.
```php
$sphream = Sphream\Sphream::range(6, 11);
// $sphream contains the elements 6, 7, 8, 9 and 10.
$sphream = Sphream\Sphream::range(-21, -17);
// $sphream contains the elements -21, -20, -19 and -18.
```

#### repeat
Creates a Sphrem by repeating a value a certain `N` times.
```php
$sphream = Sphream\Sphream::repeat("Hello", 8);
// $sphream contains the string "Hello" 8 times.
```

#### generate
Creates an infinite Sphream, the elements are generated by repeatedly
executing the callback.
```php
$callback = function () { return 2; };
$sphream = Sphream\Sphream::generate($callback);
// $sphream contains of an inifite amount of 2's.
```

### Checks

#### isEmpty
Returns true if a Sphream is empty and false otherwise.
```php
Sphream\Sphream::of([])->isEmpty(); // returns true
Sphream\Sphream::of([2])->isEmpty(); // returns false
``` 

#### isClosed
Returns true if a Sphream is closed and false otherwise.
```php
$sphream = Sphream\Sphream::of([1, 2]);
$sphream->isClosed(); // returns false
$sphream->toArray();
$sphream->isClosed(); // returns true
```

### Alterations

#### map
Applies a callback to each element of the Sphream.
```php
$sphream = Sphream\Sphream::of([2, 3])
    ->map(function ($item) { return $item * 2; });
// $sphream consist of items 4 and 6.
```

#### filter
Filters all elements from the Sphream for which the callback returns false.
```php
$sphream = Sphream\Sphream::of([1, 2, 3, 4])
    ->filter(function ($item) { return ($item % 2) == 0; });
// $sphream contains elements 2 and 4.
```

### Selections

#### take
Takes the first `N` items of the Sphream, discarding the rest of the Sphream.
```php
$sphream = Sphream\Sphream::of([2, 4, 9, 1, 3])
    ->take(3);
// $sphream contains elements 2, 4 and 9.
```

#### drop
Drops the first `N` items of the Sphream.
```php
$sphream = Sphream\Sphream::of([2, 4, 9, 1, 3])
    ->drop(2);
// $sphream contains elements 9, 1 and 3.
```

#### takeWhile
Takes elements from the Sphream as long as the callback returns true.
```php
$sphream = Sphream\Sphream::of([2, 4, 9, 1, 3])
    ->takeWhile(function ($item) { return $item < 9; });
// $sphream contains elements 2 and 4.
```

#### takeWhile
Drops elements from the Sphream as long as the callback returns true.
```php
$sphream = Sphream\Sphream::of([2, 4, 9, 1, 3])
    ->takeWhile(function ($item) { return $item != 1; });
// $sphream contains elements 1 and 3.
```
