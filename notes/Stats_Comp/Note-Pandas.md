# Pandas study note

- understanding how index works in pandas
  - Understanding `.loc`, `iloc`, `.ilocx` and others
    - `[]`: Primarily selects subsets of columns, but can select rows as well. Cannot simultaneously select rows and columns.
      - when given names/list of names, default is to select the cols; when gvien slicing index/names, selects the rows 
    - `.loc` only works on index (i.e. row and col names); selects subsets of rows and columns by label only
      ```python
      # selecting rows
      df.loc['row_label']
      # or
      df.loc[[row_labels]]
      
      # selecting cols
      df.loc[, 'col_name']
      ```
    - `.iloc` works on position (**integer** location, you must use intergers when selecting by interger location); selects subsets of rows and columns by integer location only
      ```python
      # index can take the format as start:end:step for both row and col
      df.iloc[row_index, col_index]
      ```
    - `at` gets scalar values. it's a very fast `loc`
    - `iat` gets scalar values. it's avery fast `iloc`

  - `.idxmin`, `.idxmax`
    - to return the index of the maximum/minimum value across a specified axis in a pandas DataFrame: `df.idxmax(axis = 0, skipna = True)`
      - they will return the **first occurrence** of the maximum value.

  - subsetting: using boolean in the `.loc` row indexing to select subset of the data

- `duplicated()`, `drop_duplicates()`
  - `df.duplicated(subset, keep)`: returns a Series of bool denoting dupliate rows.
  - `drop_duplicates(subset = col_lables, keep = {`first`, `last`, False}, inplace)`
- `len(df)` returns # of rows of a df
- `level=` option in `sum()` or `groupby()` 
- `nlargest(n)`
- Understanding `rang`, `np.arange`, `np.linspace`
- `.shift()`?
- `transform(aggfunc)`
- `rolling(n, min_period)`
- `freq` argument in `date_range()`
- `df.interpolate()`?




## References
- [Pandas vectorization: faster code, slower code, bloated memory](https://pythonspeed.com/articles/pandas-vectorization/?utm_medium=email&utm_source=topic+optin&utm_campaign=awareness&utm_content=20220620+data+ai+nl&mkt_tok=MTA3LUZNUy0wNzAAAAGFIT-XwYQWtkAEpfgsP268hTBrSlq4DeAtsoPVMDm7nv4dyVkbzk5i3Pu5Zl9OboAgvsPBtkI97J7XKEIEpXzuys840fcmvF6-AhFE2xysMTY_c1Y)