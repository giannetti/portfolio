---
id: 393
title: 'Lab 2: Making Digital Maps'
date: 2015-02-17T23:24:18-04:00
author: Francesca
layout: single
guid: http://francescagiannetti.com/?p=393
permalink: /lab-2-making-digital-maps/
excerpt: "A literary mapping exercise using Henry James's _The Portrait of a Lady_"
header:
  overlay_image: /assets/images/lapatriageogra323stra_0020.jpg
  overlay_filter: 0
  caption: "Source: [**Internet Archive**](https://archive.org/details/lapatriageogra323stra/page/n19)"
categories:
  - pedagogy
---
This laboratory is intended for the students enrolled in Dr. Andrea Baldi’s Italian 368 course, Walking in the Metropolis. We will work with a data set of the locations mentioned twice or more in Henry James&#8217;s _The Portrait of a Lady_.<sup><a href="#footnote">1</a></sup> The plan is to geocode these locations, which is to say we&#8217;ll assign longitude and latitude coordinates for each. With the added geospatial data, visualizing those locations in <a href="https://www.google.com/maps/d/?gmp=mpp" target="_blank" rel="noopener noreferrer">Google My Maps</a> becomes relatively easy. Finally, we&#8217;ll embed our custom digital maps into a <a href="https://sites.google.com/" target="_blank" rel="noopener noreferrer">Google Site</a>, where we can integrate text, images, maps, and most other forms of digital content, into a final digital project that you could submit for a grade.

## Needed for this lab

  1. Your favorite browser
  2. A Google account (Gmail or Scarlet Mail is fine)
  3. The [portrait-of-a-lady-locations.csv](https://rutgers.box.com/s/wpmdu99rcbbr2k4cmtoc79mcu2qv481r) data set

## Getting started

Download the [portrait-of-a-lady-locations.csv](https://rutgers.box.com/s/wpmdu99rcbbr2k4cmtoc79mcu2qv481r) file to your desktop by right clicking, and selecting &#8220;Save link as&#8230;&#8221; Open it up in Excel and have a look at the data headers. This data was prepared with some computational assistance, which hugely simplified the process but did result in a few errors and literals that look odd out of context. As an example, Henrietta Stackpole mentions that she is very eager to visit the new Republic, by which she means France&#8217;s Third Republic. It is perhaps obvious that &#8220;Republic&#8221; (line 42) does not correspond to any distinctive current day geographical or political reality. The geocoding service we&#8217;ll use won&#8217;t recognize it, so in the adjacent columns, this information was modernized and clarified.  Under **original-location**, I left what was authentic to the text because we can reuse it in our marker labels. Lastly, because this is a work of fiction, there are some locations, specifically names of private homes, that are not easily mappable. For instance, we know that Gardencourt is Mr. and Mrs. Touchett&#8217;s home in England, but unless I am mistaken, we can&#8217;t be much more specific than that. Again, the geocoding service won&#8217;t recognize it, but it&#8217;s okay. Gardencourt is important to the plot, but we can make do without it, at least for the purposes of this exercise. This is one example of the complexities of working with literary data.

## Next steps

* Log into either your Scarlet Mail or Gmail account and navigate over to <a href="https://drive.google.com" target="_blank" rel="noopener noreferrer">Google Drive</a>.
* We need to create a Google Spreadsheet in order to reuse a bit of code (specifically, a Google Apps Script) that will geocode our addresses for us.<sup><a href="#footnote">2</a></sup> Click New, and select Google Sheets. In the untitled spreadsheet, go to File > Import > Upload and select the .csv file that you copied to your desktop. Click Import, then Open now.

![Gmaps import file pop up window](/assets/images/Screen-Shot-2015-02-17-at-6.42.59-PM.png)

* Next, we want to add that geocoding script to our Google spreadsheet. Copy this <a href="https://raw.githubusercontent.com/mapbox/geo-googledocs/master/MapBox.js" target="_blank" rel="noopener noreferrer">source code</a> by clicking Ctrl+A (Command+A on a Mac) to select all, and Ctrl+C (Command+C) to copy. In your Google spreadsheet, go to Tools > Script editor [Note added 2015/11/07: this script no longer appears to work. Use instead <a href="https://github.com/leighghunt/geo-googledocs/blob/patch-1/MapBox.js" target="_blank" rel="noopener noreferrer">this fork</a> shared by <a href="https://github.com/leighghunt" target="_blank" rel="noopener noreferrer">Leigh Hunt</a>, which adds the Google Maps API]. Close the pop-up window that appears. You should see a Code.gs window. Delete that default text
    {% highlight javascript %}function myFunction() { } {% endhighlight %}

* &#8230; and paste in the copied code. Go to File > Save. It will prompt you for a script name. You can use james-geo or whatever pleases you.
* Go back to your Google spreadsheet and reload it in the browser. A menu called Geo should appear to the right of Help.

![Geo menu item in G sheets](/assets/images/Screen-Shot-2015-02-17-at-7.43.33-PM.png)

* Now we need to tell the script where our physical addresses are. Highlight all the columns and rows with the modernized location information. This should be columns C through F (current-day-location through country) and rows 2-60. Don&#8217;t worry about empty cells. With this area highlighted, go to Geo > Geocode addresses. The script will require authorization to run, and it will need to be granted access to your Google Drive. **Note**: make sure that pop-ups are not blocked in your browser. Next, the script will want you to select a geocoding service. I tend to use the default Mapquest API because it doesn&#8217;t require an API key. Click Geocode.
* Once the geocoding is done, you&#8217;ll notice that a few rows were skipped. It&#8217;s really quite understandable that the Mapquest API had no idea what we meant by the rows in question, but by now you should have a workable list of locations with lat-lon pairs to import into <a href="https://www.google.com/maps/d/splash?gmp=mpp&app=mp" target="_blank" rel="noopener noreferrer">Google My Maps</a>.

## Making a digital map

  * Over in <a href="https://www.google.com/maps/d/splash?gmp=mpp&app=mp" target="_blank" rel="noopener noreferrer">Google My Maps</a>, select Create a new map. Click on Import under Untitled layer in the left sidebar.
  
  ![Importing a layer in MyMaps](/assets/images/Screen-Shot-2015-02-17-at-10.05.40-PM.png)

  * Instead of uploading a data set, navigate over to the Google Drive view and select the Google spreadsheet with the newly geocoded data. Again make sure that you have pop-ups enabled in your browser. In the first menu, Google My Maps will ask you to tell it where your latitude and longitude coordinates are. Pair the geo-latitude column header with latitude; geo-longitude with longitude. Click continue.
  
  ![Indicate columns with geocoordinates](/assets/images/Screen-Shot-2015-02-17-at-10.10.23-PM.png)

  * Next, Google My Maps will prompt you for a marker title. Let&#8217;s select **original-location**, and click Finish.
  * Troubleshooting: the Mapquest API should work pretty well on this data set, but it does occasionally produce errors that become obvious in the map visualization. If this happens to you with your own data, a quick way to manually fix the latitude and longitude coordinates is to use regular <a href="https://www.google.com/maps" target="_blank" rel="noopener noreferrer">Google Maps</a>. When you search for a physical address in Google Maps, the lat-lon pair appears in the URL of the resulting map after the @ sign:
  ![Coordinates in G Maps URL](/assets/images/Screen-Shot-2015-02-17-at-7.16.23-PM.png)
  When you retrieve the (likely) correct lat-lon pair from Google Maps, you can return to your Google spreadsheet and make the corrections manually.
  * Google My Maps gives you nine base maps to experiment with. Click on the downward arrow to the left of Base maps and choose one that suits you.
  * When Google My Maps loads your data layer, the default view is Uniform style. Let&#8217;s instead style by data column. Click on the paint roller icon, and experiment with the options under Group places by > Style by data column, and Set labels. Here&#8217;s an example of a clickable, zoomable map visualization of the data. The darker the pin, the higher the frequency count of the location. I am sure you can come up with many others.

  * This step is important for sharing your custom map with others on the web. Click on Share in the left sidebar. Give a name and description to your map (this can be changed later). Change the access from Private (default) to Public on the web. Then click Done. Finally, click on the vertical three dots to the right of Share. Select Embed on my site,  copy (Ctrl+C) the embed code and click Ok.

## Creating your digital project

  * Let&#8217;s navigate over to <a href="https://sites.google.com/" target="_blank" rel="noopener noreferrer">Google Sites</a>. Click on Create. Leave Blank template selected, and give a unique name to your site. This can be changed later, so for now you might choose your first name + Mapping Demo.
  * Click on Select a theme, and choose one that you like. Then, click Create again to launch the new site.
  * Click on the Edit icon to edit your site (it looks like a pencil), and change the title of the home page to something more descriptive, like The Portrait of a Lady Project.
  * While still in Edit mode, place the cursor in an area of the site where you want to embed your map. Embedding your custom map will require opening the HTML editor in Google Sites. It appears as a tiny menu option with the letters HTML in between angle brackets (<>) on the right side of the horizontal navigation bar.
  ![HTML editor button in G sites](/assets/images/Screen-Shot-2015-02-17-at-10.58.56-PM.png) This is where you want to paste the embed code that you copied from Google My Maps. Paste it in, click update in the HTML editor, and then click Save to save your work on the site.
  * Experiment with adding some text to your site, either above or below your map. For now, some <a href="http://generator.lorem-ipsum.info/" target="_blank" rel="noopener noreferrer">lorem ipsum</a> filler text will do.
  * Click on the New page icon, and create another web page called Bibliography.
  ![Create page button in G sites](/assets/images/Screen-Shot-2015-02-17-at-11.20.46-PM.png)

  * One last step, which is a repetition of what we did in Google My Maps: you need to make your site publicly viewable, or at the very least viewable by the Rutgers Community. Click on Share in the upper right hand corner. Change access from Private to Public on the web. Then go back to your site and share the URL with as many people as you like!

## Coda

There is one data point that Google My Maps doesn&#8217;t allow us to visualize very well, and that is the frequency count. This is the number of times that the location appears in the text of _The Portrait of a Lady_, which is a loose measure of its significance to the plot. Although we won&#8217;t have time to go into this today, I want to show you one other tool that does allow us to scale our points by frequency-count: <a href="http://palladio.designhumanities.org/#/" target="_blank" rel="noopener noreferrer">Palladio</a>. Palladio is a browser-based visualization tool that was developed by the Stanford Humanities + Design Lab.<sup><a href="#footnote">3</a></sup>

![Screen capture of Palladio with points sized by mention frequency](/assets/images/Screen-Shot-2015-02-17-at-2.50.10-PM.png)

In this visualization, you can see that England and Rome appear most often in the text, with approximately the same frequency, which makes good sense when you know the novel. James&#8217;s text also makes frequent reference to other locations throughout Continental Europe, the U.K., and the Northeastern Seaboard of the United States.

There is one piece of information I want to mention if you decide to give Palladio a go. You will need to rearrange the geographic coordinate data into one column as **latitude, longitude**. So for example, the coordinates for Rome would appear in a cell as follows:

<code>41.9100711, 12.5359979</code>

This can be easily accomplished with the <a href="https://support.google.com/docs/answer/3094077" target="_blank" rel="noopener noreferrer">JOIN function</a> in Google spreadsheets or CONCATINATE in Excel.

* * *

<sup><a id="footnote"></a>1.</sup> To prepare this data set, I used the plain text version of Project Gutenberg&#8217;s ebook of _The Portrait of a Lady_, vols. <a href="http://www.gutenberg.org/ebooks/2833" target="_blank" rel="noopener noreferrer">1</a> and <a href="http://www.gutenberg.org/ebooks/2834" target="_blank" rel="noopener noreferrer">2</a>. I then used the <a href="http://nlp.stanford.edu/software/CRF-NER.shtml" target="_blank" rel="noopener noreferrer">Stanford Named Entity Recognizer</a> to tag all the locations in the text. Finally, I moved all the locations into a separate text document, counted them and sorted them in reverse order by frequency. Lest you think I&#8217;m some kind of computational genius, I followed the instructions pretty closely in William J. Turkel&#8217;s excellent blog post, <a href="http://williamjturkel.net/2013/06/30/named-entity-recognition-with-command-line-tools-in-linux/" target="_blank" rel="noopener noreferrer">Named Entity Recognition with Command Line Tools in Linux</a>.

<sup><a id="footnote"></a>2.</sup> Many thanks to <a href="https://github.com/yhahn" target="_blank" rel="noopener noreferrer">Young Hahn</a> for sharing his Google Apps script.

<sup><a id="footnote"></a>3.</sup> For more information on how to make the most of Palladio, watch this <a href="http://hdlab.stanford.edu/lab-notebook/palladio/2014/06/20/full-video-tutorial/" target="_blank" rel="noopener noreferrer">video tutorial</a>.