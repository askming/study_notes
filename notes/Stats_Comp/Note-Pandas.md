# Pandas study note

## Index, subsetting
- understanding how index works in pandas [^indexing]
  - Understanding `.loc`, `iloc`, `.ilocx`
    - `[]`: Primarily selects subsets of columns, but can select rows as well. Cannot simultaneously select rows and columns.
      ```{margin}
      Note that if you use df[3] it will return not row 3 but a column named 3.
      ```
      - when given names/list of names, default is to select the cols, e.g. `df['name']`; when gvien slicing index/names, selects the rows, e.g. `df[2:3]`
      - Cons: 
        - It is not as optimised as `.loc` and `.iloc`. 
        - It is more implicit, so Pandas has to figure out stuff like 'are we indexing rows or columns?' and more, behind the scenes.
        - not the recommended way to select data from your dataframe in production environments.
    - `.loc` label-based indexing on columns and/or rows; selects subsets of rows and columns by label only: `df.loc[[row_indexer], [col_indexer]]`
      - selection is inclusive, it slices from the start label up to and including the stop label
      - Integers are accepted as labels, but they will be seen as labels, not as the position in the dataframe!
      - Can use boolean mask to filter and select rows

      ```python
      # selecting rows based on their index labels (could also be numerical)
      df.loc['row_label']
      # or
      df.loc[[row_labels]]
      # or
      df.loc[1:10] # for numeric row label, result will be inclusive
      
      # selecting cols based on their names (labels)
      df.loc[:, 'col_name']
      # or 
      df.loc[:, [col_names]]
      # or 
      df.loc[:, 'col1':'col4'] #result will be inclusive
      ```


    - `.iloc` works on position (**integer** location, you must use intergers when selecting by interger location); selects subsets of rows and columns by integer location only
      - contrary to `.loc`, `.iloc` selection is **exclusive**, meaning that the stop position is not included in the final result.
      ```python
      # index can take the format as start:end:step for both row and col
      df.iloc[row_index, col_index]

      # selecting rows based on their integer row position
      df.iloc[3], df.iloc[[2,4,6]], df.iloc[:3]

      # selecting both rows and cols
      df.iloc[1:5, 1:3]
      ```
  - `at` & `iat`
    - `at` gets scalar values. it's a very fast `loc`
    - `iat` gets scalar values. it's avery fast `iloc`

  - `df.where()`: Replaces every row that doesn't match the filter with `NA`s
    ```python
    low_fat = df['diet'] == 'low fat'
    df_lf = df.where(low_fat)
    ```

  - `df.query()`: quickly select rows based on explicit SQL-like filters
    ```python
    df.query("pulse < 85")
    ```

  - `df.get()`: select a column, dictionary-style
    ```python
    df.get('diet')
    ```


  - `.idxmin`, `.idxmax`
    - to return the index of the maximum/minimum value across a specified axis in a pandas DataFrame: `df.idxmax(axis = 0, skipna = True)`
      - they will return the **first occurrence** of the maximum value.

  - subsetting: using boolean in the `.loc` row indexing to select subset of the data

## Data manipulation
- `duplicated()`, `drop_duplicates()`
  - `df.duplicated(subset, keep)`: returns a Series of bool denoting dupliate rows.
  - `drop_duplicates(subset = col_lables, keep = {`first`, `last`, False}, inplace)`
- Get the number of rows, columns, all elements (size) of DataFrame
  - `len(df)` returns # of rows of a df
  - `len(df.columns)` returns # of cols
  - `df.shapre` returns bow numbers of rows and cols
  - `df.size` returns the number of elements
-  `sum()` with `level=` option or `groupby()`
   -  `level=` option in `sum()` indicates which index level(s) the sum is performed over; it's similar to sum after `groupby` over some col(s), in which the groupby var(s) is/set to index(es) in the resulting DataFrame
   - [Pandas groupby() and sum() With Examples](https://sparkbyexamples.com/pandas/pandas-groupby-sum-examples/)
- `nlargest(n)`
  - `df.nlargest(n, columns, keep='first')` Return the first n rows ordered by columns in descending order.
  - This method is equivalent to `df.sort_values(columns, ascending=False).head(n)`, but more performant.

- `df.transform(func, axis, *args, **kwargs)`: something like the `mutate` function in `dplyr`
  - Call `func` on self producing a DataFrame with the same axis shape as self.
  - Can be used to transform values of the cols/features
  - Can be used with `groupby` and apply aggregate function to the resulting data frame.
  - Used ot filter data, applying the rule to the values created by `transform`: `df[df.groupby('city')['sales'].transform('sum') > 40]`
  - Handling missing values at the group level: `df['value'] = df.groupby('name')
                .transform(lambda x: x.fillna(x.mean()))`

- `df.rolling(window, min_periods=None, center=False, win_type=None, on=None, axis=0, closed=None, method='single')`: Provide rolling window calculations.
  - [Windowing operations](https://pandas.pydata.org/docs/user_guide/window.html#window-generic)

- Understanding `rang`, `np.arange`, `np.linspace`
  - `range` vs `np.arange`
    - The main difference between `range` and `np.arange` is that the `range()` function returns an iterator instead of a list and `np.arange()` function gives a numpy array that consists of evenly spaced values within a given interval.
    - The `range()` function generates a sequence of integer values lying between a certain range.
    - The `range()` is a built-in function whereas` arange()` is a `numpy` library function.
    - The `range()` function is more convenient when you need to iterate values using the for loop. The `np.arange()` function is more useful when you are working with arrays and you need to generate an array based on a specific sequence.
  - `np.arange` is similar to `range` but more performant
    - `np.arange(start, stop, stepsize)`
      - allows you to define the stepsize and infers the number of steps
      - **excludes the maximum value** unless rounding error makes it do otherwise
  - `np.linspace(start, stop, size)` 
    - allows you to define how many values you get **including the specified min and max value**. It infers the stepsize
    - You can exclude the `stop` value (in our case 1.3) using `endpoint=False`



## Time series data
- `df.shift()`?
  - Works with time series data
  - Shift index (date or datetime var) by desired number of periods with an optional time *freq*.
- `freq` argument in `date_range()`
- `df.interpolate()`?




## References
- [Pandas vectorization: faster code, slower code, bloated memory](https://pythonspeed.com/articles/pandas-vectorization/?utm_medium=email&utm_source=topic+optin&utm_campaign=awareness&utm_content=20220620+data+ai+nl&mkt_tok=MTA3LUZNUy0wNzAAAAGFIT-XwYQWtkAEpfgsP268hTBrSlq4DeAtsoPVMDm7nv4dyVkbzk5i3Pu5Zl9OboAgvsPBtkI97J7XKEIEpXzuys840fcmvF6-AhFE2xysMTY_c1Y)

[^indexing]: https://lucytalksdata.com/how-to-effectively-index-pandas-dataframes/