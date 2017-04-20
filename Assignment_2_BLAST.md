# Template for BLAST, orthologs, and paralogs (Week 2, 20 pts possible)

__Name:__ Eliza Tsang

__Student ID:__ 999606858

*_Please answer the following questions which are each worth 2 points. Be clear and concise with any written answers. Late assignments will be penalized with a 2pt deduction without prior communication of extenuating circumstances._*

---------------------------------

__Q1__.  Write a for loop to pluralize peach, tomato, potato (remember that these end in "es" when plural).  Provide the code below.  (See "for loop" lab)

*set fruits peach tomato potato*

      for fruit in $fruits
          echo {$fruit}es
    end

__Q2__.  In your own words provide a "translation" of the loop below (from "for loop" lab)

    for file in $myfiles
        cat $file
    end

For all the files in myfiles, read and display the contents of the files.


__Q3__. Create a table that indicates blastp runtime as a function of
threshold value.  Is there a linear relationship?  Why does does
changing the threshold change the search time?

|Threshold |Time|
|:-: |:-: |
|10 |0:01.11 |
|11| 0:00.72|
|12|0:00.51|
|13|0:00.39|
|14|0:00.32|
|15|0:00.30|

The time taken is not linear but exponential (decreasing with increasing threshold value). The higher the threshold, the faster blastp takes to complete, likely because it is not extending as much, and can disregard matches that don't meet the threshold requirement. It is more stringent.

__Q4__.  How much faster is blastp than water under default parameters?

In a single gene to proteome search, water took over 4 minutes, while blastp took less than a second.

To do a whole proteome to proteome search, water is estimated to take about 2 weeks, while blastp uses between 6-9 hours.

__Q5__.  What search parameters (matrix and threshold) would you use to
search two proteomes? Justify your answer.

To lower the time needed to search through, I'd use a higher threshold than default. As a result of the higher threshold however, I may not get as many results because the search is stricter in match scores.
(Also, with respect to word size: increasing the word size means it requires more consecutive matches so I'd get more accurate results, but I may not get many hits in return. Decreasing the word size allows the search to find other similar matches but it may not be as exact.)

In the current search, however, I simply used the default settings to get a good idea of how a blastp search and result is in general.

__Q6__.  How long will it take to search two proteomes (given the above)?

It took 7 hours and 15 minutes for my search.

__Q7__.  Using E-value to determine the best match, find a worm gene with a single fly ortholog and record their names and the reciprocal E-Values.  Is there more than one fly "hit" for your worm gene?  If so how do you decide if there is a single orthlog?

I used *-culling_limit 1* to get a single best match.

Our respective top matches did not result in an ortholog pair. However, we found a very similar match:

FBpp0078013 and	B0285.1a

The E-value was about: 2e-137


__Q8__.  Discuss what makes orthology difficult to determine by the reciprocal BLAST method.

Genes are considered orthologs if they are each other’s best matches.
If not, there must be some complexity and then we can't define each other as orthologs. There also could be a situation where we could get reciprocal hits but be wrong.

It would be helpful to build trees, but for a whole proteome that would be too much.

__Q9__.  How many paralogs did you find for T21B10.2a and for B0213.10?  What criteria did you use?

For B0213.10, I found 69 (individual) paralogs with an E value between 0 and 1e-10.
For T21B10.2a, I only found one with an E value between 0 and 1e-10.

I used the default threshold and word size.

__Q10__.  Is alignment score sufficient for determining orthology or paralogy? What other sources of information from the BLAST output might be useful?  What other types of analyses would be helpful?

It may have been a while since the organisms separated. Perhaps look at not just alignment score but see if there's an alignment across whole gene.
A phylogenetic analysis could also be helpful.

--------------------------------------------

Add and commit changes for both this document and your lab notebook to the repository.  Push to GitHub and set an issue to indicate that you are ready for this to be graded.
