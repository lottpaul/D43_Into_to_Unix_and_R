Stream Redirection and Pipes are the secret sauce of Unix. In the previous lessons, we've become familiar with some of the Unix's simple tools - but alone they're not all that exciting because we've only been outputting the results to the Terminal screen.

With stream redirection, we can read data from a file or write, or append our results to a file. Pipes allow us to directly pass our results from one program to another without needing to save any intermediate files.

Piping allows the STDOUT from one command to be passed to another command as it's STDIN

The means that we can now link simple commands like `cat`, `zcat`, `cut` , `sort`, `wc`, `head`, `tail` to do more complex tasks. We'll also learn about some Unix Power Tools in the next lesson that will expand the power of Unix even more.

| Operator | Description |
| --- | --- |
| &lt;command&gt; > &lt;file&gt; | Write STDOUT output from command to a file (overwrites the contents of file) |
| &lt;command&gt; >> &lt;file&gt; | Append STDOUT output from command to a file |
| &lt;command&gt; &lt; <file&gt; | Read file contents and pass to the command as STDIN |
| &lt;command&gt; 2> &lt;file&gt; | Redirect STDERR to a file |
| &lt;command1&gt; \| &lt;command2&gt; | Pipe the STDOUT output from the first command to the STDIN of the second command |

* * *

# Very Important Message: Bioinformatics/Piping Rules

## Rule 1: Understand your File Format

It's imperative that you understand your data file, before you start to process it. You should always view your file first, and try to understand the file format before you start to process the file.

### Example 1: How many lines are in the file D43_Unix/Data/Fertility.csv.gz

See: **D43_Unix/Data/Fertility.txt**  
See: https://search.r-project.org/CRAN/refmans/AER/html/Fertility.html

```
$ wc -l D43_Unix/Data/Fertility.csv.gz
5021

$ zcat D43_Unix/Data/Fertility.csv.gz | wc -l
254655
```

* * *

## Rule 2: Build your commands, a command at a time.

When piping multiple commands together, start with the first command and ensure that it's correct before proceeding pipe to second command - otherwise, you won't know where you went wrong and it's much harder to try and figure out at which stage(s) there is an error

### Example 1: How many different labels are there in the `sex` column?

We want to find the number of different labels used in the `sex` column in the penguins dataset.  
https://allisonhorst.github.io/palmerpenguins/articles/intro.html

```
$ cut -f8 D43_Unix/Data/penguins.csv | uniq | wc -l
345

$ cut -f8 D43_Unix/Data/penguins.csv | less

$ cut -d ',' D43_Unix/Data/penguins.csv | less

$ cut -d ',' D43_Unix/Data/penguins.csv | uniq | less

$ cut -d ',' D43_Unix/Data/penguins.csv | sort | less

$ cut -d ',' D43_Unix/Data/penguins.csv | sort | uniq | less

$ cut -d ',' D43_Unix/Data/penguins.csv | sort | uniq | wc -l

```

These two rules are not only important rules to follow when you're piping Unix commands, but also anytime you're programming, processing files, or doing analysis.

* * *

# Exercise 1: Use `cut` to extract the columns: Park Code, Park Name, and State from the file `D43_Unix/Datasets/parks.csv`. Use the `<` operator to read `D43_Unix/Datasets/parks.csv`. Then, write the output to a file called `parks_ids.txt` using the `>` stream redirection operator.

```bash
$ less parks.csv
Park Code,Park Name,State,Acres,Latitude,Longitude
ACAD,Acadia National Park,ME,47390,44.35,-68.21
ARCH,Arches National Park,UT,76519,38.68,-109.57

$ cut -f1,2,3 parks.csv | head

$ cut -d , -f1,2,3 parks.csv | head

$ cut -d , -f1,2,3 < parks.csv | head

$ cut -d , -f1,2,3 < parks.csv > parks_ids.txt

$ less parks_ids.txt

```

* * *

# Exercise 2: Sort `D43_Unix/Data/penguins.tsv` first by sex and then numerically ascending order by body_mass_g. Finally, pipe `|` the output to a `cut` command where you extract the 1st column and write the output to "order.txt" using `>`

```bash
$ less penguins.tsv
rowid   species island  bill_length_mm  bill_depth_mm   flipper_length_mm       body_mass_g  sex     year
1       Adelie  Torgersen       39.1    18.7    181     3750    male    2007
2       Adelie  Torgersen       39.5    17.4    186     3800    female  2007

$ sort -k8,8 penguins.tsv | head

$ sort -k8,8 -k7,7n penguins.tsv | head

$ sort -k8,8 -k7,7n penguins.tsv | cut -f1 | less

$ sort -k8,8 -k7,7n penguins.tsv | cut -f1 > order.txt

```

* * *

# Exercise 3: Run the following command `ls-alh`. Write the error message to a file named `error.log`. Verify that the error message was written to the file.

```bash
$ ls-alh
ls-alh: command not found

$ ls-alh 2> error.log

$ less error.log
```