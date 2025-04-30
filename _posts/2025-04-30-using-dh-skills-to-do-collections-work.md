---
layout: single
title:  "Using DH skills to do collections work"
date:   2025-04-30 11:30:00 -0300
comments: true
categories: 
    - librarianship
---

I recall once seeing a CFP about applications of digital humanities methods to (traditional) library work. Librarians are a notoriously risk-adverse group, and this call seemed oriented towards proving that the scary new thing could actually help our conservative profession do its bread-and-butter work. DH; it's not so bad! I recall thinking at the time that this was sort of an interesting idea, that I had certainly done it, but nothing that rose to the level of a research article or conference paper. This post falls into the category of ordinary librarian task made easier by DH. Since libraries, and higher ed more generally, are once again facing alarming headwinds, and I'm hearing calls to "get back to basics," and just not do the more advanced research library stuff for a while, I thought it might be advantageous to show in some detail how DH-y or data librarian-y skills can help with the more traditional subject librarian portfolio. [Matthew Lincoln](https://mstdn.social/@mdlincoln) once quipped on the old site that 90% of DH was a table join, and I laughed because it was true, especially in the broader sense of comparing these data to those other data and drawing new insights from the juxtaposition. This post, however, is about a literal table join, lol.

Since summer is around the corner, and I have fewer course and workshop requests, I am turning my attention to collections weeding, which is to say pruning older, disused books to make way for newer materials. In addition to being a DH librarian, I am also a subject liaison to various language and literature fields, which means the responsibility for our congested P class falls more or less squarely on my shoulders. This section of the stacks has been a problem for many years, but since we also loan more P classified volumes than any other Library of Congress class, it never rose to the level of a crisis in my mind. Although lately our space issues have been multiplying, hence the added incentive to give it a look. Our print retention guidelines have this to say about withdrawing volumes:

> A last copy book may be withdrawn if the book is of very low use and perpetual access to the electronic or digitized version is available in hathitrust.org or if at least five copies are available for borrowing in North American libraries identified in WorldCat. Library- and subject-specific criteria may be used to identify very low use books.

I figured I'd be able to find large numbers of our P class volumes in HathiTrust if they had fallen into the public domain, which is to say published before 1930 as of this year. I also wanted to make sure that they hadn't been checked out or used in house in a long while too, since just because something is in the public domain doesn't mean that readers don't want the print. In my experience, there are readers in every generation that favor print to digital for difficult, cognitively demanding reading. I did an analysis of our EZBorrow (ILL) data once and was stunned that Shakespeare regularly turned up in the most requested P class authors.

{% include figure popup=true image_path="/assets/images/pclass_authors.png" alt="prototype with subject tags" caption="Most requested P class authors. EZBorrow data, requested from Rutgers University." %}

Specifically, these are the criteria I used to pull volumes:

- Alexander Library (this is our main library for the humanities and social sciences)
- Open stacks (holding location for books that circulate; I didn't want to draw volumes from any holding location that was historically non-circulating since the usage statistics would be artificially depressed)
- Library of Congress call number between P1 and PZ90 (languages and literatures, literary criticism too)
- Loaned and/or used in house fewer than 5 times total
- Last loan date earlier than July 31, 2013 (these last two data points were somewhat arbitrarily chosen to indicate low use)
- Publication date is earlier than 1930 (in other words, pre-copyright)

I ended up with a list of 5,147 volumes. Next, I needed to figure out how to compare these volumes to the HathiTrust Digital Library holdings in full view. To do a table join, you will want to find a shared column of data between the two datasets. It's much less error-prone to do this with a numeric identifier, rather than with a title, say, since a one-character difference makes a new title to the computer. After fumbling around for a bit, I figured out that we mostly had OCLC numbers for our books and so did HathiTrust. Hurray!

HathiTrust regularly publishes updated bibliographic data for their holdings called [hathifiles](https://www.hathitrust.org/member-libraries/resources-for-librarians/data-resources/hathifiles/). This is an incredibly wonderful resource for all kinds of bibliographic analysis. With that said, HTDL currently has over 18 million volumes, and these files are LARGE. They politely say on the site that the files "may be difficult to open with standard spreadsheet software or text editors," which is something of an understatement. My MacBook Pro was definitely not up to the task, so I started by using the command line program `awk` to filter for only those HT volumes in the public domain. I knew from [HTDL's rights code attributes list](https://www.hathitrust.org/the-collection/preservation/rights-database/#attributes) that I just needed the volumes marked "pd" and "pdus" (I didn't look at the cc licenses because they usually indicate a newer work).

|htid               |access |rights | ht_bib_key|description |
|:------------------|:------|:------|----------:|:-----------|
|mdp.39015018415946 |deny   |ic     |          1|v.5         |
|mdp.39015066356547 |deny   |ic     |          1|v.1         |
|mdp.39015066356406 |deny   |ic     |          1|v.2         |
|mdp.39015066356695 |deny   |ic     |          1|v.3         |
|mdp.39015066356554 |deny   |ic     |          1|v.4         |
|uc1.$b759626       |deny   |ic     |          1|v. 1        |
|uc1.$b759627       |deny   |ic     |          1|v. 2        |
|uc1.$b759628       |deny   |ic     |          1|v. 3        |
|mdp.39015033913115 |deny   |ic     |          2|            |
|mdp.39015061455294 |deny   |ic     |          3|            |

In looking at the first few columns of the HTDL metadata, I saw that the values I needed were in the third column ("rights"). IC stands for in copyright, which is true of a majority of HTDL volumes.

```
awk -F "\t" '{ if($3 ~ /pd|pdus/) { print >> "hathi_full_pd.txt" }}' hathi_full_20250401.txt
```

Above is the command I used to tell `awk` that the input file is tab delimited (`-F` stands for field separator), and that any rows with "pd" or "pdus" values in the third column should get put into a new file called "hathi_full_pd.txt."[^1]

Next, I ran the following command to see how many rows that left me with.

```
 wc -l hathi_full_pd.txt
7745163 hathi_full_pd.txt
```

Seven million plus rows is still too many for me to bother trying to open the file on my personal laptop. After experimenting a bit with the sampling methods in R, I decided to segment the whole dataset using a unix command line utility called split.[^2] I made a somewhat arbitrary choice to split the file every 250 thousand rows using the following command. This resulted in 31 files named segmentaa, segmentab, segmentac, and so on. Now I could finally begin to manipulate the HTDL data using the R programming language on my laptop.

```
split -l 250000 hathi_full_pd.txt segment
```

Loading the data into my RStudio session looked like this. HathiTrust publishes their hathifiles without a header row, so while I'm reading in all those segment files, I am simultaneously adding a header row using a function. The resulting objects in my environment that I am going to interact with are called `alex_pclass_oclc` and `htseg01` through `htseg31`.

```r
# load library data
alex_pclass <- read_csv("alex_alstackc_pclass_fewerthan5circs_pre1930pub_oclc.csv")
alex_pclass <- rename(alex_pclass, oclc = `OCLC Control Number (035a)`)
# some don't have oclc numbers and will have to be manually checked later
alex_pclass_oclc <- alex_pclass %>% filter(!is.na(oclc))

# HTDL field names. I made one small change from the HTDL and am calling the column "oclc" instead of "oclc_num" to be able to write a shorter join function
headers <- c("htid","access","rights","ht_bib_key","description","source","source_bib_num","oclc","isbn","issn","lccn","title","imprint","rights_reason_code","rights_timestamp","us_gov_doc_flag","rights_date_used","pub_place","lang","bib_fmt","collection_code","content_provider_code","responsible_entity_code","digitization_agent_code","access_profile_code","author")

# function to read file with some arguments and add headers
ht_read <- function(file_path, column_names) {
  data <- read_delim(file_path, col_names = FALSE, delim = '\t', col_types = cols(.default = "c"))
  colnames(data) <- column_names
  return(data)
}

# read all the HT segment datasets in as a list of dataframes and explode into individual dataframes
files <- Sys.glob("segment*")
file_list <- lapply(files, function(file) {
  ht_read(file, headers)
})
names(file_list) <- sprintf("htseg%02d", 1:length(file_list))
list2env(file_list, envir = .GlobalEnv)
```

While attempting to convert the OCLC numbers to a numeric data type, I discovered that the HTDL sometimes stores more than one OCLC number per cell. This will interfere with my table join, so I need to split up those values into separate columns. For the time being, I decided not to bother checking anything other than the first OCLC number, but I may circle back and look at the others later. I wrote this function to split up the values, keep the first value in a column called `oclc`, store the second value in `oclc2`, and dump anything after that. Then, the for loop applies the function to each segment dataframe in my environment.

```r
# separate oclc values, if more than one, and re-type as numeric
oclc_split <- function(data, column_name) {
  split_values <- strsplit(as.character(data[[column_name]]), ",")
  data$oclc <- sapply(split_values, function(x) ifelse(length(x) >= 1, x[1], NA))
  data$oclc <- as.numeric(data$oclc)
  data$oclc2 <- sapply(split_values, function(x) ifelse(length(x) >=2, x[2], NA))
  return(data)
}

# split oclc column in each df
for (i in 1:31) {
  df_name <- sprintf("htseg%02d", i)
  df <- get(df_name)
  df <- oclc_split(df, "oclc")
  assign(df_name, df, envir = .GlobalEnv)
}
```

Finally, I arrived at the point where I could join the HathiTrust data to our library P class data. I'm using the `semi_join()` function from the dplyr library to keep any and all rows in my library's data that match an OCLC number (just the first one, remember) in the HathiTrust data. There's another for loop to go through all those HTDL segments one by one and copy the matches to the exact same objects. The last function rolls all the matches together into one dataframe that I can then write to file. There were 3,632 matches out of 5,147, which is pretty good!

```r
# join with alex p class data
oclc_compare <- function(dataset1, dataset2, shared_column) {
  matches <- semi_join(dataset1, dataset2, by = shared_column)
  return(matches)
}

# apply oclc_compare to each df
for (i in 1:31) {
  df_name <- sprintf("htseg%02d", i)
  df <- get(df_name)
  df <- oclc_compare(alex_pclass_oclc, df, "oclc")
  assign(df_name, df, envir = .GlobalEnv)
}

# roll the results into one dataframe
data_list <- list()
for (i in 1:31) {
  df_name <- sprintf("htseg%02d", i)
  df <- get(df_name)
  data_list[[i]] <- df
}
combined_data <- bind_rows(data_list)

write_csv(combined_data, "alex_pclass_in_htdl_fullview.csv")
```

It's likely that we will withdraw most of these books, freeing up some desperately needed space on our second and third floors. Scanning the titles gave me a few moments of doubt (Dinah Craik! Anatole France. Joaquin Miller. Really?), but in pretty much every case these were works that are widely held, often elsewhere in our selfsame libraries, either in the same or in later editions. Book historians and scholarly editors know that different editions of a work are not "copies" and not interchangeable, but the reality for us is that we probably no longer need ten versions of Theodore Dreiser's _An American Tragedy_, given the pressure on our spaces. There was also plenty of, dare I say it, B and C list literature, for which HathiTrust digital access is certainly good enough. I paused when I saw [_Some queer Americans and other stories_](https://catalog.hathitrust.org/Record/012453512)... Surely not "queer" like that, right? Indeed, it is a peculiar tome exoticizing and patronizing the residents of rural Appalachia. There were also plenty of vaguely prurient sounding titles like [_The College Widow_](https://catalog.hathitrust.org/Record/102426649) and [_The story of a bad boy_](https://catalog.hathitrust.org/Record/001030910), which were funny to look at, but again, nothing to agonize over in terms of keeping the print.

Incidentally, I am rusty at writing functions in R, and after fumbling through some Google search results, I caved and opened my institution's Copilot instance. While the results weren't always just right on the first or second try, I have to admit that Copilot saved me a ton of time sifting through stackoverflow replies.

Find the whole script as a GitHub Gist at <https://gist.github.com/giannetti/752ff7760f633f7cbfd194a5a1212948>.

[^1]: See ["Using AWK to Filter Rows"](https://www.tim-dennis.com/data/tech/2016/08/09/using-awk-filter-rows.html) by Tim Dennis for a very handy tutorial.

[^2]: For more split examples, see <https://servicenow.iu.edu/kb?id=kb_article_view&sysparm_article=KB0026049>.
