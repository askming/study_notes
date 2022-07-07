# Numpy study notes

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