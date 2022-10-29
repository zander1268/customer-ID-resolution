# Windjammer Consulting: Shopify Customer Identity Resolution
**Author**: [Alex FitzGerald](https://www.linkedin.com/in/alex-fitzgerald-0734076a/)

![windjammer header](Visuals/shopify_id_resolution.png)

## Overview
This project uses the Recordlinkage Python package to address the challenge of customer identification. I use a processing of potential pair matching and similarity scoring to identify true customer pairs resulting in truly unique customer file. My identity resolution methods found 238 duplicate customers resulting in a 5% reduction in the size of customer file. I achieved a 99% F1 score.

## Business Problem
Many Shopify merchants have use customer file that doesn't accurately reflect their truly unique customers because customers will often be classified as seperate when they are in fact the same customer entering different information. Some misidentifications are accidential resulting from misspellings while others intentionally use different email addresses to access discounts and gated services. This causes business problems for merchants because their customer analytics and marketing personalization are only as good as their customer identity resolution. If you don't know who is who, you can't properly assess key customer metrics like customer lifetime value, customer acquisition cost, churn, etc.

## Data
The data used was provided by Recordlinkage for practice purposes which I then manipulated to reflect the Shopify customer data structure. The data included 5,250 customer records with 5,000 truly unique customers as indicated by the Shopify ID which I used as the ground truth. Features used in the similary algorythms included; "First_Name","Last_Name","Date_of_Birth","city","State",and "Address_1".


## Methods
The first step in the process is to generate a new dataframe where the multi-index represents all possible pairs of the original dataframes. We can be smarter about our pairing by eliminating pairs that couldn't possibly be from the same individual. Using a technique called blocking we can tell the indexer object to only create pairs that match some criteria. I chose first name but you could also chose another feature. 

At this point I brought in some domain knowledge to be more intelligent about blocking. It's very common for customers to mispell their name or provide a nick name on some purchases and a full name on others. To better link these instances of similar names we can use a sorted neighbors algorythm to group names to then be used for blocking.

The process of identified potential pairs using the more discriminating sorted neighborhood process involves three steps

1.Taking the blocking series for each data frame (in our case "first name") and combining them into one series
2.Sorting the series
3.For each element in the series, the function looks within the specified window. 

If two elements from different series are within the window, they are added to the list of potential pairs.
