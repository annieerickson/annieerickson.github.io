---
layout: page
title: Modeling the COVID-19 pandemic
description: using machine learning to predict disease outcomes
img: assets/img/nyc_alpha_opt_10_day_all_data.png
importance: 3
category: work
---

At the start of the pandemic, the members of the Caltech community enrolled in the course CS-156 came together to build predictive models of the rapidly spreading disease, COVID-19.  Different teams contributed a variety of models to predict COVID-19 outcomes with uncertainty quantification on a per-county basis over a two week period.  The final predictions, which consisted of an ensemble of the best performing team models, included epidemiological, deep learning, and regression models. We then collaborated with the CDC-funded UMass-Amherst Influenza Forecasting Center of Excellence, by contributing our predictions to their [forecast hub](https://www.nature.com/articles/s41597-022-01517-w).  The goal of this hub is to compile, standardize, and ensemble predictions from groups across the country to be used for policy decisions and visualization.


<div class="row justify-content-sm-center">
    <div class="col-7">
        {% include figure.html path="assets/img/deaths_by_county_fixed.png" title="Covid-19 deaths per county in early April, 2020" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Mapping the deaths per US county for a ten day period in April, 2020
</div>
