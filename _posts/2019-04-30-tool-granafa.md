---
layout: post
title:  "Grafana"
date:   2019-04-30 01:30:13 +0000
categories: Tools
tags: Visualisation
---

Grafana is a visualisation tools available on the DCD hub to
visualise data. To access it, go to the [dwd.tudelft.nl/grafana](https://dwd.tudelft.nl/grafana),
select OAuth and sign in with your DCD Hub credentials.

![Flowchart Push Button](/docs/assets/res/grafana_signin.png)

Click on the plus on the left panel.

![Flowchart Push Button](/docs/assets/res/grafana_folder.png)

Create a new folder for your project.

![Flowchart Push Button](/docs/assets/res/grafana_new_folder.png)

Then, click on the green button 'New Dashboard' to create a new Dashboard, and
select a new panel 'Graph'.

![Flowchart Push Button](/docs/assets/res/grafana_graph.png)

At the top of this new panel, click on 'Panel Title > Edit'

![Flowchart Push Button](/docs/assets/res/grafana_edit.png)

At the bottom, in the query element GROUP BY, click on time and 'remove'.

![Flowchart Push Button](/docs/assets/res/grafana_remove_time.png)

In FROM, click on 'Select Measurement' and select your Property ID. If your
Property ID is not appearing in the list, the hub is not receiving data from
your python code.

In SELECT, click on field and select Value1. Then click on the
plus > Fields > Field to add Value2, Value3...

![Flowchart Push Button](/docs/assets/res/grafana_select_measurement.png)
