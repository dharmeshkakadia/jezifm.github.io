---
layout: post
title: 'Visualizing wikipedia links ( Part 2 )'
date:   2016-05-23 17:00:01 +0800
categories: wikipedia d3 data science mining scrap
dataset_link: https://dl.dropboxusercontent.com/u/98529778/dataset/data_science_links.csv
enable_viz: true
loading_data_gif: http://images1.desimartini.com/static1//images/loading.gif
enable_d3: true
js_links:
 - https://cdnjs.cloudflare.com/ajax/libs/underscore.js/1.8.3/underscore-min.js
---

In [part one visualizing wikipedia links][part-one], we used a table an a bar chart to display our data. In these article, we will try to use a force layout. 

What exactly is a force layout? According to D3 Reference

> A flexible force-directed graph layout implementation using position Verlet integration to allow simple constraints. For more on physical simulations, see Thomas Jakobsen. This implementation uses a quadtree to accelerate charge interaction using the Barnes–Hut approximation. In addition to the repulsive charge force, a pseudo-gravity force keeps nodes centered in the visible area and avoids expulsion of disconnected subgraphs, while links are fixed-distance geometric constraints. Additional custom forces and constraints may be applied on the "tick" event, simply by updating the x and y attributes of nodes.

That sounds complicated. How about we look at the wikipedia definition.

> Force-directed graph drawing algorithms are a class of algorithms for drawing graphs in an aesthetically pleasing way. Their purpose is to position the nodes of a graph in two-dimensional or three-dimensional space so that all the edges are of more or less equal length and there are as few crossing edges as possible, by assigning forces among the set of edges and the set of nodes, based on their relative positions, and then using these forces either to simulate the motion of the edges and nodes or to minimize their energy.

Now that looks better, but still I can't get a clear picture of what exactly is a force layout. We spent 5 minutes reading those boring paragraph and still have no idea what are we talking about. We're currently experiencing the limitation of using __words__ to convey information. The more accurate we try to define something using __words__, the harder it is for the reader to digest. But that's another topic.

Going back to defining a force-layout, how about we try looking for a picture. Lets see..

![force-layout][img-force-layout]

Now that looks better. It's way simpler than the definition, but now we have better idea on what a force layout is.

Now imagine those circles as an article in wikipedia and the line that connects them are the link for each article. I think its gonna look great. Can't imagine it? Dont worry I've created a working example below.

<div id="chart"></div>



[part-one]: {% post_url 2016-05-18-visualizing-wikipedia-dataset %}
[img-force-layout]: http://orm-chimera-prod.s3.amazonaws.com/1230000000345/images/idvw_1107.png

<script>
	var DATASET_LIMIT = 20;
	$("#chart").append("<img src='{{ page.loading_data_gif }}'></img>");
	d3.text("{{ page.dataset_link }}", function(data) {
		$("#chart").empty();
		window.data = data;
		// var dataset = d3.csv.parse(data);		
	});
</script>
