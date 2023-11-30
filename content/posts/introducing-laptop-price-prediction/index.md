---
title: "Introducing my latest project: Laptop Price Prediction"
date: 2023-11-01T00:03:20-08:00
featuredImage: "posts/introducing-laptop-price-prediction/featured.png"
tags: ["MLZoomCamp", "Machine Learning"]
description: "A model and website I built for the MLZoomCamp Midterm project to estimate laptop prices from specs"
---

## Introducing [Laptop Price Prediction](https://laptop-price-prediction.tzvi.dev/)! 🎉 🎉 🎉 

### What is it?
There are thousands of laptops on the market with very similar specifications and prices. Figuring out which one is a good value for the money is difficult. My goal for this project was to create a model that can return a price target based on specs passed to an ML model based on data scraped from Amazon in October 2023. 

_Fair Warning: It isn't very accurate (has an RMSE of ~400) and I am looking forward to hearing from anyone who has ideas on how to improve the model._

### Why did I build this
This is my Midterm Project submission for the [MLZoomCamp](https://github.com/DataTalksClub/machine-learning-zoomcamp) and in order to build this I had to:
- Clean the dataset in order to improve the accuracy.
- Investigate the values in the dataset using EDA.
- Build multiple models including LinearRegression, Ridge Regression, and an ensemble model called XGBoost to compare the performance of each.
- Build and deploy a web service to host the model and serve predictions.