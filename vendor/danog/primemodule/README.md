# PrimeModule

[![Build Status](https://travis-ci.org/danog/PrimeModule.svg?branch=master)](https://travis-ci.org/danog/PrimeModule)

Prime module capable of doing prime factorization of huge numbers very quickly.

It can factorize huge numbers (even bigger than `PHP_INT_MAX` thanks to the wolfram alpha/python modules) very quickly.


## Installation:

Install with composer.  

```
composer require danog/primemodule
```

Install python to enable the python module and the PHP curl extension to enable the wolfram alpha module.

## Usage:

This library has 4 prime factorization modules (ordered by speed, huge semiprime is usually a 20 digit semiprime generated by [telegram](https://core.telegram.org), see the travis ci tests for more stats):


* native_cpp - A [native c++](https://github.com/danog/PrimeModule-ext) factorization module (it's the fastest), medium time 0.03943687915802 tested with 100 huge semiprime

* python - A [quadratic sieve](http://codegolf.stackexchange.com/questions/8629/fastest-semiprime-factorization) python module (usually it's faster than the pollard brent module, other times it just gets stuck (and then killed after 10 seconds by the lib), medium time 0.35134809732437 seconds calculated using 100 huge semiprimes, some of which caused the module to freeze and be killed. Usually it's 10 times faster than the pollard brent module)

* python_alt - A [pollard brent](https://stackoverflow.com/questions/4643647/fast-prime-factorization-module) module also written in python (medium time 0.1801231908798 seconds calculated using 100 huge semiprimes)

* wolfram - A [wolfram alpha](https://wolframalpha.com) module (usually takes around 2.1294961380959 seconds calculated using 100 huge semiprimes)

* native - A [native PHP lopatin](https://github.com/LonamiWebs/Telethon/blob/master/telethon/crypto/factorizator.py) module (usually takes around 2.5698633241653 seconds calculated using 100 huge semiprimes, may sometimes be faster than the wolfram module: for example on HHVM native factorization usually takes 0.1 seconds)

These modules can be used either in the single variant, which returns only one factor (useful for semiprime factorization), or the full methods, that return an array with all of the factors.

This module was created to do semiprime factorization, so it might not perform very well with composite numbers.

The python/wolframalpha modules accept numeric strings bigger than `PHP_INT_MAX`, and if the factors are bigger than `PHP_INT_MAX` they will also returned as a string.

An automatic function can also be used, which chooses automatically the module in the following order: python_alt, python, native, wolfram.


```
require 'vendor/autoload.php';

// quadratic sieve factorization
$factor = \danog\PrimeModule::python_single(2768594593405030913); // returns 1455582581 or 1902052573
// pollard brent sieve factorization
$factor = \danog\PrimeModule::python_single_alt(2768594593405030913); // returns 1455582581 or 1902052573
// native PHP single factorization
$factor = \danog\PrimeModule::native_single(2768594593405030913); // returns 1455582581 or 1902052573
// wolfram factorization
$factor = \danog\PrimeModule::wolfram_single(2768594593405030913); // returns 1455582581 or 1902052573
// automatic factorization
$factor = \danog\PrimeModule::auto_single(2768594593405030913); // returns 1455582581 or 1902052573


// quadratic sieve factorization
$factor = \danog\PrimeModule::python(15); // returns an array with 3 and 5
// pollard brent sieve factorization
$factor = \danog\PrimeModule::python_alt(15); // returns an array with 3 and 5
// native PHP factorization
$factor = \danog\PrimeModule::native(15); // returns an array with 3 and 5
// wolfram factorization
$factor = \danog\PrimeModule::wolfram(15); // returns an array with 3 and 5
// automatic factorization
$factor = \danog\PrimeModule::auto(15); // returns an array with 3 and 5


```


See `tests/testing.php` for more detailed examples.

Library created by Daniil Gentili (https://daniil.it)