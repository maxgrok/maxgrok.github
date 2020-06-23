---
template: post
title: How to Setup Kibana for Elasticsearch 5.6
slug: one-hundredth-post
draft: true
date: 2019-06-07T13:00:00.000Z
description: This is a post about how to setup Kibana to visualize your
  Elasticsearch 5.6 data.
category: Kibana/Elasticsearch
tags:
  - Elasticsearch
  - Kibana
  - Visualization
---

## How to Setup Kibana for Elasticsearch 5.6 in 10 Easy Steps! 

(1) Download the correct version of Kibana, not the latest. <br/>
(2) Run your elasticsearch instance in a Terminal tab by going into your Elasticsearch version 5.6 folder and running `./bin/elasticsearch`<br/>
(3) Go to ./config/kibana.yml file and make sure your elasticsearch.hosts is pointing to your server location. In my case, it's `localhost:9200`.<br/>

(4) Run the `./bin/kibana` in Terminal from your Kibana folder. It may take a few times to let it run because it is from a different developer than Apple identified developers. Just right click on the file in Finder and select open with iTerm/Terminal, then click open on the prompt. <br/>
    A series of tasks that Kibana needs to run in order to function will pop up in the Terminal. You should see something like the following: <br/>

![Kibana-output](./kibana_output.jpeg)
<br/>
(5) Navigate to `localhost:5601`. You will see the Kibana default page like the following: 
<br/>
![Kibana-homepage](./kibana-homepage.jpeg)

(6) Go to the folder of your project which elasticsearch is connected to and run this command to find your indices to enter into Kibana: `curl 'localhost:9200/_cat/indices?v'`<br/>

This will show you output including the health, status, index, uuid, pri, rep, docs.count, docs.deleted, store.size, pri.store.size<br/>

Here is an example of the health status output: <br/>

![health-status-output](./health-status-output.jpeg)
<br/>
Here is an example of the rest of the output without the indices: 
<br/>
![rest-of-output](./rest-of-output.jpeg)
<br/>
(7) Copy the indices name that you want to add to Kibana and paste it into the "Index pattern" box within the 'Management' tab under 'Index Patterns' with the header 'Configure an index pattern'. 
<br/>
(8) Choose a 'Time Filter field name' after entering in the index name in the 'Index pattern' box. 
<br/>
(9) Click on 'Create' and you have added your index to Kibana for visualization!
<br/>
(10) Go to the 'Discover' tab in Kibana to see your index data. You can use <a href="https://www.elastic.co/guide/en/elasticsearch/reference/5.6/query-dsl-query-string-query.html#query-string-syntax">lucene query syntax</a> to search through your data. <br />

For more information to setup Kibana for developing visualizations, use the following links:<br/>

<a href="https://www.elastic.co/webinars/getting-started-kibana">Kibana - Getting Started </a><br/>


