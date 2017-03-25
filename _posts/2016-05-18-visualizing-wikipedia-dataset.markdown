---
layout: post
title: 'Visualizing wikipedia links ( Part 1 )'
date:   2016-05-17 17:00:01 +0800
categories: wikipedia d3 data science mining scrap
dataset_link: assets/data_science_links.csv
enable_viz: true
loading_data_gif: http://images1.desimartini.com/static1//images/loading.gif
enable_d3: true
js_links:
 - https://cdnjs.cloudflare.com/ajax/libs/nvd3/1.8.3/nv.d3.min.js
css_links:
 - https://cdnjs.cloudflare.com/ajax/libs/nvd3/1.8.3/nv.d3.min.css
---


In the previous post, we've successfully gathered data regarding the wiki pages relationship around [data science][data-science-wiki]. What exactly are we going to do with this data?

Image this, you now have the relationship between wiki pages. What are the questions that we can answer using this [dataset][dataset-link]. I'll provide some of my ideas below

1. From the data science article, which other article has the highest connection to it?
2. What are the top articles that surrounds data science?
3. Is there an article bigger than data science?
4. Is data science the center of this link?

To answer the following question, we need to see the [dataset][dataset-link]. We can view it as a table. 



_Note: Only the first 10 result was shown._

<div id='table'></div>

We can use this but this make our understanding slower. Why? because 

> tables are good when viewing specific data, that is when we know exactly where to look.

In this case we have no previous knowledge on what exactly the dataset looks like. How about a chart? I think a bar chart would make sense, lets check it out.

To create a bar chart we need to identify what goes to our x and y axis. We need a categorical variable for the x axis while we can use continuous or discrete numeric variable for the y. With that being said we can set our x axis as the wiki article and the y axis as the frequency on how many times it was linked.

<div id='bar-chart'></div>

We see in this chart is that __Statistics__ is the most linked article followed by __Data Mining__, higher than our seed article __Data Science__. So what exactly am I gonna do with that data?

Data alone may not directly help us in what we want to achieve, but with additional creativity data can be powerful. Imagine you are someone who wants to learn data science but didn't know where to start. By looking at these chart, it enables you to see that _statistics_ and _data mining_ are closely related to data science.

But I can do that with google. Of course an easy google search will help you with that. The point is, google can help us with public topics but what about sensitive information, like business or corporate data. You can run these without the risk exposing it to the public.

In the next topic will choose a better visualization, called __force layout__.

[data-science-wiki]: https://en.wikipedia.org/wiki/Data_science
[crisp-image]: https://upload.wikimedia.org/wikipedia/commons/b/b9/CRISP-DM_Process_Diagram.png
[dataset-link]: {{ page.dataset_link }}

{% if page.enable_viz %}
<script> 
	var DATASET_LIMIT = 20;
	$("#table").append("");
	d3.text("{{ page.dataset_link | relative_url }}", function(data) {
		$("#table").empty();
		// Visualization - table
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

		// Visualization - bar chart
		// init vars
		var links = [];
		// get data
		var data = d3.csv.parse(csvText);
		for (var i = data.length - 1; i >= 0; i--) {
			links.push(data[i].from);
			links.push(data[i].to);
		}
		// process data
		var link_count = d3.nest()
		   .key(function(d) { return d })
		   .rollup(function(d) { return d.length })
		   .entries(links);
		link_count.sort(function(a, b) {
			return b.values - a.values;
		});
		// start visualization
		nv.addGraph(function() {
			// specify chart type
			var chart = nv.models.discreteBarChart()
				.x(function(d) { return d.key; })
				.y(function(d) { return d.values; });
			chart.xAxis.rotateLabels(-45);
			// specify layout
			var svg = d3.select('#bar-chart').append('svg');
			svg.attr('height', '30vw');

			svg.datum([{
					'key': 'link_count',
					'values': link_count.slice(0, DATASET_LIMIT)
				}])
				.call(chart);
			// housekeeping
			nv.utils.windowResize(chart.update);
			return chart;
		});

	});
</script>
{% endif %}
