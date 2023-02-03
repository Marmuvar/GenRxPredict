# GenRxPredict: Analyzing Pharmaceutical Patents and Time-to-Market for Prediction of Generic Product Competition

## A Thesis Presented for the Master of Data Science and Analytical Storytelling from Truman State University (Kirksville, MO)

Full text is available ![here](https://github.com/Marmuvar/GenRxPredict/blob/main/Benmuvhar-Classification%20of%20Generic%20Manufacturers%20and%20Competition%20in%20the%20Pharmaceutical%20Industry.pdf)  

The following overview abridges key strategies from the thesis.  

## Overview

The United States Pharmaceutical Industry provided almost 194 billion daily doses of pharmaceuticals in 2021, representing around \$776 billion in wholesale costs and \$80 billion in patient out-of-pocket costs (IQVIA, 2022). Pharmaceuticals enter commercial trade after the Food and Drug Administration (FDA) issues a marketing authorization based on either a new drug application (NDA) for a novel (“branded”) drug product or an abbreviated new drug application (ANDA) for a generic drug product. An NDA documents extensive studies of safety and efficacy for the drug molecule and the drug product and can represent over a billion dollars of development costs and over a decade of research expenditures (Sun et al, 2022).
In contrast, a generic sponsor of an ANDA does not need to prove safety and efficacy through clinical trials.  Instead, they demonstrate “therapeutic equivalency” to the branded product through a small clinical study in volunteers showing that there are no differences in the rate or amount of drug absorbance into the body.  Further, the products must be pharmaceutically equivalent by having the same active pharmaceutical ingredient, same dosage form, same strengths, and same route of administration.  (Center for Drug Evaluation and Research (CDER), 2017). Since clinical effectiveness is a function of drug concentration, demonstration of similar drug concentrations makes it unnecessary for generic products to duplicate extensive clinical performance trials required for branded product approval (Midha & McKay, 2009).  As a result, two to five years of generic drug development might only cost around $2.5 to 5 million dollars, or around 1/400th the cost of the branded pharmaceuticals.
The 1984 Drug Price and Competition Act balanced the intellectual property rights of innovative brand pharmaceuticals with a regulatory framework for generic product development.   Novel products became eligible for additional exclusivity periods and extensions to the twenty years of patent protection based on regulatory review time.  In exchange, the brand companies could not pursue litigation against a generic pharmaceutical company for patent infringement until after an ANDA was submitted.  Generic drug companies gained the right to pursue necessary clinical and development studies without risk of patent infringement litigation.  

Generic competition erodes drug price.   The first generic product to market is sold at a significant discount.  After 6 or more competitors, the price is as little as 95% of wholesale cost (Conrad and Lutter, 2019). 

Many companies who are approved to make a drug no longer market the product.  Declining drug price and low contribution to company revenue were correlated with drug product shortages between 2013 and 2017 (FDA, 2019).

The average number of patents for each approved new drug product has steadily increased since the Drug Price and Competition Act was passed in 1984. Because patent expiry dates may be staggered due to regulatory agreements, the increased number of patents provide opportunity for brands to extend the period without generic competition.  In parallel, the median time between generic and brand approval has also increased.  This may reflect both time elapsed while waiting for patents to expire and generic companies finding profitability in older drug products.  While many generics are approved in the year following patent expiry, a large majority are approved in the years preceding expiry.  

![BrandPatCt](brandPatCt-1.pdf) 

![GenFrBrnApp](genFrBrnApDate-1.pdf) 

![GenVsLastPat](genVsBranLasPat-1.pdf)


##Objective  

Given the benefits provided by generic product availability and the risk associated with bringing new products to market, I wanted to understand:

*Do patents, dosage forms, ingredients, and packaging provide a basis for predicting generic competition?  
*Can generic competition be predicted relative to patent expiration?  

## Data 

### Approved Drug Products with Therapeutic Equivalence Evaluations. 25th to 42nd Editions.  

Each year, the FDA publishes the commonly referenced “Orange Book”, an official record  of drug products approved for marketing in the United States. Further, it specifies the relationship between the original drug products and equivalent generics. Claimed patent protections and regulatory exclusivity in effect for each drug product application are also listed.  While an electronic version is available, details of patents and bioequivalence categories are retired as patents expire or products are discontinued.  Therefore, historical PDF versions were required.  


### Structured Product Labels  

Each approved drug product carries an exhaustive labeling of ingredients, packaging, dosing instructions, clinical studies, side effects and other information.  The FDA requires all labels to follow a standardized XML format, which facilitates consistent extraction of data fields.  The National Library of Medicine maintains the comprehensive repository of labels for all currently marketed products.  


## Data Processing

For product application and patent records, custom modules were written in Python to parse and clean data following PDF conversion using modules from pypdf2 and tabula.  For product labels, custom modules written in Python processed the data after using element.etree to parse the XML structures.

## Market Analysis

## Methodology  

Data sets were formed comprising of features including counts of patents, the offset between brand approval and patent expiry, dosage forms, ingredients, and packaging components.  Products were filtered to include brand products with patent expiry dates between 1990 and 2010, which ensured time for generic products to reach market.  Each brand product was categorized as to whether any generic product was approved before the first or last patent expiry. 

Training sets were formed from 70% of the samples, and minority classes were up-sampled to 50%  Data were scaled between 0 and 1 based on minimum and maximum values.  

All analysis was performed in R implementing packages available from CRAN, including xgboost for boosted gradient analysis, e1071 for support vector machine analysis, and rpart for decision tree analysis.  

## Results

### Boosted Gradient | Baseline Features | Competition before Last Patent Expiry | Multiple Administration Routes  

Overall model accuracy was statistically significant (greater than no-information rate (NIR).  When baseline patent features were supplemented with product ingredients, incremental improvement in accuracy was observed.  

In general, brand products approved well-before first patent expiry were more likely to have generic competition before its final patent expired.  While tablet dosage forms were more likely to have generic competition, a liquid or injectable dosage form predicted an absence of early competition.

Low previous product count a predictor of absence of competition
Presence of water predicts absence of competition.   This aligns with injectables and liquid observation
Common tablet ingredients (crospovidone, starch, and mannitol) predict absence of competition
Other common tablet ingredients (magnesium stearate, hypromellose, lactose monohydrate) predict competition

![BaseShap](baseXGMultiShap-1.pdf) 

### Boosted Gradient | Baseline Features | Competition before Last Patent Expiry | Oral Administration Routes  

![BaseIngOralLaShap](baseIngXGOralLaShap-1) 
Shorter times before first patent expiry predictive of absence of competition. This is intuitive, as several years are required for generic product development.  A high number of patents, howver, is predictive of competition, while a high number of exclusivities predicts the absence of competition.  Among the ingredients, sucrose, mannitol, and hydroxypropyl cellulose predict competition, while lactose, talc, and several colorants predict an absence of competition.  All of these ingredients are common.  With the exception of hydroxypropyl cellulose, none would indicate a complex dosage form.      


### Boosted Gradient | Baseline Features | Competition before First Patent Expiry | Oral Administration Routes  

![BaseIngOralFirShap](baseIngXGOralFirShap-1.pdf)
Longer times before first patent expiry predictive of absence of competition.
Hydroxypropyl cellulose is predictive of absence of competition.  
Depending on grade, it can be associated with more complicated tablet formulations.
Polysorbate 80 and polyethylene glycol can be found in liquid formulations.  In this model, they are predictive of the presence of competition. 

## Limitations and Further Studies  

Market dynamics influence product selection.  While the models have limited predictive accuracy arising from trends in historical patent and formulation data, greater accuracy might be gained by including sales revenue and volume data.

The machine learning algorithms used in the study are influenced by hyperparameters, a selection of calculation variables and criteria.  While these parameters were tuned using a limited test set of data, they were broadly applied to multiple sets of variables.  Additional tuning studies with each variable set could change model accuracy and precision.  

Finally, due to expansion and consolidation in the pharmaceutical industry, certain assumptions were made about the provenance of products and companies.  For example, company incorporation status (e.g. "inc", "ltd") was removed from names.   This may discount business units that were operationally independent from more experienced parts of a corporation.  Similarly, no attempt was made to address corporate name changes arising from re-branding, such as Valeant becoming Bausch Healthcare in 2018.   This could limit the perceived experience a company has with certain dosage forms.  

## Conclusions

Overall, the study has justified the importance of predicting competition in the generic pharmaceutical industry.  Custom modules were built to successfully extracted and compile data from a multi-volume annual report in PDF format.  Last, models predicted competition before first and last patent expiry using data derived from the FDA Orange Book and product labels.  


## Required Packages
See [package bibliography](packages.bib)

## Compilation 

After cloning repository, run bookdown::render_book() from the repository root directory.
Individual code sections can then be explored within _main.rmd generated from the command.

## Works Cited
See [bibliography](book.bib)
