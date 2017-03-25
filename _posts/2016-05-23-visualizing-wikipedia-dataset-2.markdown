---
layout: post
title: 'Visualizing wikipedia links ( Part 2 )'
date:   2016-05-23 17:00:01 +0800
categories: wikipedia d3 data science mining scrap
dataset_link: assets/data_science_links.csv
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

That's some useless image. It seems a __force layout__ isn't a good representation of the data. Majority of the article has link to one another, making the layout a big circle with bunch of links inside it. 

![rubber ball][rubber-ball]

Yep! pretty much like that.

We need another type of visualization to see this kind of data. 

[part-one]: {% post_url 2016-05-18-visualizing-wikipedia-dataset %}
[img-force-layout]: http://orm-chimera-prod.s3.amazonaws.com/1230000000345/images/idvw_1107.png
[rubber-ball]: https://c2.staticflickr.com/4/3203/3099496650_0e99bd4218_b.jpg



<script>
	var DATASET_LIMIT = 20;
	$("#chart").append("<img src='{{ page.loading_data_gif }}'></img>");
	d3.text("{{ page.dataset_link | relative_url }}", function(data) {
		$("#chart").empty();
		// process data
		var dataset = d3.csv.parse(data);		
		var nodes = _.union(_.pluck(dataset, 'from'), _.pluck(dataset, 'to'));
		var links = dataset.map(function(d) {
			if (d.from < d.to) {
				return nodes.indexOf(d.from) + '-' + nodes.indexOf(d.to);
			} else {
				return nodes.indexOf(d.to) + '-' + nodes.indexOf(d.from);
			}
		});
		links = d3.nest()
			.key(function (d) { return d; })
			.rollup(function (d) { return d.length; })
			.entries(links);
		links = links.map(function (d) {
			indexes = d.key.split("-")
			return {
				'source': parseInt(indexes[0]),
				'target': parseInt(indexes[1]),
				'strength': d.values
			}
		});
		nodes = nodes.map(function(d, i) {
			return {
				'id': i,
				'name': d
			}
		});

	// start visualization
	var width = 750,
		height = 500,
		animationStep = 1000;

	// create scales
	var color = d3.scale.category10();

	var force = d3.layout.force()
		.nodes(nodes)
		.links(links)
		.size([width, height])
		.charge([-100])
		.linkDistance([200]);

	var svg = d3.select('#chart').append('svg')
		.attr('width', width)
		.attr('height', height);

	var node = svg.selectAll('circle')
		.data(nodes)
		.enter()
		.append('circle')
		.attr('r', 10)
		.style('fill', function(d, i) { return color(i) }) ;
		// .call(force.drag);

	var link = svg.selectAll('line')
		.data(links)
		.enter()
		.append('line')
		.style('stroke', '#ccc')
		.style('stroke-width', 1) ;
	force.on('start', function() {
		link.attr('x1', function (d) { return Math.random() * width })
			.attr('y1', function (d) { return Math.random() * height })
			.attr('x2', function (d) { return Math.random() * width })
			.attr('y2', function (d) { return Math.random() * height });

		node.attr('cx', function (d) { return Math.random() * width })
			.attr('cy', function (d) { return Math.random() * height });
	});
	force.on('end', function () {
		link.transition().duration(1000)
			.attr('x1', function (d) { return d.source.x })
			.attr('y1', function (d) { return d.source.y })
			.attr('x2', function (d) { return d.target.x })
			.attr('y2', function (d) { return d.target.y });

		node.transition().duration(1000)
			.attr('cx', function (d) { return d.x })
			.attr('cy', function (d) { return d.y });
	});
	force.start();
	
	// end here
	});
</script>
