
## Writing (re)usable code

- Crack open your code - can you use it again?

- Can you adapt it to modify your question, feed in new data,
and modify the scientifically-important bits easily?

---

### Functions as modular steps

The beauty and danger of any programming project is that there are many ways
to organize equivalent functionality.
While liberating, unorganized variety in your code is more difficult for 
to read and help you with, and is more difficult for future you to read
and re-use.

One way to clearly organize code is to use a "functional" approach.
This approach uses functions, as you learned a bit about on Wednesday,
to organize your code into "code blocks".
You "call" this function by giving it "arguments", and the function will
return "return values".

This approach allows you to focus your attention on making (and testing that) 
a function does one thing, and does it well.
Then later you can use these larger pieces in novel combinations to flexibly
adapt to new requirements in your analysis workflows.

For example, in one of the workshop's author's recent papers, 
they defined a function that 
[wrote out multiple ggplot formats](https://github.com/sashaflevy/PPiSeq/blob/646dbe151e7b6044e762fff1cf36b185dffe3bdc/subtree/scripts/homodimer_report.Rmd#L53)
at once so that 
[they wouldn't](https://github.com/sashaflevy/PPiSeq/blob/646dbe151e7b6044e762fff1cf36b185dffe3bdc/subtree/scripts/homodimer_report.Rmd#L188)
[have to](https://github.com/sashaflevy/PPiSeq/blob/646dbe151e7b6044e762fff1cf36b185dffe3bdc/subtree/scripts/homodimer_report.Rmd#L255)
[re-type that](https://github.com/sashaflevy/PPiSeq/blob/646dbe151e7b6044e762fff1cf36b185dffe3bdc/subtree/scripts/homodimer_report.Rmd#L311)
[code ever](https://github.com/sashaflevy/PPiSeq/blob/646dbe151e7b6044e762fff1cf36b185dffe3bdc/subtree/scripts/homodimer_report.Rmd#L328)
[again](https://github.com/sashaflevy/PPiSeq/blob/646dbe151e7b6044e762fff1cf36b185dffe3bdc/subtree/scripts/homodimer_report.Rmd#L373).

---

Functions are handy.
For example, let's calculate the standard error of a sample of values:

```{=html}
<div class="incremental">
```

```r
values <- c(4,3,2,2,5,3,6,2,2,4)
stderr(values)
```

```
## Error in stderr(values): unused argument (values)
```
```{=html}
</div>
```

```{=html}
<div class="incremental">
```

Er ... what? What does `stderr()` do?

Use the help functionality of `?` to figure out what the `stderr()` function
is built for, and if there are any other functions that calculate the
standard error of the mean.

```{=html}
</div>
```

---

It would appear that functionality is not built into base R.
Perhaps they assume that everyone will write their own standard error function?

Let's write one.

How do you calculate the standard error of the mean of a sample?
^[I always forget and look at wikipedia...]
Does that look familiar from your stats classes?

Let's code it up. I recommend you first develop the code in your functions
by playing with it interactively:

```{=html}
<div class="incremental">
```

```r
sd(values)/sqrt(length(values))
```

```
## [1] 0.4484541
```

To use this multiple times, we could just copy and paste it into each script
or workflow we want to use it in. However, this leaves lots of complexity 
for the future user to have to handle.

For example, we can copy and paste a new variable name into the code:


```r
values2 <- c(10,30,20)
sd(values2)/sqrt(length(values))
```

```
## [1] 3.162278
```

This can lead to errors in writing, small incorrect parts.
In the above example, I purposefully neglected to change the second `values`
to `values2`. Did you catch that easily, or did you have to look for it?

Additionally, how do we save it, document it, and share it?

Copy and pasting from a file is okay, but it takes up a lot more work/space.
We could email around a file of code chunks, or share on a website.

How do we make sure it works?

This is hard to do with copy-paste code chunks. Changing the chunk to work
on a new input can be an opportunity for introducing errors, typos.

```{=html}
</div>
```

---

#### Write it as a function!

This would make a useful function.

- What are the parameters? Are any defaults set?
- How does it calculate with the parameters?
- What does it return?

Once it is a function, it can be copy and pasted, 
or `source()`'d from a `.R` file.
As you develop it, you can test that it works and change inputs easily.
Later, you could change the function to fix bugs, and you don't have to then
fix all the places you copied it to - it's fixing the code _inside_ 
the function. 

```{=html}
<div class="incremental">
```

```r
sez <- function(x) {sd(x)/sqrt(length(x))}
sez(values)
```

```
## [1] 0.4484541
```

```r
sez(rnorm(10))
```

```
## [1] 0.3511155
```

```r
sez(rnorm(100))
```

```
## [1] 0.1018274
```

```r
sez(rpois(1e3,3))
```

```
## [1] 0.05605128
```
```{=html}
</div>
```

---

### Tips for modular workflows

- Try to not "hardcode" things - if it's a number, consider if it can be a
    *parameter* that is "passed in" as an argument.

- Group repeated code functions - some folks say you should never repeat
    code (but do what works for you!).

- Try to read inputs and outputs as general, flexible formats -
    strings of filenames, vectors of values

- Write a comment at the top of the function that says what it's doing
    and what to expect, generally comment things.

Consider, the `list.files()` and `hist()` functions are already built this way!

---

### Apply is a popular tool

Apply is another common way of doing something over and over.
It is a very compact way to take pieces of a list, vector, dataframe, or matrix
and put them into a function. There are:

- `apply` - for 2D objects
- `lapply` - for lists and vectors
- `sapply` - is `lapply` but with simplified returns
- `mapply` - is for combinations of multiple variables
- `replicate` - calls a function multiple times

You would be helped by knowing what these are, roughly what the idea is, 
and how to read other code that has these.

---

Some people *strongly* prefer coding this way. Here is an example:


```r
list_of_protein_files <- lapply(
        list.files("data/viral_structural_proteins/",full.names=T),
        read.delim,
        sep="\t",header=F)
```

These all:

- take some variable with multiple values, either along a list or along the
    rows or columns of a data.frame/matrix
- sticks each one of these into a function
- returns the values, often as a list (`unlist()` is helpful here)

Checkout one of the help pages (`?lapply`) to see specifics.

These are similar to the `Map()` function and "map" functional programming
in other languages.
You don't need to program this way, but there are some advantages to this
style if you'd like to learn.

---



---

### Learn from examples

Let's look at two chunks of code from 
[a paper](https://github.com/sashaflevy/PPiSeq) (lightly edited).
The experiment is counting barcoded lineages of yeast cells to estimate
"PPIs" (protein-protein interactions)
^[Ask Darach if you want details, or [read the paper](doi.org/10.7554/eLife.62365)]

Here's an example of one style of writing R script:


```r
# filter out bad barcode lineages ( <= 2 time points counts > 0, or maximum of each time point <= 5 or total counts of a lineage < 10)
bad_index = rep(0, nrow(DBC_known_counts))
for(i in 1:length(bad_index)){
        counts = as.numeric(DBC_known_counts[i, 4:8])
        if (length(which(counts != 0)) < 3 | max(counts) < 5 | sum(counts) < 10){
                bad_index[i] = 1
        }
}
length(which(bad_index == 1)) # 1447775
```

<!-- exercise -->

- What is going on here?
- How do you feed in new data?
- How do you run this multiple times?
- How do you change the logic?

---

Here's another example from the same author:


```r
Random_reference <- function(PPI_multiple, all_PPI_filtered, size, sampling_number){
  PPI_PPiseq = PPI_multiple[,1]
  RRS_size= 0
  RRS= character(length=0)
  while (RRS_size < size) {
    yeast_PPI_random= all_PPI_filtered[sample(1: length(all_PPI_filtered), sampling_number, replace=F)]
    RRS= unique(c(RRS, yeast_PPI_random))
    RRS_overlap= intersect(RRS, PPI_PPiseq)
    RRS_size= length(RRS_overlap)
  } 
  RRS_duplicate_marked = mark_duplicates_fast(RRS_overlap)
  return (RRS_duplicate_marked[,1])
}

# ... and later ...
Random_reference(PPI_multiple, all_PPI_filtered, number_PPI, sample_number)
```

<!-- exercise -->

Same questions.

---

What are some differences?

What's useful for you?

What's a lot of effort to do?

---

### Style guides can be inspiring

What's your coding style going to be?

- [tidyverse style guide](https://style.tidyverse.org/)
- [google-specific changes](https://google.github.io/styleguide/Rguide.html)
- [Jean Fan's](https://jef.works/R-style-guide/)
- search for "R style guide"

Be inconsistently consistent! Do what works!

Balance for yourself:

- How easy is it to write?
- How easy is it for you to read?
- How easy is it for others to read?
- How similar is it to what everyone else is doing (a very good thing)?

But most importantly, use what folks around you are using.
Be lazy and copy others!

---

<!-- exercise -->

- How would you describe your code writing style?
- How do you name variables, functions?
- How good are your comments?
- Are there any ideas here you would like to incorporate?

---



<!--

#### Let's write a plotting module/function

How about a `ggplot2` style boxplot?


Write a function that makes a ggplot2 boxplot for some numbers.

What should the function take, what should it do?

What is "some numbers"?

What should it return?

[NOTE]: calling ggplot inside a function may not print, you may have to
    wrap it in `print(ggplot code)` or return the ggplot object as
    a variable

---

### More complex workflow

Let's write a simulation of viral evolution.

(could be a bad idea, considering....)

More examples/exercise

show a simulation of something???. genetic drift of a virus replicating?

lineage G1312F

exercise - wrap the entire analysis as a function

talk about ease of calling

ease of tweaking this

exercise - break into subfunctions, generate and plot

ease of changing models

---

-->

