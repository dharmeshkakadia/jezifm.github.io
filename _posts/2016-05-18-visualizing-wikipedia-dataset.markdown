---
layout: post
title: 'Visualizing wikipedia links (incomplete)'
date:   2016-05-17 17:00:01 +0800
categories: wikipedia d3 data science mining scrap
dataset_link: https://dl.dropboxusercontent.com/u/98529778/dataset/data_science_links.csv
enable_viz: true
---
{% if page.enable_viz %}
{% include blog-head.html %}
{% endif %}


In the previous post, we've successfully gathered data regarding the wiki pages relationship around [data science][data-science-wiki]. What exactly are we going to do with this data?

Image this, you now have the relationship between wiki pages. What are the questions that we can answer using this [dataset][dataset-link]. I'll provide some of my ideas below

1. From the data science article, which other article has the highest connection to it?
2. What are the top articles that surrounds data science?
3. Is there an article bigger than data science?
4. Is data science the center of this link?

To answer the following question, we need to see the [dataset][dataset-link]. We can view it as a table. 

<div id='table'></div>

_Note: Only the first 10 result was shown._

We can use this but this make our understanding slower. Why? because 

> tables are good when viewing specific data, that is when we know exactly where to look.

In this case we have no previous knowledge on what exactly the dataset looks like. How about a chart? I think a bar chart would make sense, lets check it out.

To create a bar chart we need to identify what goes to our x and y axis. We need a categorical variable for the x axis while we can use continuous or discrete numeric variable for the y. With that being said we can set our x axis as the wiki article and the y axis as the frequency on how many times it was linked. For statistically inclined readers it would be a histogram.

<div id='histogram'></div>



[data-science-wiki]: https://en.wikipedia.org/wiki/Data_science
[crisp-image]: https://upload.wikimedia.org/wikipedia/commons/b/b9/CRISP-DM_Process_Diagram.png
[dataset-link]: {{ page.dataset_link }}

{% if page.enable_viz %}
{% include blog-footer.html %}
<script>    
	// table visualization
	d3.text("{{ page.dataset_link }}", function(data) {
		var parsedCSV = d3.csv.parseRows(data);
		window.csvText = data;
		window.parsedCSV = parsedCSV;

		var container = d3.select('#table')
			.append('table')

			.selectAll('tr')
				.data(parsedCSV.slice(0, 11)).enter()
				.append('tr')

			.selectAll('td')
				.data(function(d) { return d; }).enter()
				.append('td')
				.text(function(d) { return d; });
	});
</script>
{% endif %}
