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

- If you expected the column to have unique values for each row, duplicates
  would be easy to spot -- they'd have a frequency more than 1.

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

### In R

R provides an easy way of viewing the data of all columns in a file.

    > data = read.csv('filename.csv')
    > summary(data)
           name            city          size        
    JYOTHI K :254   Delhi    : 63   Min.   :  1.010  
    CHAITHRA :196   Mumbai   : 59   1st Qu.:  5.987  
    NIRMALA  : 37   Bangalore: 53   Median : 33.085  
    ASHWINI  : 13   Ahmedabad: 33   Mean   :148.969  
    BASAVARAJ: 12   Hyderabad: 32   3rd Qu.:176.305  
    SUNITA   : 12   Kolkata  : 31   Max.   :997.690  
    (Other)  :476   (Other)  :729                    

This shows the most common elements in each column. The last column "size"
has the minimum, maximum, mean, and the quartiles.

To see the frequencies of the cities, use:

    table(data$city)

To see this sorted, use:

    sort(table(data$city))

Other common errors
-------------------

Sometimes, non-[ASCII](http://en.wikipedia.org/wiki/ASCII) characters can
cause problems. These often arise when copying from applications that convert
quotes into [smart quotes](http://en.wikipedia.org/wiki/Quotation_mark_glyphs).

In UNIX, you can use the following to match non-ASCII characters:

    grep -P "[\x80-\xFF]" filename
    
(This is not easy to do in Excel.)

Correcting errors
-----------------

Once you have identified the error, you need to correct it. In most cases,
correction is a matter of replacing wrong values with the right values.
In other cases, you may want to remove the row that contains wrong values.

### In UNIX

To remove any rows that have the word "foo", use [grep](http://en.wikipedia.org/wiki/Grep):

    grep -v foo filename.csv > output.csv

To remove any rows that contain the word "foo" in the 3rd column, use [awk](http://en.wikipedia.org/wiki/Awk):

    awk -F, '{ if ($3!="foo") print }' filename.csv > output.csv

To replace all occurrances of the word "foo" with "bar", use [sed](http://en.wikipedia.org/wiki/Sed):

    sed "s/foo/bar/g" filename.csv > output.csv

To replace the word "foo" with "bar" in the 3rd column, use [awk](http://en.wikipedia.org/wiki/Awk):

    awk -F, '{ gsub(/foo/,"bar",$3); print $3 }' filename.csv > output.csv

### In Excel

To remove values:

1. [Filter the column](http://office.microsoft.com/en-us/excel-help/filter-data-in-a-range-or-table-HP010073941.aspx) and select the rows to remove.
2. Delete the rows

To replace values in columns:

1. [Filter the column](http://office.microsoft.com/en-us/excel-help/filter-data-in-a-range-or-table-HP010073941.aspx) and select the rows to remove.
2. [Search and replace](http://support.microsoft.com/kb/288291) the wrong values with the right values

### In R

To remove any rows that contain the word "foo" in the "name" column, use:

    data = data[grep('foo', data$name, invert=T),]

To replace "foo" with "bar" in the "name" column, use:

    data$name = gsub('foo', 'barr', data$name)