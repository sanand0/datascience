Data cleansing
==============

Often, the data you have will have errors, and you'll need to clean it up.
Missing data, inconsistent labels, duplicates and misspellings are common
examples of errors. The [Wikipedia article on data
cleansing](http://en.wikipedia.org/wiki/Data_cleansing) talks more about this.

Finding errors
--------------

A common technique to identify data errors is to look at the frequency of
values in each column. Based on what the column is, this would tell you the
nature of errors.

For example,

- If the "Gender" column contains values:

        M       80
        F       60
        Male    15
        Female  12
        -       8

  you know there are inconsistent labels ('M' and 'Male' are both used for
  males) as well as 8 rows of missing data.

- If the "District" column contains over 15 values when you know there are
  only 12 districts, it's obvious that there is a mismatch.

- If there's more than 1 person with the same mobile number, there is a
  potential duplication problem.

- If a large number of students have a mark of 'AAA' or '888', it probably
  indicates a missing value. Perhaps the student didn't write the exam.

- If there are people with an age of -1, that's probably an error, or
  indicates a missing value.

- If you spot people with the name 'John Smith' and 'John A Smith', there is a
  chance that these could be the same person.

To find such errors, you could take each column, find the frequency of each
value, and sort the result both by the column and by the frequency. Sorting by
the column helps spot spelling mistakes and similar names. Sorting by
frequency helps find whether some values are very common or uncommon.

### In UNIX

    # Extract the first column
    cut -d, -f1 filename.csv > column-1.csv

    # Find the frequency, sorted by column
    sort column-1.csv | uniq -c

    # Find the frequency, sorted by frequency
    sort column-1.csv | uniq -c | sort -k 2n

### In Excel

1. Create a [pivot table](http://office.microsoft.com/en-us/excel-help/pivottable-reports-101-HA001034632.aspx)
   with the column you're investigating as a row, and the "Count" of the
   same column as values.
2. Right click on the column to sort by the column name.
3. Right click on the "Count" column to sort by frequency.


Finding duplicates
------------------


Correcting errors
-----------------
