---
layout:     post
title: "Language propensity across the globe"
subtitle:   "Just trying to make bubble charts cool again"
date:       2015-06-07
author:     "Thomas Vincent"
header-img: "img/post-bg-06.jpg"
---

<p align="justify">
I recently came across this <a href="http://www.scmp.com/infographics/article/1810040/infographic-world-languages">great infographic</a> that displayed the list of languages spoken around the world with the use of...bubble charts. For some reason, I've always looked down on bubble charts, probably because someone with authority displayed contempt for them at some point during my education. This infographic completely switched my mind around, and so I set out to reproduce a poor man's version of it. I gathered data from the <a href="https://www.ethnologue.com/statistics/size">following source</a> and filled it out into the appropriate JSON format. The D3 code and CSS styling was then straightforward:
</p>

<h4> CSS styling </h4>
{% highlight css linenos %} 
<style type="text/css">

    text {
      font-size: 11px;
      font-weight: bold;
      pointer-events: none;
    }

    text.parent {
      fill: #1f77b4;
    }

    circle {
      fill: #ccc;
      stroke: #999;
      stroke-width: 2px;
      pointer-events: all;
    }

    circle.parent {
      fill: #1f77b4;
      fill-opacity: .1;
      stroke: steelblue;
    }

    circle.parent:hover {
      stroke: #ff7f0e;
      stroke-width: .5px;
    }

    circle.child {
      pointer-events: none;
    }
</style>
{% endhighlight %}


<h4> Javascript code </h4>
{% highlight javascript linenos %}

<script src="http://d3js.org/d3.v3.min.js"></script>

<script>

var w = 1280,
    h = 900,
    r = 900,
    x = d3.scale.linear().range([0, r]),
    y = d3.scale.linear().range([0, r]),
    node,
    root;

var pack = d3.layout.pack()
    .size([r, r])
    .value(function(d) { return d.size; })

var vis = d3.select("#bubble_chart").insert("svg:svg", "h2")
    .attr("width", w)
    .attr("height", h)
  .append("svg:g")
    .attr("transform", "translate(" + (w - r) / 9 + "," + (h - r) / 9 + ")");

var data = {
     "name": "",
     "children": [
      {
        "name": "Chinese",
        "children": [
          {"name": "Chinese", "size": 1197},
          {"name": "Gan", "size": 20.6},
          {"name": "Hakka", "size": 30.1},
          {"name": "Huizhou", "size": 4.60},
          {"name": "Jinyu", "size": 45.0},
          {"name": "Mandarin", "size": 848},
          {"name": "Min Bei", "size": 10.3},
          {"name": "Min Dong", "size": 9.12},
          {"name": "Min Nan", "size": 46.6},
          {"name": "Min Zhong", "size": 3.10},
          {"name": "Pu-Xian", "size": 2.56},
          {"name": "Wu", "size": 77.2},
          {"name": "Xiang", "size": 36.0},
          {"name": "Yue", "size": 62.2}
        ]
      },
      {
        "name": "Lahndav",
        "children": [
          {"name": "Pakistan", "size": 88.7},
          {"name": "Hindko, Northern", "size": 1.88},
          {"name": "Pahari-Potwari", "size": 2.50},
          {"name": "Punjabi, Western", "size": 62.6},
          {"name": "Saraiki", "size": 20.1}
        ]
      },
      {"name": "Spanish", "size": 339},
      {"name": "English", "size": 335},
      {"name": "Hindi", "size": 260},
      {"name": "Portuguese", "size": 203},
      {"name": "Bengali", "size": 189},
      {"name": "Russian", "size": 166},
      {"name": "Japanese", "size": 128},
      {"name": "Javanese", "size": 84.3},
      {"name": "German, Standard", "size": 78.1},
      {"name": "Korean", "size": 7.2},
      {"name": "French", "size": 75.9},
      {"name": "Telugu", "size": 74.0},
      {"name": "Marathi", "size": 71.8},
      {"name": "Turkish", "size": 70.9},
      {"name": "Tamil", "size": 68.8},
      {"name": "Vietnamese", "size": 67.8},
      {"name": "Urdu", "size": 64.0},
      {"name": "Italian", "size": 63.8},
      {
        "name": "Persian",
        "children": [
          {"name": "Persian", "size": 57.0},
          {"name": "Dari", "size": 9.60},
          {"name": "Persian, Iran", "size": 47.4}
        ]
      },
      {
        "name": "Malay",
        "children": [
          {"name": "Malay", "size": 60.5},
          {"name": "Banjar", "size": 3.50},
          {"name": "Indonesian", "size": 23.2},
          {"name": "Malay [zlm]", "size": 15.8},
          {"name": "Malay, Central", "size": 1.59},
          {"name": "Malay, Jambi", "size": 1.00},
          {"name": "Malay, Kedah", "size": 2.60},
          {"name": "Malay, Pattani", "size":1.00},
          {"name": "Minangkabau", "size":5.53},
          {"name": "Musi", "size": 3.10}
        ]
      },
      {
      "name": "Arabic",
        "children": [
          {"name": "Arabic", "size": 242},
          {"name": "Arabic, Algerian Spoken", "size": 26.7},
          {"name": "Arabic, Chadian Spoken", "size": 1.14},
          {"name": "Arabic, Eastern Egyptian Bedawi Spoken", "size": 1.69},
          {"name": "Arabic, Egyptian Spoken", "size": 55.0},
          {"name": "Arabic, Gulf Spoken", "size": 5.34},
          {"name": "Arabic, Hijazi Spoken", "size": 6.02},
          {"name": "Arabic, Libyan Spoken", "size": 4.32},
          {"name": "Arabic, Mesopotamian Spoken", "size": 15.1},
          {"name": "Arabic, Moroccan Spoken", "size": 21.0},
          {"name": "Arabic, Najdi Spoken", "size": 9.87},
          {"name": "Arabic, North Levantine Spoken", "size": 14.8},
          {"name": "Arabic, North Mesopotamian Spoken", "size": 6.30},
          {"name": "Arabic, Omani Spoken", "size": 1.09},
          {"name": "Arabic, Sa’idi Spoken", "size": 19.0},
          {"name": "Arabic, Sanaani Spoken", "size": 7.60},
          {"name": "Arabic, South Levantine Spoken", "size": 6.47},
          {"name": "Arabic, Sudanese Spoken", "size": 16.9},
          {"name": "Arabic, Ta’izzi-Adeni Spoken", "size": 7.08},
          {"name": "Arabic, Tunisian Spoken", "size": 11.2}
        ]
      }
     ]
    };

  node = root = data;

  var nodes = pack.nodes(root);

  vis.selectAll("circle")
      .data(nodes)
    .enter().append("svg:circle")
      .attr("class", function(d) { return d.children ? "parent" : "child"; })
      .attr("cx", function(d) { return d.x; })
      .attr("cy", function(d) { return d.y; })
      .attr("r", function(d) { return d.r; })
      .on("click", function(d) { return zoom(node == d ? root : d); });

  vis.selectAll("text")
      .data(nodes)
    .enter().append("svg:text")
      .attr("class", function(d) { return d.children ? "parent" : "child"; })
      .attr("x", function(d) { return d.x; })
      .attr("y", function(d) { return d.y; })
      .attr("dy", ".35em")
      .attr("text-anchor", "middle")
      .style("opacity", function(d) { return d.r > 20 ? 1 : 0; })
      .text(function(d) { return d.name; });

  d3.select(window).on("click", function() { zoom(root); });

  function zoom(d, i) {
    var k = r / d.r / 2;
    x.domain([d.x - d.r, d.x + d.r]);
    y.domain([d.y - d.r, d.y + d.r]);

    var t = vis.transition()
        .duration(d3.event.altKey ? 7500 : 750);

    t.selectAll("circle")
        .attr("cx", function(d) { return x(d.x); })
        .attr("cy", function(d) { return y(d.y); })
        .attr("r", function(d) { return k * d.r; });

    t.selectAll("text")
        .attr("x", function(d) { return x(d.x); })
        .attr("y", function(d) { return y(d.y); })
        .style("opacity", function(d) { return k * d.r > 20 ? 1 : 0; });

    node = d;
    d3.event.stopPropagation();
  }

</script>

{% endhighlight %}


<style type="text/css">

    text {
      font-size: 11px;
      font-weight: bold;
      pointer-events: none;
    }

    text.parent {
      fill: #1f77b4;
    }

    circle {
      fill: #ccc;
      stroke: #999;
      stroke-width: 2px;
      pointer-events: all;
    }

    circle.parent {
      fill: #1f77b4;
      fill-opacity: .1;
      stroke: steelblue;
    }

    circle.parent:hover {
      stroke: #ff7f0e;
      stroke-width: .5px;
    }

    circle.child {
      pointer-events: none;
    }

</style>

Finally, make sure to include the following container in which to define you SVG.

{% highlight html linenos%}
<div id="bubble_chart" style="text-align:left"> </div>
{% endhighlight %}

<p>
and there you go!
</p>

<center> <div id="bubble_chart" style="text-align:left"> </div> </center>


<script src="http://d3js.org/d3.v3.min.js"></script>

<script>

var w = 1280,
    h = 900,
    r = 900,
    x = d3.scale.linear().range([0, r]),
    y = d3.scale.linear().range([0, r]),
    node,
    root;

var pack = d3.layout.pack()
    .size([r, r])
    .value(function(d) { return d.size; })

var vis = d3.select("#bubble_chart").insert("svg:svg", "h2")
    .attr("width", w)
    .attr("height", h)
  .append("svg:g")
    .attr("transform", "translate(" + (w - r) / 9 + "," + (h - r) / 9 + ")");

var data = {
     "name": "",
     "children": [
      {
        "name": "Chinese",
        "children": [
          {"name": "Chinese", "size": 1197},
          {"name": "Gan", "size": 20.6},
          {"name": "Hakka", "size": 30.1},
          {"name": "Huizhou", "size": 4.60},
          {"name": "Jinyu", "size": 45.0},
          {"name": "Mandarin", "size": 848},
          {"name": "Min Bei", "size": 10.3},
          {"name": "Min Dong", "size": 9.12},
          {"name": "Min Nan", "size": 46.6},
          {"name": "Min Zhong", "size": 3.10},
          {"name": "Pu-Xian", "size": 2.56},
          {"name": "Wu", "size": 77.2},
          {"name": "Xiang", "size": 36.0},
          {"name": "Yue", "size": 62.2}
        ]
      },
      {
        "name": "Lahndav",
        "children": [
          {"name": "Pakistan", "size": 88.7},
          {"name": "Hindko, Northern", "size": 1.88},
          {"name": "Pahari-Potwari", "size": 2.50},
          {"name": "Punjabi, Western", "size": 62.6},
          {"name": "Saraiki", "size": 20.1}
        ]
      },
      {"name": "Spanish", "size": 339},
      {"name": "English", "size": 335},
      {"name": "Hindi", "size": 260},
      {"name": "Portuguese", "size": 203},
      {"name": "Bengali", "size": 189},
      {"name": "Russian", "size": 166},
      {"name": "Japanese", "size": 128},
      {"name": "Javanese", "size": 84.3},
      {"name": "German, Standard", "size": 78.1},
      {"name": "Korean", "size": 7.2},
      {"name": "French", "size": 75.9},
      {"name": "Telugu", "size": 74.0},
      {"name": "Marathi", "size": 71.8},
      {"name": "Turkish", "size": 70.9},
      {"name": "Tamil", "size": 68.8},
      {"name": "Vietnamese", "size": 67.8},
      {"name": "Urdu", "size": 64.0},
      {"name": "Italian", "size": 63.8},
      {
        "name": "Persian",
        "children": [
          {"name": "Persian", "size": 57.0},
          {"name": "Dari", "size": 9.60},
          {"name": "Persian, Iran", "size": 47.4}
        ]
      },
      {
        "name": "Malay",
        "children": [
          {"name": "Malay", "size": 60.5},
          {"name": "Banjar", "size": 3.50},
          {"name": "Indonesian", "size": 23.2},
          {"name": "Malay [zlm]", "size": 15.8},
          {"name": "Malay, Central", "size": 1.59},
          {"name": "Malay, Jambi", "size": 1.00},
          {"name": "Malay, Kedah", "size": 2.60},
          {"name": "Malay, Pattani", "size":1.00},
          {"name": "Minangkabau", "size":5.53},
          {"name": "Musi", "size": 3.10}
        ]
      },
      {
      "name": "Arabic",
        "children": [
          {"name": "Arabic", "size": 242},
          {"name": "Arabic, Algerian Spoken", "size": 26.7},
          {"name": "Arabic, Chadian Spoken", "size": 1.14},
          {"name": "Arabic, Eastern Egyptian Bedawi Spoken", "size": 1.69},
          {"name": "Arabic, Egyptian Spoken", "size": 55.0},
          {"name": "Arabic, Gulf Spoken", "size": 5.34},
          {"name": "Arabic, Hijazi Spoken", "size": 6.02},
          {"name": "Arabic, Libyan Spoken", "size": 4.32},
          {"name": "Arabic, Mesopotamian Spoken", "size": 15.1},
          {"name": "Arabic, Moroccan Spoken", "size": 21.0},
          {"name": "Arabic, Najdi Spoken", "size": 9.87},
          {"name": "Arabic, North Levantine Spoken", "size": 14.8},
          {"name": "Arabic, North Mesopotamian Spoken", "size": 6.30},
          {"name": "Arabic, Omani Spoken", "size": 1.09},
          {"name": "Arabic, Sa’idi Spoken", "size": 19.0},
          {"name": "Arabic, Sanaani Spoken", "size": 7.60},
          {"name": "Arabic, South Levantine Spoken", "size": 6.47},
          {"name": "Arabic, Sudanese Spoken", "size": 16.9},
          {"name": "Arabic, Ta’izzi-Adeni Spoken", "size": 7.08},
          {"name": "Arabic, Tunisian Spoken", "size": 11.2}
        ]
      }
     ]
    };

  node = root = data;

  var nodes = pack.nodes(root);

  vis.selectAll("circle")
      .data(nodes)
    .enter().append("svg:circle")
      .attr("class", function(d) { return d.children ? "parent" : "child"; })
      .attr("cx", function(d) { return d.x; })
      .attr("cy", function(d) { return d.y; })
      .attr("r", function(d) { return d.r; })
      .on("click", function(d) { return zoom(node == d ? root : d); });

  vis.selectAll("text")
      .data(nodes)
    .enter().append("svg:text")
      .attr("class", function(d) { return d.children ? "parent" : "child"; })
      .attr("x", function(d) { return d.x; })
      .attr("y", function(d) { return d.y; })
      .attr("dy", ".35em")
      .attr("text-anchor", "middle")
      .style("opacity", function(d) { return d.r > 20 ? 1 : 0; })
      .text(function(d) { return d.name; });

  d3.select(window).on("click", function() { zoom(root); });

  function zoom(d, i) {
    var k = r / d.r / 2;
    x.domain([d.x - d.r, d.x + d.r]);
    y.domain([d.y - d.r, d.y + d.r]);

    var t = vis.transition()
        .duration(d3.event.altKey ? 7500 : 750);

    t.selectAll("circle")
        .attr("cx", function(d) { return x(d.x); })
        .attr("cy", function(d) { return y(d.y); })
        .attr("r", function(d) { return k * d.r; });

    t.selectAll("text")
        .attr("x", function(d) { return x(d.x); })
        .attr("y", function(d) { return y(d.y); })
        .style("opacity", function(d) { return k * d.r > 20 ? 1 : 0; });

    node = d;
    d3.event.stopPropagation();
  }

</script>


