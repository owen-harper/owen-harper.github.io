---
title: "Azure Data Factory: On a Budget"
categories:
  - Blog
tags:
  - Azure Data Factory
---

ADF is a powerful cloud ETL tool capable of data transformation at scale. I have learnt (the hard way) that using the tool in certain ways can become very expensive. Here are a couple of cost saving tips I have picked up along the way:

<h2>Use Data Flows Sparingly</h2>
Data Flows are powerful and can shift and transform large volumes of Data. But they do cost much more than Pipeline Activities. If you can, use a Pipeline Activity, they are more than sufficient for basic Copy Data tasks.

![Example Pipeline](/assets/images/ADF-On-A-Budget/adf-pipeline.png)

When a Data Flow is required, do as much as you can in a pipeline and use the Data Flow activity to trigger the Data Flow where needed.

<h2>Be careful with Data Flow Debug</h2>
Turning this mode on keeps Data Flow Compute turned on, allowing you to quickly preview the data at different stages in the data flow. It also eliminates the long start up times of debug runs. But be warned, using it too much can be costly!

<h4>Avoid using it for normal Debug Runs</h4>
When debugging Pipelines, simply clicking Debug will auto turn on Data Flow debug for the entire duration of the pipeline. This will allow Data Flow Tasks to be executed quickly, but also means you will still be paying for the Compute when executing normal Pipeline Activities.

Instead, you can click the dropdown on the Debug button and choose "Use Activity Runtime"

![Use Activity Runtime button](/assets/images/ADF-On-A-Budget/data-flow-debug.png)

This will then execute the pipeline without data flow debug, only spinning up compute when required for Data Flow tasks.

<h4>Minimise Use</h4>
This one is perhaps a little obvious, but be extra careful when you do need to use DF Debug. Make sure you only turn it on when you need it and off again after. It is also wise to set the TTL to the minimum, so if you do forget it will auto turn off at some point.

<h2>Check Data Flow Core Count</h2>
When using the 'Data Flow' activity in Pipelines, make sure you set the Core Count to the minimum (or as appropriate). It sometimes defaults to more cores than required.. More Cores = More Cost!

This can be changed in the Settings of the Activity:

![Change core count](/assets/images/ADF-On-A-Budget/core-count.png)

<h2>Ensure All Pipelines Finish</h2>
When finishing up ADF Dev work for the day, make sure you check for active runs in Monitor. Ensure you don't have any long running or failing pipelines stuck in a loop. These could continue for days, racking up a big cost!

<h2>Set Up a Budget</h2>
Set up a budget with Alerts on your Azure Subscription. This will help you to keep within your expectation of cost and avoid nasty surprises.

![Change core count](/assets/images/ADF-On-A-Budget/cost-alert.png)
