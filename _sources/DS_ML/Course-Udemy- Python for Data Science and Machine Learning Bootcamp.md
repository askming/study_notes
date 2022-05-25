# Udemy: Python for Data Science and Machine Learning Bootcamp

 *Study material stored @  OneDrive/Documents/Study Documents/Online courses*

----
## Section 5. Numpy arrays

1.  numpy array, vector vs matrix	

    - `np.arange`: uses step size vs `np.linspace`: uses length as input
    - `np.eye(n)`: creates n-dimensional diagonal matrix, i.e. identity matrix
    - `np.random`
      - `np.random.rand`: uniform(0, 1)
      - `np.random.randn`: $N(0,1)$
      - `np.random.randint`: random integer between low and high input
    - reshape method of an array: shape feature/attribute of an array
    - `.max()/.min()`: returns value, `.argmax()/.argmin()`: returns index label
      - Compared with `.argX()` functions `idxmax()` or `idxmin()` may be preferred
    - `array.dtype`

    1.  numpy array indexing

    - array slicing => a new array: now a copy of the sliced part but just a pointer to the original array
      - to make a real copy, use `array.copy()`method
    - `np.array()` can be used to create an array instance
    - indexing of 2-d array: if only one index is provided -> refers to the row number, eg `array[0]` -> first row
      - to get a single element, using double `[ `, i.e. `[[]]`,  can achieve it; BUT, single bracket with momma is preferred 

    1.  

    - Universal functions: unfun

    - broadcasting

## Section 6. Pandas

27. DataFrames - Part 1
    - `df.drop(axis=0/1, inplace = True)`, without `inplace = True`, pd will just wake make a copy of the DF after dropping, or assign DF is unchanged
    - Indexing a DF:
      - `df.loc["row index name"]`
      - `df.iloc[row numeric index number]`
28. DataFrames - Part 2
    - :star: Python's `and` keyword works only for scalar scalar booleen object but not for array of Boolean​; should use `&` for arrays
    - rest index: `df.reset_index()`, original index => a column; no inplace
    - set index: `df.set_index(column number)`
29. DataFrames - Part 3
    - `pd.multiIndex.from_turple(input turple)`
    - `df.xs(index value, level = index name)`: cross-section method
30. Missing data
    - `df.dropna(default axis = 0)`: drop any row with `nan`
      - thresh = int, at least how many `nan` values are needed to drop the row/column
    - `df.fillna(value = )`
31. Groupby
    - `df.groupby(colum name).mean()`
    - :star: ​`df.gorupby().describe()`
32. Merging, joining, and concatenating
    - `pd.concat(df1, df2, df3, ...)`, `axis = 0` by default => join the rows
      - if column/index doesn't align, will lead to some `nan`
    - `pd.merge(how = "left/right/innter, etc", on = column name)`
    - `pd.join()`: similar as merge by uses index rather than column for matching
33. Operations
    - find unique values: `df[col].unique()`
    - find frequency of each unique value in a column: `df[col].value_counts()`
    - :star: apply method: `df[col].apply(fun)`
    - `df.sort_values(by = col_name)`
    - `df.isnull()`
    - pivotal table: `df.pivot_table(value = , index = , columns = )`
34. Data I/O
    1. .csv: `pd.read_csv()`, `pd.to_csv("name", index = False)`
    2. Excel: `xlrd` module
       - `pd.read_excel("file.xlsx", sheet_name = " ")`
       - `pd.to_excel("", sheet_name=" ")`
    3. HTML: `lxml`/`html5lib`/`BeautifulSoup4`
       - `pd.read_html("xxx.html")`
    4. SQL: `sqlalchemy` => `create_engine`

## Section 8. Python for data visualization - `matplotlib`

42-44. `matplotlib` Parts 1 - 3

- ` %matplotlib inline`: used in jupyter nb, otherwise need to use `plot.show()` everytime

- Functional method: `plt.plot(x, y)`

- OO method:

  ```python
  fig = plt.figure() # figure -> canvas
  axes = fig.add_axes() # axes level plot
  axes.plot(x, y)
  ```

- Note: `plt.subplot()` $\ne$ `plt.subplots()`, the second creates a list of axes object as doing multiple `fig.add_axes()`

  `fig, axes = plt.subplots()`

- `plt.tight_layout()`: better show multple subplots

## Section 9. Python for data visualization - `seaborn`

48. `distplot`: histogram

    `jointplot`: scatter plot

    `pairplot`: scatter plot matrix

    `rugplot`: density plot -> kdeplot

49. Categorical plot

    - `barplot`: x = cat, y = continuous
    - `countplot`: x = cat, y = count of occurrences
    - `boxplot`: x = cat, y = continous
    - `violinplot`: `hue`, `split = True`
    - `stripplot`: `jitter  = True`
    - `swarmplot`: `stripplot` + `violinplot`
    - `factorplot`: can do all above by specifying `kind = ...`

50. Matrix plot

    - matrix form of the data
    - `heatmap` (matrix-df)
    - `clustermap`: clustered version of `heatmap`

51. Grids

    - `g.sns.PairGrid()`
    - `g.map.diag/upper/lower`
    - `FacetGrid`: creates subgroups for plotting

52. Regression plot

    - `lmplot()`: scatter plot with regression line

53. Style & color

    - `sns.set_style()`
    - `sns.despine()`, inputs: top, right, bottom, left
    - `sns.set_context("poster", font_scale = x)`

## Section 10. Python for data visualization - Pandas built-in data visualization

56. Pandas built-in data visualization
    - `pd[‘col’].plot(kind = “”, …)`
    - `pd['col'].plot.hist()`
    - `df.plot.x()`

## Section 11. Python for data visualization - plotly & cufflinks

60. plotly & cufflinks
    - Cufflinks: toolbox links plotly & pandas
    - plotly is free to use for all functions but needs to pay if like to save online
    - `from plotly import __version__`
    - `from plotly.offline import download_plotlyjs, init_notebook_mode, plot.iplot`
    - `init_notebook_mode(connected = True)`
    - use plotly: `df.iplot()`

## Section 13. Data capstone project

- `.groupby().unstack()` ?
- `.groupby().reset_index()` -> `FacetGrid()`

## Section 18. K-nearest neighbors

- In KNN, all variables need to be at the same scale, otherwise some variables may dominate the distance calculation
- Find scikit-learn cheatsheet (multiple)
- Tuning parameters
  - n-neighbors
  - Distance metric

## Section 19. Decision trees & random forest

## Seciton 20. SVM

102. 
     - `from sklearn.svm import SVC`
     - Grid search:
       - `from sklearn.model_selection import GridSearchcv`
       - `grid = GridSearchCV(SVC(), param_grid, refit = True, Verbose = 3)`
       - `grid_fit(x_train, y_train)`

## Section 21. K means clustering (unsupervised)

- Finding K: 
  - "elbow" method, using SSE: sum of the squared distance between each member of the clusters and its centroid
  - In `sklean.dataets`, can use `make_blobs` to generate fake cluster data
  - After fitting kmeans to data, can retrive centers from `x.cluster_centers` and retrieve cluster label from `x.labels_`

## Section 22. PCA

110. PCA with python

     - need to standadize the variables before conducitng PCA:

       ```python
       from sklearn.preprocessing import standardScaler
       scaler = StandardScaler()
       scaler.fit(df)
       scaled_df = scaler.transform(df)
       ```

     - Load PCA

       ```python
       from sklearn.decomposition import PCA
       ```

## Section 23. Reconmmendation system

- Content based: attribute of the items and based on similarity b/t them
- Collaborative filtering (CF): Amazon. based on knowledge of users' attitude to items, "wisdom of crowd"
  - more commonly used, produces better results
  - able to feature learning on its own
- CF subtypes
  - memory-based collaborative filtering 
  - Model-based collaborative filtering: SVD
- Pandas: `df.corrwith(df)`: correlation b/t two df columns
- Seems the method only uses correlation b/t ratings to check the similarity between 2 movies

## Section 25. Big data and spark with python

- Local vs distributed system:

  - distributed means multiple machiens connected in a network

- Hadoop: a way ot distribute very large files across multiple machines

  - Hadoop Distributed File System (HDFS)
  - Hadoop also uses MapReduce, that allows computations on that data
  - HDFS used blocks of data iwth a size of 128 MB by default; each of these blocks is replicated 3 times; the blocks are distributed in a way to support fault tolerance

- MapReduce: a way of splitting a computation taks to a distributed set of files (such as HDFS); it consists a job tracker and multiple Task Trackers

- Spark can be thought as a flexible alternative to MapReduce:

  - MapReduce requires files to be stored in HDFS, Spark doesn't
  - Spark can perform operations up to 100X fater than MR

- Core idea of Spark: resilient distributed dataset (RDD), four main features

  - Distributed collection of data
  - Fault-tolerant
  - Parallel operation - partioned
  - Ability to use many data sources

- RDD are immutable, lazily evaluated, and cacheable

- There are two types of RDD operations:

  1. Transformations
     - RDD.filter
     - RDD.map: ~ `pd.apply()`
     - RDD.flatMap
  2. Actions
     - First: return the first element of RDD
     - Collect: return all the elements of the RDD
     - Count: retun NO. of elements
     - Take: return an array with the first n elements of the RDD

- Reduce(): aggregate RDD elements using a function that returns a single element

- ReduceByKey(): aggregate Pair RDD elements using a function that returns a Pair RDD

  - similar ot groupby operation

- AWS EC2: virtual computer lab

  - login to EC2 using SSH

    ```bash
    ssh -i xx.pem ubuntu@public DNS #
    ```

  - PySpark setup

    - source.hashrc # set to Anaconda Python

129. Intro. to Spark & Python

     - Notebook magic command:

       ```python
       %% writefile example.txt
       text ...
       ```

       anything within text is written into "example.txt"

       ```python
       from pyspark import SparkContext
       SC = SparkContext()
       ```

       - SC has many different methods

130. RDD Transformations & Actions

     - Transformation => an RDD object
     - Action => a local object 

## Section 26. Neural Nets and Deep Learning

- Perceptron: "feed-formed" model, i.e. inputs are sent into the neuron, are processed, and result in an output
  1. receive inputs
  2. weight inputs
  3. sum inputs
  4. generate output

134. TensorFolow
     - Basic idea: create data flow graphs, which have nodes and edges. The array (data) passed along from layer of nodes ot layer of nodes is known as Tensor
     - Two ways to use TF:
       - Customizable Graph Session
       - Sci-kit learn type interface with `contrib.Learn`
135. 
136. TensorFlow basics

- Object/Data is called "Tensor"
- `tf.Session()` => `tensor.run()` method
- Placeholder: inserts a placeholder for a tensor that will be always fed

139. TF estimators
     - Steps
       1. Read in Data (normalize if necessary)
       2. Train/test split the data
       3. Create estimator feature columns
       4. Create input estimator column
       5. Train estimator model
       6. Predict with new test input function



