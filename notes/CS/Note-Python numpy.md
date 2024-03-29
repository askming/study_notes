# Numpy study notes

## General learning resources
- [Python Numpy Tutorial (with Jupyter and Colab)](https://cs231n.github.io/python-numpy-tutorial/)

## Show numpy version and the configuration
```python
np.__version__
np.show_config()
```

## Memory size of an array
The memory size of an array equals to `array.size * array.itemsize`

## Get the documentation of a numpy function
- using `np.add` as an example:
    ```python
    np.info(np.add)
    ```
    - in command line `$ python -c "import numpy as np; np.info33(np.add)"`

## Compare `arange` and `linspace`
- Both return evenly spaced numbers over a specified interval.
- `arange` is similar to `linspace`, but it uses step size instead of number of samples

## Reverse an vector
- `np.flip(vec)`
- `vec[::-1]`

## Find non-zero/zero elements in a list
- non-zero: `np.nonzero(l)`
- zero: `np.argwhere(l == 0)`

## Create identify matrix
- `np.eye(n)`
- `np.identity(n)`
- `identity` just calls eye so there is no difference in how the arrays are constructed.
- the main difference is that with `eye` the diagonal can may be offset, whereas `identity` only fills the main diagonal.


## Pad an array with `np.pad`
`numpy.pad(array, pad_width, mode='constant', **kwargs)`

## Understanding `np.nan`

## `numpy.diag`
- `numpy.diag(v, k=0)`: Extract a diagonal or construct a diagonal array. 
  - The default is 0. Use k>0 for diagonals above the main diagonal, and k<0 for diagonals below the main diagonal.

## numpy array index
- what does the following code mean?
    ```python
    Z = np.zeros((8,8),dtype=int)
    Z[1::2,::2] = 1
    Z[::2,1::2] = 1
    ```

## matrix production in numpy
- `numpy.dot(m1, m2)`
- `m1 @ m2` in Python 3.5 and above


## Uncategorized
- `numpy.unravel_index(99, (6, 7, 8))`
- creating new dtype
    ```python
    color = np.dtype([("r", np.ubyte),
                  ("g", np.ubyte),
                  ("b", np.ubyte),
                  ("a", np.ubyte)])
    ```  

 ## Find common values between two array
 - `np.intersect1d(v1, v2)`, where `v1` and `v2` are two numpy arrays


## Ignore numpy warnings
- `defaults = np.seterr(all="ignore")`
  - back to sanity: `np.setarr(**defaults)`

- with a context manager
  ```python
    with np.errstate(all="ignore"):
        np.arange(3)/0
  ```

## Date in numpy
- today: `numpy.datetime64('today')`
- yesterday: `numpy.datetime64('today') - numpy.timedelta64(1)`
- get all the dates in cerntain month/between two dates, e.g. `np.arange('2016-07', '2016-08', dtype='datetime64[D]')`

## Doing calculation in place
- `numpy.add(A, B, out = B)`

## Check if two arrays are equal
- comparing values only: `np.allclose(A, B)`
- comapring both shape and values: `np.array_equal(A, B)`

## Make an array immutable
- given an array v: `v.flags.writeable = False`


## Read txt file in numpy
- Example
  ```python
  from io import StringIO

  # Fake file
  s = StringIO('''1, 2, 3, 4, 5

                  6,  ,  , 7, 8

                  ,  , 9,10,11
              ''')
  Z = np.genfromtxt(s, delimiter=",", dtype=np.int)
  print(Z)
  ```

## Sort an array by the nth column
  ```python
  Z = np.random.randint(0,10,(3,3))
  print(Z)
  print(Z[Z[:,1].argsort()])
  ```


## `np.reshape()`
- what does the `-1` mean in `v.reshape(-1, x)`
- more generally, the index of arraries

## `np.bincount()`
- Convert a `np.bincount()` result back to the original vector
  ```python
  C = np.bincount([1,1,2,3,4,4,6])
  A = np.repeat(np.arange(len(C)), C)
  print(A)
  ```


## `np.enisum`
See intro blog [here](https://ajcr.net/Basic-guide-to-einsum/)