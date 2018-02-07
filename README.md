# Plotting the Course Through Charted Waters

> Heat maps, stacked area plots, mosaic plots, choropleths – oh my! There are so many different ways to visually convey relationships and patterns in data! While there are many tutorials on making them, there are very few for learning to read and understand them. In this workshop on data visualization literacy, you'll learn to recognize many popular types of charts and how to glean insights from them.

To be given at [Wikimedia Foundation's All Hands 2018](https://office.wikimedia.org/wiki/All_Hands/2018)'s [third workshop session](https://office.wikimedia.org/wiki/All_Hands/2018/Workshops) (3:15P-4:00P) in the Cypress room.

## Setup

This workshop uses the [learnr](https://rstudio.github.io/learnr/) and [rmarkdown](http://rmarkdown.rstudio.com/) packages from [RStudio](https://www.rstudio.com/) to create [an interactive web application](http://dataviz-literacy.wmflabs.org/) (which should automatically send you to either [mirror 1](http://dataviz-lit-01.wmflabs.org/), [mirror 2](http://dataviz-lit-02.wmflabs.org/), or [mirror 3](http://dataviz-lit-03.wmflabs.org/)) and [a static web page](https://bearloga.github.io/wmf-allhands18/).

```R
install.packages(c("tidyverse", "devtools", "learnr"))
# Visualizations
devtools::install_github("tidyverse/ggplot2")
install.packages(c("cowplot", "waffle", "RColorBrewer", "DT", "mixtools", "ggridges"))
# Data
install.packages("pageviews")
```

### Load Balancing

The workshop _can_ be deployed to shinyapps.io but we are hosting it using [Wikimedia Cloud Services](https://wikitech.wikimedia.org/wiki/Help:Cloud_Services_Introduction) VMs running Shiny Server (see [this post](https://blog.wikimedia.org/2017/08/21/discovery-dashboards-puppet/) for details).

There is a separate VM that acts as a portal and sends users to the different servers so that we don't have a single overloaded Shiny server. We are using the following web proxies configured in [Horizon](https://wikitech.wikimedia.org/w/index.php?title=Help:Horizon_FAQ):

| Hostname         | Backend instance | Backend port | Backend IP   | Role       |
|:-----------------|:-----------------|-------------:|:-------------|:-----------|
| dataviz-literacy | shinyserv-lb     |         3838 | 10.68.19.61  | Portal     |
| dataviz-lit-01   | shinyserv-01     |         3838 | 10.68.19.31  | App server |
| dataviz-lit-02   | shinyserv-02     |         3838 | 10.68.17.173 | App server |
| dataviz-lit-03   | shinyserv-03     |         3838 | 10.68.19.32  | App server |

We edited `/etc/nginx/sites-available/default` to have the following configuration based on recommendations in [this article](https://support.rstudio.com/hc/en-us/articles/213733868-Running-Shiny-Server-with-a-Proxy):

```nginx
map $http_upgrade $connection_upgrade {
     default upgrade;
     ''      close;
}
upstream datavizlit {
    # some kind of session affinity might be required if shiny-r isn't stateless
    # (either cookie or ip_hash)
    # sticky cookie srv_id expires=1h domain=.dataviz-literacy.wmflabs.org path=/;
    # ip_hash;
    server 10.68.19.31:3838;
    server 10.68.17.173:3838;
    server 10.68.19.32:3838;
}
server {
    listen 3838;
    location / {
        proxy_pass http://datavizlit;
        # websocket require HTTP 1.1
        proxy_http_version 1.1;
        proxy_redirect off;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        # proxy_read_timeout 1d;
        proxy_buffering off;
    }
}
```

We are also using `nginx-full` which includes support for websockets.
