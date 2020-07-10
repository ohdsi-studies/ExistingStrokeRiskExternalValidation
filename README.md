Proof of concept demonstrating the utilization of the OHDSI data network to perform external model validation of existing stroke models
=======================================================================================================================================

<img src="https://img.shields.io/badge/Study%20Status-Repo%20Created-lightgray.svg" alt="Study Status: Repo Created">

- Analytics use case(s): **Patient-Level Prediction**
- Study type: **Methods Research**
- Tags: **External validation**
- Study lead: **Jenna Reps**
- Study lead forums tag: **[jreps](https://forums.ohdsi.org/u/jreps)**
- Study start date: **Jan 1, 2019**
- Study end date: **Feb 20, 2019**
- Protocol: **-**
- Publications: **[Here](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/s12874-020-00991-3)**
- Results explorer: **-**


A package for running the OHDSI network study to externally validate 5 existing stroke risk prediction models using the PatientLevelPrediction framework


Features
========
  - Creates target cohort of females newly diagnosed with atrial fibrilation between ages 35-65
  - Creates 4 outcome cohorts of various stroke definitions
  - Implements 5 existing stroke risk prediciton models and validates on data in OMOP CDM for each target and outcome cohort combination
  - Sends summary results 

Technology
==========
  ExistingStrokeRiskExternalValidation is an R package.

System Requirements
===================
  Requires R (version 3.3.0 or higher).

Dependencies
============
  * PatientLevelPrediction
  * PredictionComparison

Getting Started
===============
  1. In R, use the following commands to download and install:

  ```r
  #================================= STEP 1: INSTALL PACKAGES ==================================
install.packages("devtools")
devtools::install_github("OHDSI/PatientLevelPrediction")
devtools::install_github("OHDSI/PredictionComparison")
devtools::install_github("ohdsi-studies/ExistingStrokeRiskExternalValidation")

library('ExistingStrokeRiskExternalValidation')
# view the models - this should pop up a View with the model info
viewModels()
                    
#================================= STEP 2: MAIN STUDY ==================================
options(fftempdir = 'T:/yourFftemp')
dbms <- yourDbms
user <- yourUsername
pw <-yourPassword
server <- Sys.getenv('server')
port <- Sys.getenv('port')
connectionDetails <- DatabaseConnector::createConnectionDetails(dbms = dbms,
                                                                server = server,
                                                                user = user,
                                                                password = pw,
                                                                port = port)

databaseName = 'friendlyDatabaseName'
cdmDatabaseSchema <- 'yourCdmDatabaseSchema'
cohortDatabaseSchema <- 'yourCohortDatabaseSchema'
cohortTable <- 'existingStrokeVal'
outputLocation <- 'C:/existingStrokeVal'

ExistingStrokeRiskExternalValidation::main(
  connectionDetails=connectionDetails,
  oracleTempSchema = NULL,
  databaseName=databaseName,
  cdmDatabaseSchema=cdmDatabaseSchema,
  cohortDatabaseSchema=cohortDatabaseSchema,
  outputLocation=outputLocation,
  cohortTable=cohortTable,
  createCohorts = T,
  runAtria = T,
  runFramingham = T,
  runChads2 = T,
  runChads2Vas = T,
  runQstroke = T,
  summariseResults = T,
  packageResults = T,
  N=10)

# After checking the compressed folder containing the shareable results submit the results
# either email them to study admin or run
submitResults(exportFolder = file.path(outputLocation,'export',paste0(databaseName,'.zip') ), keyLocation, userName)
# where keyLocation, userName are provided by request


```

License
=======
  ExistingStrokeRiskExternalValidation is licensed under Apache License 2.0

Development
===========
  ExistingStrokeRiskExternalValidation is being developed in R Studio.

