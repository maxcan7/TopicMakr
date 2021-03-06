# TopicMakr

## Summary
My Insight Data Engineering Fellowship 2019A New York Project  
  
TopicMakr is a proof of concept for a topic modeling platform for data scientists to train topics for industry use-cases such as recommendation systems.  
  
The project uses whole open source books scraped from Project Gutenberg. These books are read into an s3 bucket for processing and model training.  
  
The pipeline is a Latent Dirichlet Allocation (LDA) topic model. The book data are preprocessed, tokenized, and trained in pyspark. The model trains a pre-configurable number of topics from the book data, producing distributions for the probabilities of words given topics, and for topics given documents. Topics may be interpreted by visual inspection of the top N most probable words per topic (often 7), and the books may then be characterized by the most probable topics for each book.  
  
After the model is trained in pyspark, these data are stored in a postgres database, where some of the outputs may be viewed on topicMakr.ml, producing with a dash web app.      

## Directory Structure
    |-- README.md
    |-- configurations_template.sh
    |-- dash
    |    -- topicMakr_app.py
    |-- data
    |    -- scrape_gutenberg.sh
    |-- environment_setup
    |    -- spark_environment_setup.sh
    |    -- spark_standalone_setup.sh
    |-- images
    |    -- pipeline.png
    |-- src
    |    -- topicMakr_pyspark.py
    |-- topicMakr_sparksubmit.sh

## Pipeline Structure
![Image of Pipeline](https://github.com/maxcan7/TopicMakr/blob/master/images/pipeline.png)

## How-To
1. Clone this repository
2. Run the shell scripts in ./environment_setup/ folder
3. Make sure the requirements (below) have been met
4. Follow the ./data/scrape_gutenberg.sh script using your own file system (or replace the dataset with another text corpus)
5. Set up your configurations (see configurations_template.sh) 
6. Run topicMakr_sparksubmit.sh
7. The data tables can be displayed with a dash app using ./data/topicMakr_app.py, given your own postgres database and web domain
  
## Requirements  
* Unix operating system (this pipeline was tested using linux ubuntu)  
* An AWS cluster for processing  
* An AWS s3 bucket of raw text files of books (note that the pipeline currently contains code which may be specific to the formatting of book text on project gutenberg)  
* Hadoop (for spark)  
* Spark (for processing)  
* Python 2.7 (for pyspark, the python version of spark)  
* Postgres (or a similar SQL database)  
  
### AWS  
* The aws cluster was spun up using [Insight's pegasus](https://github.com/InsightDataScience/pegasus)  
* For general command line use, s3cmd must be installed on unix  
  
### Spark  
* See environment_setup folder for spark configuration  

### Python  
* Python version 2.7 was used for compatibility with other packages  
* The boto3 package was used for acccessing the s3 bucket  
* The fnmatch and numpy packages were used for data manipulation  
* The nltk package was used for a stop word list for model training  
* The model was trained using term frequency inverse document frequency (TF-IDF) vectors derived from the text files. The LDA model was implemented using pyspark machine learning (ml) package  
  
### Data  
* Project Gutenberg books were scraped using a modified version of [mbforbe's gutenberg scraping code](https://gist.github.com/mbforbes/cee3fd5bb3a797b059524fe8c8ccdc2b)  
* For the version of the script used in this pipeline, see /data/scrape_gutenberg.sh  
* This script requires s3cmd to be installed in unix 
