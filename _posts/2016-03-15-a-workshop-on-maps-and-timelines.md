---
id: 719
title: A Workshop on Maps and Timelines
date: 2016-03-15 03:56:54 -04:00
author: Francesca
layout: single
guid: http://francescagiannetti.com/?p=719
permalink: /a-workshop-on-maps-and-timelines/
excerpt: "Text of a workshop I led at the Music Library Association Annual Meeting 2016"
header: 
  overlay_image: /assets/images/austausch.png
  overlay_filter: 0.4
toc: true
toc_label: "Contents"
categories:
  - pedagogy
  - professional development
---
<span class="dropcap">T</span>his post is a partial replication of a workshop I recently led at the Music Library Association Annual Meeting in Cincinnati, Ohio. The data and slides can be downloaded at <http://bit.ly/musiclib2016>. The slides had originally been created as a reveal.js presentation, which is available [here](http://francescagiannetti.com/musiclib2016/#/).

Although I haven&#8217;t yet had a chance to apply a spatial approach to my own research, I get asked to teach digital maps more than any other digital humanities technique. I&#8217;m not quite sure why, but it may have to do with librarians and academic faculty wanting to explore a geographic aspect of their sources in a more purposeful and deliberate fashion. As many researchers have remarked, reading and noting location information is one thing; plotting it on a map may lead to perceptions and insights that were hitherto only intuited, or entirely hidden. Although not quite as straightforward as word clouds, maps may offer a kind of gateway experience to more complex forms of digital or computational analysis. Plus, maps have a lot of appeal to humanities professors wanting to integrate a visual component to their course assignments.

I&#8217;ve certainly had fun exploring the PostGIS and PostgreSQL functionality of CartoDB as a way of transforming and filtering datasets. I&#8217;ve gathered my knowledge from the CartoDB tutorials (see under Other Resources) and <a href="https://github.com/clhenrick/cartodb-tutorial" target="_blank">various</a> <a href="https://github.com/statsmaths/cartodbTutorial" target="_blank">other</a> <a href="http://www.azavea.com/about-us/staff-profiles/daniel-mcglone/" target="_blank">professionals</a>. If you have any questions or remarks on this material, please do leave me a comment below.

One of the questions I received during the workshop had to do with my software choice. When choosing a mapping tool, the decision really must rest on what one hopes to accomplish. I chose to use CartoDB for most of the workshop because it offers a very generous free plan; one only needs to provide an e-mail address to get started. The visualization wizard provides a nice assortment of options for customization, including pop-up windows in which one can include text and images. The Torque visualization is fantastic for animations (as is Heatmap) if one&#8217;s dataset includes a date column. And it is super easy to share and embed CartoDB maps. The one drawback I can think of is that CartoDB does not currently support raster data, which is to say that one could not import a scan of a historic map or schematic plan. For such uses, one might look instead to MapWarper, QGIS, or even Palladio. Incidentally, I mention Palladio under Timelines below, but it is another great, free mapping tool. One thing that is easier to accomplish in Palladio than in CartoDB is the point-to-point visualization, if one&#8217;s data include a start and end location (e.g. place of birth and place of death).

## Motivations for Using Spatial and Temporal Approaches in the Humanities

### Identifying Patterns

<a href="https://web.stanford.edu/group/spatialhistory/cgi-bin/site/viz.php?id=133&project_id=0"><img class="align-left" src="/assets/images/fever-150x150.png" alt="fever" /></a> Modern geospatial approaches owe a lot to the field of epidemiology, which has used maps as a way to detect patterns in the way that disease spreads. This example shows the [Yellow Fever Epidemic of 1850](https://web.stanford.edu/group/spatialhistory/cgi-bin/site/viz.php?id=133&project_id=0). Historians at Stanford created it as a way to explore the lived experience of disease and death. They note that &#8220;different temporal aggregations of data can shift interpretations of the spatial pattern of epidemics.&#8221;

&nbsp;

### Charting a Travel Narrative

<a href="http://enec3120.neatline-uva.org/neatline/show/a-sentimental-journey"><img class="align-right" src="/assets/images/sterne-150x150.png" alt="sterne" /></a> UVA student Kurt Jensen visualizes the travels of Yorick in Laurence Sterne&#8217;s _A Sentimental Journey Through France and Italy_. In doing so, he explores the tensions between narrative and chronological time.

&nbsp;

### Exploring Relationships

<a href="http://republicofletters.stanford.edu/casestudies/franklin.html"><img class="align-left" src="/assets/images/franklin-150x150.png" alt="franklin" /></a> The Franklin case study of [Mapping the Republic of Letters](http://republicofletters.stanford.edu/index.html) compares Benjamin Franklin&#8217;s correspondence network to that of Voltaire. Using this view of the data, one could make the argument that Franklin was the more cosmopolitan of the two. This finding would likely be rigorously disputed by an Enlightenment scholar.

&nbsp;

### Modeling Changes Through Time

Ben Schmidt makes [several interesting observations](http://sappingattention.blogspot.com/2012/10/data-narratives-and-structural.html) about his work mapping the paths taken by American ships in 1800-1860. This visualization collapses the data into a single year to show the seasonal migration of ships in the Pacific Ocean.

{% include video id="WVnuWXk8w4g" provider="youtube" %}

### Exploring Content

<a href="http://photogrammar.yale.edu/"><img class="align-left" src="/assets/images/photogrammar-150x150.png" alt="photogrammar" /></a>Maps and timelines can lead to new insights about your collections, but they are also a great way of allowing users to explore content. The [Yale Photogrammar](http://photogrammar.yale.edu/) provides an interactive map interface and scrubber bar for exploring thousands of photographs by geography, time period, and creator. See the Labs section for more facets.

&nbsp;

## Data Formatting

### Part I

| Description               | City     | Country |
| ------------------------- | -------- | ------- |
| Mozart family starts tour | Salzburg | Austria |
| They arrive in Munich     | Munich   | Germany |
| Then they go to Mannheim  | Mannheim | Germany |

Use a header row to describe your data; avoid special characters, spaces and numbers here. Place individual locations on rows underneath the header; 1 row = 1 location. Store each address element in its own cell.

### Part II

| Street        | City       | State | Zip   | Time                 |
| ------------- | ---------- | ----- | ----- | -------------------- |
| 35 W Fifth St | Cincinnati | OH    | 45202 | 2016-03-05T13:40:00z |

Store address elements and dates as <u>text fields</u> so that your spreadsheet application does not autoformat them and introduce errors. Use a machine readable format for dates and times, i.e. [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm).

### Part III

| old_city   | old_country | new_city    | new_country |
| ---------- | ----------- | ----------- | ----------- |
| Königsberg | Prussia     | Kaliningrad | Russia      |

Geocoding services need contemporary geopolitical information to work well. If you&#8217;re working with historical data, add some columns to record where the location is currently.

### Part IV

| Description                          | City     | District  | Country | Certainty |
| ------------------------------------ | -------- | --------- | ------- | --------- |
| Beethoven was somewhere around here. | Karlsbad | Karlsruhe | Germany | medium    |

If you&#8217;re dealing with uncertainty in your data, record it as a separate element.

### Part V

Lastly, it&#8217;s recommended to assign a unique identifier to each location, particularly if it is important to know the order in which you entered your data. Many mapping applications automatically assign a unique ID to records, but sorting will destroy any original order.

## Exercises

Create a free CartoDB account at [cartodb.com/signup](https://cartodb.com/signup) and log in. Take a few minutes to explore the interface. See: Dashboard, Datasets, Maps.

### About our Data

We will explore a dataset prepared by Michelle Oswell of the Curtis Institute on Breitkopf & Härtel&#8217;s Concert Program Exchange, or Konzertprogramm Austausch. B&H began the series in 1893 [&#8220;as a means to promote current awareness of concert repertories via the circulation of printed concert programmes.&#8221;](http://inconcert.datatodata.com/datasets/concert-programme-exchange-1901-1914/) Subscribing organizations sent their programs to B&H for distribution in the series. See Michelle&#8217;s [ATMLA presentation](https://prezi.com/ox7uwlp6tila/breitkopf-hartels-concert-programm-austausch/?utm_campaign=share&utm_medium=copy) for more background on her project.

### Adding Data

In the materials downloaded to your desktop, there is a data folder with three datasets. We will add <b style="color: #827807;">austausch.csv</b> to CartoDB by going to the Datasets window of the Dashboard.

_Note_: by your user name in the upper left corner, you should see either <b style="color: #0d6b82;">Maps</b> or <b style="color: #0d6b82;">Datasets</b> with a down arrow next to it. If it says Maps, click the down arrow and navigate to Your datasets.

Click on New Dataset, where you should see a Data file option (default). Click BROWSE and choose <b style="color: #827807;">austausch.csv</b>. Click Connect Dataset.

CartoDB should do a good job of “guessing” your data type (point). In the Data View of your dataset, hopefully you’ll see a new column called <b style="color: #0d6b82;">the_geom</b> with coordinate data. CartoDB will automatically geocode physical locations for you, which is to say it will assign latitude and longitude coordinates, so long as you supply clean address data. Geocoding of administrative regions (city, state, country) is free; geocoding street addresses is capped at 100 per month in the free plan.

Let&#8217;s add a second dataset. Return to the Datasets window of your dashboard. Click the little crooked arrow in the upper left side of your screen (next to the title of the dataset) to get back there. Now click New Dataset. Press BROWSE and find the dataset called <b style="color: #827807;">vg2500_geo84.zip</b> in the data folder.<sup>1</sup> Click Connect Dataset. This zipped file actually contains five files. We are only interested in the one called <b style="color: #827807;">vg2500_bld</b>. This stands for Bundesländer, or state. You can delete the others if you want to keep your dataset area clean.

## Data Wrangling

Returning now to the Austausch data you uploaded, let&#8217;s assume that CartoDB doesn&#8217;t assign latitude and longitude coordinates to your physical addresses, or it skips a few rows (this will probably happen). If <b style="color: #0d6b82;">the_geom</b> is empty, or if a few cells in this column have null values, go to Edit > Georeference. Navigate to the City Names option; enter <b style="color: #0d6b82;">citymodern</b> as the city, and <b style="color: #0d6b82;">country</b> as the country (skip Admin Region), and click CONTINUE. Then click &#8220;Georeference your data with points.&#8221;

Here&#8217;s something odd. Munich appears a lot in our dataset, but in the map view you&#8217;ll probably see that there&#8217;s no dot over Munich. This is a peculiar GeoNames geocoding error. Here&#8217;s a global fix. Go to the SQL window, paste in this command, and click **Apply query**.

{% highlight sql %}
UPDATE austausch
SET the_geom = CDB_LatLng(48.1333,11.5667) 
WHERE citymodern ILIKE '%Munich%'
{% endhighlight %}

Let&#8217;s turn now to the Bundesländer polygons, called <b style="color: #827807;">vg2500_bld</b>. Return to the datasets window and click on it to open the data view. We&#8217;re going to add a column to this dataset in which we will include values for the total number of subscribers by German state. Click on the little icon on the bottom right of your screen (it will say &#8216;add column&#8217; when you hover over it with your cursor) and add a new column. Rename it <b style="color: #0d6b82;">subs_per_state</b>. Change the data type from string to number.

### PostGIS Functions

Now we are going to count the number of subscribers by German state using the ST_Intersects function. This command is updating <b style="color: #827807;">vg2500_bld</b>, setting the <b style="color: #0d6b82;">subs_per_state</b> column to equal a total (*) count of all the subscribers that intersect with each state. ST_Intersects is the PostGIS function for spatial joining. More information on other PostGIS functions can be found [here](http://postgis.net/docs/reference.html).

{% highlight sql %}
UPDATE vg2500_bld
    SET subs_per_state = (
    SELECT COUNT(*)
    FROM austausch
    WHERE ST_Intersects(the_geom, vg2500_bld.the_geom)
    )						
{% endhighlight %}

Next, let&#8217;s draw some lines in between our Breitkopf & Härtel subscribers to get an idea of the subscription network. It may be useful to collect our points by issue first. This would help if we wanted to visualize the subscribers of individual issues of the Exchange Concert Programs (as opposed to all of them at once). We&#8217;ll use a PostGIS function called ST_Collect. Click on the SQL window, paste in this command, and click **Apply query**

{% highlight sql %}
SELECT ST_Collect (the_geom) AS the_geom, issues
FROM austausch 
GROUP BY issues
{% endhighlight %}

Next, we&#8217;ll draw those lines. Click on the SQL window, paste in this command, and click **Apply query**. CartoDB gives you the option to &#8220;create dataset from query.&#8221; Click on this option so that the lines can be added as an optional data layer to your map visualization. Change the title of this dataset to <b style="color: #827807;">austausch_subscribers</b>

{% highlight sql %}
SELECT ST_MakeLine (the_geom ORDER BY time ASC) AS the_geom, issues 
FROM austausch 
GROUP BY issues
{% endhighlight %}  

If you&#8217;re not sick of PostGIS yet, then here&#8217;s one more thing you can try. Let&#8217;s say we want to draw a radius around the old Breitkopf & Härtel headquarters in Leipzig so it stands out on our map visualization. Go back to your dataset window. Click New Dataset, then Create empty dataset. Call it <b style="color: #827807;">bnh</b>. Double click on the cell under <b style="color: #0d6b82;">the_geom</b> to enter a coordinate pair. Enter longitude 12.3833; latitude 51.3333.

Now click on the SQL window and run this command. This will draw a 50 mile radius around Leipzig that we can then add as a data layer to our map visualization in order to draw attention to this location. We&#8217;re converting from the default meters to miles by multiplying 50 times 1609 (1609 meters ≈ 1 mile).

{% highlight sql %}
SELECT 
    ST_Transform(
      ST_Buffer(
        the_geom::geography,
        50*1609
        )::geometry,
      3857
      ) AS the_geom_webmercator,
    cartodb_id
    FROM 
    bnh
{% endhighlight %}

The ST_Buffer function creates a polygon or shape. Click on &#8220;create dataset from query.&#8221; You&#8217;ll see that the resulting dataset has two columns: <b style="color: #0d6b82;">cartodb_id</b> and <b style="color: #0d6b82;">the_geom</b>. Click on the icon on the lower right of your screen (add column). Rename this column <b style="color: #0d6b82;">name</b>. Double click on the cell underneath it and enter the value: <b style="color: #691d0a;">Breitkopf und Härtel</b>. This allows you to use this text as a label in your map visualization.

## Visualization

Now we&#8217;ll put together some of these data layers we&#8217;ve created into a shareable, interactive map visualization. Go back to your dataset window by clicking on the bent arrow on the upper left of your screen. Go back to the <b style="color: #827807;">austausch</b> dataset and double click on it to open the data view. Scroll over to the <b style="color: #0d6b82;">time</b> column and make sure that CartoDB has assigned a data type of &#8216;date&#8217; to it.

Now navigate to the map view of <b style="color: #827807;">austausch</b>. You&#8217;ll see CartoDB&#8217;s default SIMPLE visualization with the orange dots. Open the tool bar on the right hand of your screen by clicking on the paint brush icon (visualization wizard). Next try the CLUSTER visualization. Experiment with the buckets, the fill color, the transparency value (note: 1 is perfectly opaque, 0.5 is 50% transparent), the marker stroke (white line around the dots), and every and anything else that strikes your fancy.

In other words, press all the buttons. You can&#8217;t hurt anything.

Another CartoDB visualization that is open to us with this dataset is TORQUE. Torque is for time series data. Set the time column to <b style="color: #0d6b82;">time</b> (date works too). It may not make much sense for this dataset, but note that you can make the torque visualization cumulative, meaning that the dots fade very slowly so that a cumulative impact over time may be observed.

Yet one more thing to try &#8211; HEATMAP &#8211; to see all the hotbeds of B&H subscriber activity. Notice that you can animate your heatmap by the time or date column.

Settle on one of these visualization options for <b style="color: #827807;">austausch</b>, and then click on the button VISUALIZE in the upper right corner. Then click: OK, CREATE MAP.

Next, click on the infowindow panel (the icon looks like a cartoon bubble). You can only have infowindows in the SIMPLE, CHOROPLETH and CATEGORY visualizations, so if you didn&#8217;t happen to choose one of those, skip to the next slide. Let&#8217;s turn on some column names in your infowindows. I suggest creating a hover event for <b style="color: #0d6b82;">cityhistorical</b>. Then maybe create a click event for <b style="color: #0d6b82;">concert_title</b> and <b style="color: #0d6b82;">venues</b>. Note: you can always switch to Data View to inspect the column headers and determine which you want to display in the infowindow. To reorder your fields in the infowindows, just click and drag them.

In the toolbar on the righthand side of your screen, click on the plus (+) button at the top to add another data layer. Add the German states layer, <b style="color: #827807;">vg2500_bld</b>, that we tinkered with earlier. Reorder your layers so that points (<b style="color: #827807;">austausch</b>) are uppermost and the polygons or shapes (<b style="color: #827807;">vg2500_bld</b>) are on the bottom. You can do this by clicking and dragging on the layers in that right-hand toolbar.

When you add the German states layer, CartoDB may default to the CHOROPLETH visualization. But if not, open the visualization wizard while in the <b style="color: #827807;">vg2500_bld</b> layer and select CHOROPLETH. Now we can use that <b style="color: #0d6b82;">subs_per_state</b> column we added earlier during the data transformations. Feel free to change the color ramp, quantification, buckets, and anything else. You can also add a label text to the states by selecting the <b style="color: #0d6b82;">gen</b> column. But it might also make your map a bit cluttered. Now we can see where the highest concentration of subscribers are located.

Now might be a good time to mention [ColorBrewer](http://colorbrewer2.org/), which is a great resource for selecting colors for map visualizations.

If you have the layers to add from the earlier data transformation exercises, try adding either the lines connecting the subscriber locations, or the buffer around Leipzig (Breitkopf & Härtel). I&#8217;d recommend stacking the data layers in this order, from top to bottom, for best legibility: <b style="color: #827807;">austausch</b> (points), <b style="color: #827807;">bnh</b> (polygon), <b style="color: #827807;">austausch_subscribers</b> (lines), and <b style="color: #827807;">vg2500_bld</b> (polygons). Note that using all four data layers at once may produce an overly dense visualization, but if you can finesse the colors, transparency and overlays, it just might be possible.

Let&#8217;s take a moment to debrief. What have you noticed about this Breitkopf & Härtel Concert Program Exchange dataset through the process of working with it? Are there any patterns or trends you&#8217;ve observed? Interesting characteristics? Is there anything missing that you thought would be there? Does it raise any additional questions? Technical questions?

### Publishing and Sharing your Map

In the upper left hand corner below the title, select &#8216;Edit Metadata&#8217; to add a Description and Tags to your map to make it more easily discoverable.

On the Options button on the bottom left hand corner, configure which elements will be on your final shared map. I recommend: Title, Description, Search Box, Share Options and Layer Selector.

Finally, in the upper right hand corner, select Publish to get a public URL for your map. You can share your map using that public URL, or by embedding the map on a website or blog using the Embed iframe and pasting it into your website.

## Timelines

I want to show briefly some of the features of a different tool developed by the Stanford Humanities + Design Lab: [Palladio](http://palladio.designhumanities.org/#/). Although it doesn&#8217;t have the PostGIS and PostgreSQL capabilities of CartoDB, Palladio has a map tool that supports all kinds of visualizations (see [Miriam Posner&#8217;s tutorial](http://miriamposner.com/blog/getting-started-with-palladio/) for the details). I particularly like the Timeline and Timespan panels.

Let&#8217;s give Palladio a whirl with the [Repertory Report](http://archives.metoperafamily.org/archives/frame.htm) from the Metropolitan Opera&#8217;s MetOpera Database. What we&#8217;ll see won&#8217;t be substantively different from what [FiveThirtyEight](http://fivethirtyeight.com/features/the-same-four-operas-are-performed-over-and-over/) and [Suby Raman](http://subyraman.tumblr.com/post/101048131983/10-graphs-to-explain-the-metropolitan-opera) have already shown: the Met isn&#8217;t too adventurous with its programming.

Go to [Palladio](http://palladio.designhumanities.org/#/) and click Start. Drag the dataset called <b style="color: #827807;">met-operas-clean-geo.tsv</b> into Palladio&#8217;s text window and click Load. You&#8217;ll probably see a red dot or two next to some of the metadata fields. Palladio wants to verify that any special characters were in fact intended, so click on the red dot to review them. Then click Done. Make sure that Palladio has understood that <b style="color: #0d6b82;">first_performance</b> and <b style="color: #0d6b82;">last_performance</b> are dates.

Click on the Map visualization in Palladio&#8217;s upper navigation menu. Click New Layer. Select Map Type: Points (default) and then for Places, select the <b style="color: #0d6b82;">lat_long</b> column. Set your Tooltip layer to <b style="color: #0d6b82;">work</b>. Check the box next to &#8216;Size points,&#8217; and size them according to &#8216;Number of untitled&#8217; (total number of works in the Metropolitan&#8217;s repertory = 330). Click Add layer. This is a truly dull dataset in terms of geography, since we&#8217;ve only got one venue, but next open the Timespan panel on the bottom of your screen.

Timespan is a great feature for exploring datasets with two dates, e.g. birth and death dates, start and end dates for tours, etc. In the Timespan panel, go to Layout and select Parallel. Make sure that Start date is using the <b style="color: #0d6b82;">first_performance</b> column and End date is using <b style="color: #0d6b82;">last_performance</b>. Set the label to be <b style="color: #0d6b82;">work</b>. We don&#8217;t really need a grouping key with this dataset, but set Group to <b style="color: #0d6b82;">venue</b> (wouldn&#8217;t it be cool to have this repertory data for more than one theatre?).

Parallel lines in the Timespan visualization will mean that the opera had its first and last performance in relatively close chronological succession. Deep diagonal lines indicate that the first and last performances are far apart, which likely means the work is an old chestnut. Hover over the lines with your cursor to see the work titles.

Click on the Timeline button to open a Timeline panel. Set Dates to <b style="color: #0d6b82;">first_performance</b>, Height to &#8216;Number of Untitled&#8217; (number of repertory works), and Group by <b style="color: #0d6b82;">work</b>.

This timeline gives us another view into the same phenomenon: the bulk of new operas were premiered before 1940. It may however be a bit easier in Timeline (as opposed to Timespan) to hover over the bars and see the work titles. You may discover that some of the post-WWII first performances were not exactly new works (e.g. Verdi&#8217;s _I Vespri Siciliani_ and Händel&#8217;s _Rodelinda_). Here&#8217;s the [Repertory Report](http://archives.metoperafamily.org/archives/frame.htm) again for easy comparison.

## Other Resources

### Other Free (or Free-ish) Mapping Tools

  * [Google MyMaps](https://www.google.com/mymaps)
  * [MapWarper](http://mapwarper.net/)
  * [MapBox](https://www.mapbox.com/) and [TileMill](https://www.mapbox.com/tilemill/)
  * [Leaflet](http://leafletjs.com/)
  * [QGIS](http://www.qgis.org/en/site/)

### Other Free Timeline Applications

  * [SIMILE Timeline](http://www.simile-widgets.org/timeline/)
  * [TimelineJS](https://timeline.knightlab.com/)
  * [NeatlineSimile](http://neatline.org/plugins/), if you&#8217;re using [Omeka](http://omeka.org/) as your digital publishing platform

### Self-Guided Learning

CartoDB has the Map Academy and a separate tutorials section, both of which are excellent resources for learning about GIS.

  * [Map Academy](http://academy.cartodb.com/)
  * [CartoDB Tutorials](http://docs.cartodb.com/tutorials/)
  * [CartoDB Tips & Tricks](http://docs.cartodb.com/tips-and-tricks/)

* * *

<sup>1</sup> Dataset by Joerg Moosmeier of Esri Deutschland Gmbh. <a href="http://www.arcgis.com/home/item.html?id=ae25571c60d94ce5b7fcbf74e27c00e0" target="_blank">http://www.arcgis.com/home/item.html?id=ae25571c60d94ce5b7fcbf74e27c00e0</a>