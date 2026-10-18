# T7: ENH: Make DataFrame.filter accept filters in new formats
Source: https://github.com/pandas-dev/pandas/issues/61317
Fetched via GitHub API | Original issue plus comments, unedited

## Issue body (@datapythonista)
I think it'd be very nice for users to get this working regarding [filter](https://pandas.pydata.org/docs/dev/reference/api/pandas.DataFrame.filter.html):

```python
df.filter(df["age"] > 18)  # same as `df[df["age"] > 18`

df.filter("age > 18")  # same as `df.query("age > 18")`, I think `.query` should be deprecated if this is implemented in `.filter`

df.filter(lambda df: df["age"] > 18)  # same as `df[df['age'].apply(lambda x: x > 18)]`, useful for method chaining
```

I think implementing this is reasonably simple. I think the main challenge is how to design the API in a way that filter can be intuitive and still work with the current parameters. And particularly, keeping backward compatibility. But personally, I think this would be so useful, that worth finding a solution.

CC: @rhshadrach 

## Comment (@rhshadrach)
Partial proposal:

  - Accept `items` (will maybe want to rename this argument?) of type:
     - Series (will align on index)
     - Non-Series list-likes (must be same length as df)
     - strings a la `query`
     - UDFs to be discussed.
   - Deprecate `like`, `regex`; offer no alternatives.
   - Deprecate `axis=1` but add `DataFrame.select` (somewhat talked about in https://github.com/pandas-dev/pandas/issues/55289).

For UDFs, it seems to me that the usage in the OP can be readily handled by `pipe`. I would more expect passing a UDF to `filter` would operate row-wise similar to `apply(..., by_row=True)`.

Another question is how strict we are on the values that will be filtered. Do we require these to be `bool/np.bool_`, or do we allow any value an internally pandas will evaluate the truthyness of it. I would lean toward the latter.



## Comment (@datapythonista)
I like the idea.

If I understand correctly, the main use of `df.filter(cond)` where cond is a `Series` will be equivalent to now use `df[cond]`. I think implementing the `like` and `regex` behaviors would be trivial with `df.filter(df["col"].str.contains("xxx"))` and same for `regex`, right? It does feel we're offering a very reasonable alternative.

I see your point for using `.pipe` to filter, and in a way kind of agree. But it feels like `df.filter(lambda x: x["age"] > 18)` will be together with `df.filter(df["age"] > 18)` the most common used case by far. While `df.pipe(lambda x: x[x["age"] > 18)` may seem a reasonable alternative, I think it will really make users' life easier to support the former, as I think it's way more intuitive.

In any case, what you propose seems like a great improvement.

## Comment (@rhshadrach)
> I think implementing the `like` and `regex` behaviors would be trivial with `df.filter(df["col"].str.contains("xxx"))` and same for `regex`, right? It does feel we're offering a very reasonable alternative.

Agreed - I should have said no _new_ alternatives. :laughing:

For UDFs, one reason not to have `df.filter(lambda x: x["age"] > 18)` operate by row is that it is effectively a transpose (`x` being a Series means it can only have one dtype), one of the behaviors I would love to remove from pandas across the board. Another is that `agg`, `apply`, `transform` all pass columns (vertical) objects into the UDF. While it doesn't make sense for `filter` to act column-by-column, passing the entire DataFrame seems closer in behavior than operating horizontally.

However I do not find it intuitive that in `df.filter(lambda x: x["age"] > 18)` the `x` is the same as `df`. I agree in the utility of having this for method chaining, but I immediately think `x` as being a component (element / column / row) of `df` instead of the entire thing. Perhaps that's just me?

A bit of restatement of my previous post, but it seems like `df.filter(lambda x: ...)` acting by row provides new functionality otherwise not readily available (I think?) where as having `x` be all of `df` is very close to duplicating `pipe`.

Finally, if we are to have `x` be the same as `df` in this case, what is the validation on the result? Must it be a Series with the same index as `df`, or are we going to allow alignment. Can users returns list-likes of the same length?

Overall, I lean toward operate by-row here, but not strongly.

> I think `.query` should be deprecated if this is implemented in `.filter`

I agree, but desire the deprecation would be slow. That is, first introduce filter and change the docs to discourage the use of `query`. Then after 1 or 2 years, start the deprecation process.

cc @pandas-dev/pandas-core for any thoughts.

## Comment (@simonjayhawkins)
> Overall, I lean toward operate by-row here, but not strongly.

`DataFrame.filter` does not filter a Dataframe on its contents, the filter is applied to the labels of the index. The suggestion in the OP is to essentially add value based conditional filtering to this method.

If you operate by row, (or by column if the axis argument is retained), then if you passed a Series with the Series.name set to the index label then it would be easier to filter based on the index label and thereby potentially justify the removal of  `like` and `regex` and offer no alternatives?



## Comment (@datapythonista)
I see your point @rhshadrach, and I think what you propose is very reasonable and maybe even thr best option in theory.

In practice, I would be very surprised if most users don't find the pyspark-like API of the function receiving the whole dataframe more intuitive. See [this example](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.filter.html) in their docs:

```python
df.filter(df.age > 3).show()
```

We can't compare directly with a lazy API, but I think what I propose is quite similar to this.

Also, it was discussed before about adding `pandas.col("my_col")` to avoid the lambda. I guess that would look like:

```python
df.filter(pd.col('age') > 3)
```

Personally if filter will accept both this expression and a lambda, I think it's way more clear and intuitive that the lambda works the way I described.

Let's see what other people think, maybe what's clear and intuitive to me it's not to others.



## Comment (@Dr-Irv)
Maybe I'm missing something, but why deprecate `query()`.  I have LOTS of code that uses that.

Why not leave `filter` as is - it operates on labels - and maybe expand `query()` to take expressions as proposed here.

So that `df.query(df["age"] > 18)` and `df.query("age > 18")` would do the same thing


## Comment (@datapythonista)
That's a reasonable option. I think filter is more clear, and is what everybody else is using. If we were to implement the API from scratch now, I think it would be the obvious choice. For backward compatibility query may be better, and we can surely consider it. But I would rather have a very long deprecation timeline, than keep the API IMHO wrong because of a choice we did that now is not ideal.

## Comment (@rhshadrach)
> Why not leave filter as is - it operates on labels

Because it's at odds with other DataFrame libraries.

  - PySpark: https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.filter.html
  - Polars: https://docs.pola.rs/api/python/stable/reference/dataframe/api/polars.DataFrame.filter.html
  - ibis: https://ibis-project.org/reference/expression-tables.html#ibis.expr.types.relations.Table.filter
 
The exceptions are Modin and dask, but I think they were designed to model the pandas API.

In addition, I would argue `query` is an odd choice of a name for a filtering method.

I'm fine with leaving query as it is for a long time, including indefinitely.

## Comment (@Dr-Irv)
> > Why not leave filter as is - it operates on labels
> 
> Because it's at odds with other DataFrame libraries.

But those libraries were introduced _after_ `pandas` !  So shouldn't THEY be modifying _their_ API's?  (I say this somewhat facetiously)

> In addition, I would argue query is an odd choice of a name for a filtering method.

Then maybe you think that SQL (Structured Query Language) should be called SFL (Structured Filtering Language) ?  (definitely said facetitously)

To me `query` is appropriate if  you think of it from the perspective of how SQL works.



## Comment (@datapythonista)
For a method that returns a subset of rows based on a condition, I think the standard terminology is filter. Query seems more appropriate for a more complex expression that can get data doing operations that not only involve a filter. I think SQL is consistent with this, since it allows to do more than the WHERE clause. And I think query feels inappropriate. `df.where` would be consistent with SQL, but to me filter is clearly the right choice.

## Comment (@rhshadrach)
> To me query is appropriate if you think of it from the perspective of how SQL works.

Queries in SQL can do so much more than filter. `DataFrame.query` can only filter. I think this is supporting my contention that `query` is an odd name.

## Comment (@simonjayhawkins)
> Why not leave `filter` as is - it operates on labels - and maybe expand `query()` to take expressions as proposed here.

That makes sense to me as we would not be mixing label based "filtering" with value based "filtering"

> I think filter is more clear, and is what everybody else is using. If we were to implement the API from scratch now, I think it would be the obvious choice.

This also makes sense to me.

So the issue is how to make this transition. If we don't mix the label based and value based "filtering" this surely makes the transition path more difficult.

I'm still not clear how we keep the current label based filtering functionality if we "Deprecate like, regex; offer no alternatives." When we deprecate we say something like "x is deprecated. use ... instead". @rhshadrach can you clarify what the ... would be?

## Comment (@datapythonista)
> When we deprecate we say something like "x is deprecated. use ... instead". [@rhshadrach](https://github.com/rhshadrach) can you clarify what the ... would be?

`df.filter(df["col"].index.str.contains("xxx"))` (or same with square brackets)... this was discussed above in one of the many replies, Richard meant no new specific alternative, and all the existing filter method funcionality is already possible (and personally I'd bet that the alternative using a boolean mask based on the index attribute may already be more popular than the filter method.

## Comment (@simonjayhawkins)
> and personally I'd bet that the alternative using a boolean mask based on the index attribute may already be more popular than the filter method.

Yes, boolean indexing is one of the core strengths of pandas and remains the best practice for filtering data. Its clarity and explicit nature make it ideal for developers who want to see exactly which rows or columns are being selected—for example, using expressions like `df[df['age'] > 18]` directly leaves little room for ambiguity.

In contrast, convenience methods like `DataFrame.filter` and `DataFrame.query` were originally designed to offer syntactic sugar for specific filtering operations that might be less straightforward with boolean indexing. However, extending these methods to incorporate functionality already achievable through boolean indexing creates a duplicate API. This duplication tends to blur the clear separation of concerns: boolean indexing for explicit condition-based filtering, and the convenience methods for more specialized use cases such as label-based filtering or evaluating query expressions.

From the perspective of user-friendliness and maintainability, especially for newcomers, it is perhaps more intuitive to keep these methods unchanged. Retaining their original, focused design helps avoid confusion. New users won't have to decide between multiple approaches for the same operation, and experienced users can continue to leverage boolean indexing as a robust tool for data selection. Moreover, a stable and clear API encourages better code clarity and consistency, both of which are essential for long-term maintainability.

In summary, while extending these convenience methods might seem like a way to offer more flexibility, doing so risks introducing unnecessary redundancy and potential confusion. Maintaining the current API allows developers to choose the most appropriate filtering method—whether it's the explicit power of boolean indexing or the specialized convenience of `filter` and `query`—without overlapping functionality?


## Comment (@datapythonista)
I think the existing API is already duplicated, as you mention, filter is syntactic sugar for 3 very particular use cases (I personally never used).

I don't think the square brackets is a good API for method chaining, so I'm happy with the duplication after the changes proposed here.

Also, after having used both pyspark and polars, I find the filter method with a condition one of the essential functionality of a dataframe library. If we manage to implement the syntax below, I think it'll be the most important and convenient API change to pandas since I started contributing:

```python
df.filter(pd.col('age') > 3)
```

Of course other will have different points of view, but for a large amount of our user base I think this would be a huge improvement. And as a first step it needs the changes proposed in this issue.

## Comment (@simonjayhawkins)
> And as a first step it needs the changes proposed in this issue.

If DataFrame.filter did not already exist and do something different it would definitely be more straightforward to implement this.

> I think the existing API is already duplicated, as you mention, filter is syntactic sugar for 3 very particular use cases (I personally never used).

I don't disagree. Let me think on this some more.

> I don't think the square brackets is a good API for method chaining, so I'm happy with the duplication after the changes proposed here.

noted. 


## Comment (@simonjayhawkins)
for some additional context, it seems we are covering some of the same ground as #12401

## Comment (@simonjayhawkins)
pinging @jorisvandenbossche for input as participant https://github.com/pandas-dev/pandas/issues/12401 and the follow up open issue #26642.

I'm guessing from https://github.com/pandas-dev/pandas/issues/26642#issuecomment-511080719 that @jorisvandenbossche may want to retain the syntatic sugar that .filter offers but is not adverse to renaming the method.

## Comment (@Dr-Irv)
This is an interesting discussion, and I'm glad some of the old issues were linked in to give a historical context.  Here is how I see things (and feel free to disagree):

1. Way back when, `pandas` had a `DataFrame.select()` method and `DataFrame.filter()` method, both which operated on labels.  `DataFrame.select()` was deprecated and removed because of the functionality of `DataFrame.loc` and `DataFrame.filter()`.
2. There has been debate over time regarding the various ways people can filter data based on labels as well as data.  Right now, there are 3 ways:  `DataFrame.filter()` allows filtering on labels, `DataFrame.loc` allows including an expression that creates a boolean mask, and `DataFrame.query()` allows arbitrary expressions that allow filtering on _both_ labels and data.
3. In the meantime, libraries like `polars` and `pyspark` have introduced a `filter()` method that allows filtering on data.

So a question here is whether we change `DataFrame.filter()` to have functionality similar to what is used in `polars` and `pyspark` , and how to create a transition path for those using `filter()` for filtering on labels.  

One thing that I want to point out about `query()`, which I find very useful, is that it allows filtering on a combination of labels and data.  

In his comment at https://github.com/pandas-dev/pandas/issues/61317#issuecomment-2831973882, @datapythonista suggested this kind of syntax:  `df.filter(df["col"].index.str.contains("xxx"))`.  If the index were named "foo", I would use `df.query("foo.str.contains('xxx')")`, which, at least for me, is a lot cleaner.  When I have a `DataFrame`, typically with a `MultiIndex`, I think of the labels in the `MultiIndex` as data so I can do queries that combine both expressions on labels in the index with expressions on data in the columns.


## Comment (@rhshadrach)
> One thing that I want to point out about `query()`, which I find very useful, is that it allows filtering on a combination of labels and data.

`.filter` will behave exactly the same on string input. And I am fine with keeping `query`. I have a bit of a preference to have a long period of discouraging it and then deprecating, but I'm fine with it remaining an alias for `filter` (on string inputs) indefinitely.

> how to create a transition path for those using `filter()` for filtering on labels.

I would propose to add a new argument `cond`. Users can either specify some nonempty subset of `{items, like, regex}` or `cond`, but not both. We can then introduce the deprecation on specifying the former.

## Comment (@jbrockmendel)
I'm squeamish about overloading, in particular the string case.  In the last dev call we talked about nudging people away from query/eval and toward pd.col.  Instead of `df.filter("age > 3")` can we just support `df.filter(pd.col("age") > 3)`?  Its slightly longer, but the maintenance-wise I think much simpler.

## Comment (@rhshadrach)
In addition to maintenance, it has no odd edge cases (e.g. integer column labels) and gets away from the pandas-specific `@` syntax as well (which can trip up linters on variable usage). One can also use f-strings in certain cases with query, e.g.

```python
mylist = ["a", "b", "c"]
df.query(f"x in {mylist}")
```

but this only works depending on the string representation, where as `pd.col("x").isin(mylist)` does not.
