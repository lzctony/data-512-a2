# Data 512 A2: Bias in data
## Project Goal
The goal of this assignment is to explore the concept of 'bias' through data on Wikipedia articles - specifically, articles on political figures from a variety of countries. First, we need to use a machine learning service called **ORES** (Objective Revision Evaluation Service) to estimate the quality of each article for the wekipedia's dataset. Then, we will combine a dataset of Wikipedia articles with a dataset of country populations by matching the country names. After combining these two dataset, I am able to  to perform an analysis of how the coverage of politicians on Wikipedia and the quality of articles about politicians varies between countries such as the following:
* the countries with the greatest and least coverage of politicians on Wikipedia compared to their population.
* the countries with the highest and lowest proportion of high quality articles about politicians.

## Tool
I use Jupyter Notebook write Python code to access, manipulate and plot the data.

You can install Python and Jupyter Notebook by downloading **Python 3.6** version from [ANACONDA](https://www.anaconda.com/download/#macos)

The following Python Packages are used to throughout this project:
* **requests**
* **json**
* **csv**
* **pandas**

## License of The Source Data

**The wikipedia dataset** (Politicians by Country from the English-language Wikipedia) 
* [Terms & Conditions](https://figshare.com/terms)

**The Population dataset**

Copyright © 2016, Population Reference Bureau. All rights reserved.
* [Privacy Policy](http://www.prb.org/DataFinder/Topic/~/link.aspx?_id=11A2A1677D184053936CE705FAEDEC1D&_z=z)
* [Contact](http://www.prb.org/DataFinder/Topic/~/link.aspx?_id=314EE6FCD2DD46B986A742458744C76A&_z=z)

**ORES**
By using the API **ORES** (Objective Revision Evaluation Service), you agree to Wikimedia's Terms of Use and Privacy Policy.
* **Terms of Use** [(Wikimedia Foundation terms of use)](https://wikimediafoundation.org/wiki/Terms_of_Use/en)
* **Privacy Policy** [(Privacy policy)](https://wikimediafoundation.org/wiki/Privacy_policy)
See [https://ores.wikimedia.org](https://ores.wikimedia.org) for more information on how to use the API.

Also under CC-BY-SA 4.0.

## Wikipedia Dataset
The wikipedia dataset can be found on [Figshare](https://figshare.com/articles/Untitled_Item/5513449).
The data was extracted via the Wikimedia API using the associated code. It is formatted as a CSV and saved as **page_data.csv** in the "data" directory. Columns are:

| Column | Description |
| --- | --- |
| country | 'containing the sanitised country name, extracted from the category name' |
| page | 'containing the unsanitised page title' |
| last_edit | 'containing the edit ID of the last edit to the page' |

You can also download the dataset directly by clicking [**DOWNLOAD**](https://ndownloader.figshare.com/files/9614893)

## Getting Article Quality Predictions
Next step, we need to get the predicted quality scores for each article in the Wikipedia dataset above. We're using a Wikimedia API endpoint for a machine learning system called **ORES** (Objective Revision Evaluation Service). [**ORES**](https://www.mediawiki.org/wiki/ORES) estimates the quality of an article (at a particular point in time), and assigns a series of probabilities that the article is in one of 6 quality categories. The options are, from best to worst:

| Column | Description |
| --- | --- |
|  **FA** | Featured article |
|  **GA** | Good article |
|  **B** | B-class article |
| **C** | C-class article |
| **Start** | Start-class article |
| **Stub** | Stub-class article |

The documentation can be found [here](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model).

When you query the API, you will notice that ORES returns a prediction value that contains the name of one category, as well as probability values for each of the 6 quality categories above. For this project, I only capture and use the value for prediction.

**Note:** There are four articles in the Wikipedia data that could not get the prediction values. 

## Population data
The population data is on the [Population Research Bureau website](http://www.prb.org/DataFinder/Topic/Rankings.aspx?ind=14). Download this data as a CSV file (hint: look for the 'Microsoft Excel' icon in the upper right).

It is formatted as a CSV and saved as **Population Mid-2015.csv**. Columns are:

| Column | Description |
| --- | --- |
| Location | 'country name' |
| Location Type | 'country' |
| TimeFrame | 'Mid-2015' |
| Data Type | 'string' |
| Data | 'population' |
| Footnotes | '' |

You can also download the dataset directly by clicking [**DOWNLOAD**](http://www.prb.org/RawData.axd?ind=14&fmt=14&tf=76&loc=34235%2c249%2c250%2c251%2c252%2c253%2c254%2c34227%2c255%2c257%2c258%2c259%2c260%2c261%2c262%2c263%2c264%2c265%2c266%2c267%2c268%2c269%2c270%2c271%2c272%2c274%2c275%2c276%2c277%2c278%2c279%2c280%2c281%2c282%2c283%2c284%2c285%2c286%2c287%2c288%2c289%2c290%2c291%2c292%2c294%2c295%2c296%2c297%2c298%2c299%2c300%2c301%2c302%2c304%2c305%2c306%2c307%2c308%2c311%2c312%2c315%2c316%2c317%2c318%2c319%2c320%2c321%2c322%2c324%2c325%2c326%2c327%2c328%2c34234%2c329%2c330%2c331%2c332%2c333%2c334%2c336%2c337%2c338%2c339%2c340%2c342%2c343%2c344%2c345%2c346%2c347%2c348%2c349%2c350%2c351%2c352%2c353%2c354%2c358%2c359%2c360%2c361%2c362%2c363%2c364%2c365%2c366%2c367%2c368%2c369%2c370%2c371%2c372%2c373%2c374%2c375%2c377%2c378%2c379%2c380%2c381%2c382%2c383%2c384%2c385%2c386%2c387%2c388%2c389%2c390%2c392%2c393%2c394%2c395%2c396%2c397%2c398%2c399%2c400%2c401%2c402%2c404%2c405%2c406%2c407%2c408%2c409%2c410%2c411%2c415%2c416%2c417%2c418%2c419%2c420%2c421%2c422%2c423%2c424%2c425%2c427%2c428%2c429%2c430%2c431%2c432%2c433%2c434%2c435%2c437%2c438%2c439%2c440%2c441%2c442%2c443%2c444%2c445%2c446%2c448%2c449%2c450%2c451%2c452%2c453%2c454%2c455%2c456%2c457%2c458%2c459%2c460%2c461%2c462%2c464%2c465%2c466%2c467%2c468%2c469%2c470%2c471%2c472%2c473%2c474%2c475%2c476%2c477%2c478%2c479%2c480)

## Final Data File
After retrieving and including the **ORES** (Objective Revision Evaluation Service) data for each article. I am able to merge the wikipedia data and population data together for further analysis. Both datasets have fields containing country names for just that purpose. After merging the data, I drop the obervations that cannot be matched (Either the population dataset does not have an entry for the equivalent Wikipedia country, or vice versa). The variables of the final dataset are as shown below:

| Column | Description
| --- | --- |
| country | 'country name' |
| article_name | 'article name' |
| revision_id | 'last edit id' |
| article_quality | 'article quality prediction by ORES' |
| population | 'population of the country' |

Note: the revision_id here is the same thing as last_edit in Wikipedia Dataset, which we used to get article predictions from the ORES API.

## Table
After obtaining the final dataset, based on the analyses below:
* the countries with the greatest and least coverage of politicians on Wikipedia compared to their population.
* the countries with the highest and lowest proportion of high quality articles about politicians.
I have made 4 tables. Moreover, **table1** and **table2** refer to the first analysis while **table3** and **table4** refer to the second analysis.

1. **10 highest-ranked countries** in terms of number of politician articles as a proportion of country population

| country | article_population_percentage(%) |
| --- | --- |
| Nauru | 0.488029% |
| Tuvalu | 0.466102% |
| San Marino | 0.248485% |
| Monaco | 0.105020% |
| Liechtenstein | 0.077189% |
| Marshall Islands | 0.067273% |
| Iceland | 0.062268% |
| Tonga | 0.060987% |
| Andorra | 0.043590% |
| Federated States of Micronesia | 0.036893% |

2. **10 lowest-ranked countries** in terms of number of politician articles as a proportion of country population

| country | article_population_percentage(%) |
| --- | --- |
| India | 0.000075% |
| China | 0.000083% |
| Indonesia | 0.000084% |
| Uzbekistan | 0.000093% |
| Ethiopia | 0.000107% |
| Korea, North | 0.000156% |
| Zambia | 0.000168% |
| Thailand | 0.000172% |
| Congo, Dem. Rep. of | 0.000194% |
| Bangladesh | 0.000202% |

3. **10 highest-ranked countries** in terms of number of GA and FA-quality articles as a proportion of all articles about politicians from that country

| country | high_quality_percentage(%) |
| --- | --- |
| Korea, North | 23.076923% |
| Saudi Arabia | 11.764706% |
| Uzbekistan | 10.344828% |
| Central African Republic | 10.294118% |
| Romania | 9.770115% |
| Guinea-Bissau | 9.523810% |
| Bhutan | 9.090909% |
| Vietnam | 8.376963% |
| Dominica | 8.333333% |
| Mauritania | 7.692308% |

4. **10 lowest-ranked countries** in terms of number of GA and FA-quality articles as a proportion of all articles about politicians from that country

| country | high_quality_percentage(%) |
| --- | --- |
| Turkmenistan | 0.0% |
| Tajikistan | 0.0% |
| Monaco | 0.0% |
| Mozambique | 0.0% |
| Nauru | 0.0% |
| Tonga | 0.0% |
| Cape Verde | 0.0% |
| Guadeloupe | 0.0% |
| Kazakhstan | 0.0% |
| Suriname | 0.0% |