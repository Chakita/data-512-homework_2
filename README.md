# Analysis of Quality of Wikipedia Articles on Global Politicians

This repository contains the data and code for the analysis of quality of Wikipedia articles on global politicians. The goal of the analysis is to find the top 10 countries and regions with the highest coverage (number of articles per capita) and with the most pewr capita high quality articles. Parts of the code were adapted from this [example notebook](https://drive.google.com/file/d/1GN1ULxKombHRzVsNKzj7tBhnBrSWUWXc/view?usp=drive_link) provided by Dr. David McDonald. The Wikipedia Politicians dataset and the Population dataset where also extracted and provided by Dr. David McDonald.

## Data
- The Wikipedia [Category:Politicians](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) by nationality was crawled to generate a list of Wikipedia article pages about politicians from a wide range of countries. This data is in this repository as politicians_by_country.AUG.2024.csv.
- The population data is available in CSV format as population_by_country_AUG.2024.csv from the repository. This dataset was downloaded from the [world population data sheet](https://www.prb.org/international/indicator/population/table) published by the Population Reference Bureau.
- The article quality is extracted using the [ORES](https://www.mediawiki.org/wiki/ORES) API. ORES (Objective Revision Evaluation Service) is a web service and API that provides machine learning as a service for Wikimedia projects. The different quality levels that ORES assignes to an article (from best to worst) are as follows:
  - FA - Featured article
  - GA - Good article (also known as A-Class)
  - B - B-Class article
  - C - C-Class article
  - Start - Start-class article
  - Stub - Stub-class article
- ORES requires a specific revision ID of an article to be able to make a label prediction. For our analysis, we will use the latest revision of an article. To obtain the latest revision ID for an article, we will use the [Article Page Info MediaWiki API](https://www.mediawiki.org/wiki/API:Info).
- The resulting dataset consisting of country, region, population, article_title (politician name), revision_id and article_quality is stored in wp_politicians_by_country.csv which can be found on the repository.
- wp_countries-no_match.txt consists of  all countries for which there are no matches i.e either the population dataset does not have an entry for the equivalent Wikipedia country, or vice-versa.
- articles_without_scores.txt consists of all the politicians for whom we were unable to extrqact the latest revision ID for their corresponding articles through the page info API. This seems to be because these articles are not in English.

All the data collected in this analysis through the APIs are subject to the terms and conditions of the [Wikimedia Foundation terms of use](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use) which states that users can freely access and reuse the content on Wikimedia platforms, including articles and datasets, under free and open licenses. Any contributions made to Wikimedia platforms must be licensed under a free and open license, allowing the content to be freely shared and reused by others. Users are responsible for their edits and contributions and must adhere to laws, avoid copyright infringement, and respect the platform's policies.

## Research Implications
While working on the analysis, I found out about the ```country-converter``` package which was pretty handy to map countries to regions. For this analysis, we map each country to the region that is the lowest in the hierarchy (for example, we map India to South Asia and Singapore to South East Asia). I found that these regions are actually the [United Nations geoscheme](https://en.wikipedia.org/wiki/United_Nations_geoscheme#:~:text=The%20United%20Nations%20geoscheme%20is,on%20the%20M49%20coding%20classification.) which is a scheme that divides 248 countries into 22 geographical subregions.

From the analysis results, one of the things that stands out is the per capita coverage in countries which experience heavy censorship and also countries in which English is not a primary language.
Countries like China and Saudi Arabia have relatively lower number of articles due to their censorship policies coupled with the fact that English is not a primary language spoken here. The other countries in the bottom 10 list appear to fall into the same category as well. 

Before performing the analysis, I expected the results to be biased against countries that do not speak English as a primary language since Wikipedia is predominatly a English based website and the ORES model might only be trained on English articles and articles written by non-native English speakers are smaller in number and are prone to being labelled as poor quality by the model due to bias. Most of these countries and regions were English is not a primary language might have more and better quality articles in their respective primary languages but that is not captured and accounted for by the ORES model.

Our analysis suggests that English Wikipedia might not be a good data source to gauge article quality for articles concerning specific countries where English not widely spoken and also countries where media faces considerable censorship especially for content related to socio-political matters. Also in some countries, Wikipedia might not be as popular as a source of information as some of their local websites. For isntance, China has Baidu which is more popular and is likley to contain more and higher quality articles. Our findings froom the analysis are consistent with these observations. Most of top 10 countries and regions with high coverage and high aritcle quality are all in regions where most of the population used English as their primary language. 

The data, especially the coverage might also be slightly biased against highly populated countries. These countries like India and China have a lower per capita value for both coverage and number of high quality articles since they have an especially large population but a disproportionately large portion of them do not speak English. To allieviate these issues and address the limitations, researchers should take into account other forms of online encyclopedias these countries use, the number and quality of the non-English Wikipedia pages produced by these contries and account for the large denominator for highly populated countries, perhaps by normalising these coverage and per capita quality metrics.


