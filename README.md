**New!** The tool can now handle duplicate columns and much more unstructured data.  Making logical decisions and giving the user full authority if they want to modify as needed. So in scenarios when there is some anomaly, the tool will give an option if the user wants to determine what happens (duplicate column, unnecessary column, no sample data, irregular sample data, etc ). I relevant tutorial will be uploaded and the link will be provided soon!


cBioPortal needs our data in a certain format with metadata information to be eligible to be uploaded.  We can use the script in the following path to make the necessary modifications and metadata. 

## Overall Work Flow :

1. Collect the clinical data and its path
2. Use the cbioportal_study_parser python command line tool to make a study folder/ directory.
3. Move/ Copy the study to the in locally installed cBioportal instance(in the study directory) in planet.
4. Use the script provided by cBioportal to take our study to the cBioportal instance.
5. Now we can see our data in local cBioportal after broadcasting to our local host

Examples for steps 3 and 4 are provided here, but also [Deploy with Docker](https://docs.cbioportal.org/deployment/docker/) (cbioportal official documentation) as provided most recent way for step 4. 

### Step 1: Copy path of your clinical file

The tool now expects the clinical file to be in csv or excel. If you need any custom use case of the data, please know which column is patient specific (overall survival)and which column is sample specific(mutation, tumor class) . 

**Note**: This patient/ sample specific data separation is usually handled well automatically with logics and with a predefined column in most cases.  Most of the columns are labelled as sample column except the defined patient columns. So knowing the patient columns can help if the data has some different name other than predefined patient columns. 

### Step 2: cBioportal study parser

This script processes clinical sample data from an Excel file and prepares it for further analysis in cBioportal. The modification script is located at 

```bash
 # cd  /usr/share/cbioportal/cbioportal_study_parser
 #~/usr/share/cbioportal/cbioportal_study_parser
 # For the latest version, use the one below
cd /usr/share/cbioportal/cbioportal_study_parser/CBioportal_Study_Parser 
```

## General User Guide:

There are two ways we use cBioportal_study_parser tool. There are two different versions. 

1. **v3** takes prompts( it will ask questions) from the user and behaves according to the instructions from the user.  More intuitive and flexible. 
2. **v2** takes all arguments at once as an argument in the code. More useful when used as part of a pipeline. 

**Construction**: version 2 does not have all the features yet, so less robust and less flexible with user data requirements. But we are on the way to making it robust.

After using either version we will have a directory or folder containing meta data files and corresponding data files. In the language of cBioportal, this is called a study. We need to update this file in the cbioportal installation in our local installation on Planet: Pluto or online.

**Recommendation 1:**  Please use the latest stable version of Python 3 and Pandas so that if there are any part of the code which contains depreciated syntax we can identify early. This will ensure that our tool use usable for longer without getting backdated. 

**Recommendation 2:**  We can use uv a very fast python virtual environment manager. The guide line is in Notion page Titled [“Python Projects and Environments”](https://www.notion.so/Python-Projects-and-Environments-1dfca2bf49eb802ea955f7fed8db21b1?pvs=21) and the [official guide](https://docs.astral.sh/uv/) if necessary.

```bash
# Example
curl -LsSf https://astral.sh/uv/install.sh | sh
uv venv mybBioenvironment --python 3.13
source mybBioenvironment/bin/activate
uv pip install numpy pandas openpyxl

```

## V3 User guide: User question/answer way :

We need to use the cBioportal_study_parser_v3.py.  Required packages are standard Python 3.6+ (tested on Python 3.11 ), pandas, numpy, and os. The os library is included in the standard Python 3  distribution, so most likely not required to be installed.   

```bash
# Use the commented line to make use you have necessary libraries
# uv pip install numpy pandas openpyxl
python cBioportal_study_parser_v3_1.py
```

Then there will be the following prompt(question/answer-like) on the commend line or terminal: 

1. Enter the Clinical data Excel or csv file path :

Please copy the full path of the clinical data (excel/csv). We tested on PC (windows 11) like path. Please me know if it works with other path style mac, Linux etc. 

1. The following intuitive questions will be asked. 

Enter the name of the study:  
Enter the cancer type (or 'coadread' if not provided):
Enter the cancer study identifier (or 'name of the study' if not provided) :
Enter the description of the study (default: 'name of the study'):

Note: other cancer types can be found here: [Introduction](https://docs.cbioportal.org/file-formats/#cancer-type)

According to the [cBioPortal file formats documentation](https://docs.cbioportal.org/file-formats), cancer types are typically represented using **TCGA (The Cancer Genome Atlas) study abbreviations**. Here are some common cancer type abbreviations used in cBioPortal:

### **Common TCGA Cancer Type Abbreviations:**

- **ACC** – Adrenocortical Carcinoma
- **BLCA** – Bladder Urothelial Carcinoma
- **BRCA** – Breast Invasive Carcinoma
- **CESC** – Cervical Squamous Cell Carcinoma
- **CHOL** – Cholangiocarcinoma
- **COAD** – Colon Adenocarcinoma
- **DLBC** – Diffuse Large B-Cell Lymphoma
- **ESCA** – Esophageal Carcinoma
- **GBM** – Glioblastoma Multiforme
- **HNSC** – Head and Neck Squamous Cell Carcinoma
- **KICH** – Kidney Chromophobe
- **KIRC** – Kidney Renal Clear Cell Carcinoma
- **KIRP** – Kidney Renal Papillary Cell Carcinoma
- **LAML** – Acute Myeloid Leukemia
- **LGG** – Lower Grade Glioma
- **LIHC** – Liver Hepatocellular Carcinoma
- **LUAD** – Lung Adenocarcinoma
- **LUSC** – Lung Squamous Cell Carcinoma
- **MESO** – Mesothelioma
- **OV** – Ovarian Serous Cystadenocarcinoma
- **PAAD** – Pancreatic Adenocarcinoma
- **PCPG** – Pheochromocytoma & Paraganglioma
- **PRAD** – Prostate Adenocarcinoma
- **READ** – Rectum Adenocarcinoma
- **SARC** – Sarcoma
- **SKCM** – Skin Cutaneous Melanoma
- **STAD** – Stomach Adenocarcinoma
- **TGCT** – Testicular Germ Cell Tumors
- **THCA** – Thyroid Carcinoma
- **THYM** – Thymoma
- **UCEC** – Uterine Corpus Endometrial Carcinoma
- **UCS** – Uterine Carcinosarcoma
- **UVM** – Uveal Melanoma

**Additional Notes:**

- Some studies may use **custom abbreviations**, but TCGA codes are the standard.
- For non-TCGA studies, check the specific dataset in cBioPortal for naming conventions.

1. Then there will question on the genetic alteration type.  If not provided, clinical will be used since we are currently working mostly with that.

Enter the genetic alteration type (default: 'CLINICAL'):

1. Lastly, the tool will ask where to save the study. If nothing is provided, the study will be saved in the current working directory. 

Enter the working directory (default: './'):

1. There will be the following questions on customization of the study according to the user needs. (Details coming up)
- do you want to specify irrelevant columns. Press y for yes and n for no(tool will search predefined irrelevant column features in the data) :  
Using default irreverent columns
- do you want to specify patient_collumns? Press y for yes and n for no(tool will search predefined features in the data) :  
Using default patient columns.
- do you want to specify sample_collumns. If Press y, the tool will look for the collums you define in the input file. If Press n, the tool will take all the collumns in found sample collumn without the patient collumns(This usually is most of the case) :
- If the tool see some collum names repetation it will ask how to handle this.

## V2 user guide: User pass command-line arguments:(under construction)

The script requires several command-line arguments to define study parameters and file locations.

```bash
python cBioportal_study_parser.py -f <excel_file> -n <study_name> -ct <cancer_type> -csi <cancer_study_identifier> [options]

```

Example:

```bash
python cBioportal_study_parser.py -f 2422_DACHS-CRC-DX_CLINI.xlsx -n "2422_DACHS-CRC-DX_CLINI" -ct coadread -csi colorectal_cancer_NCT_Dresden_2025

```

### Step 3: Copy/ Move the study folder to the local cBioportal instance

We can either upload the study file to the online official instance of cBioportal (https://docs.cbioportal.org/) or to any local installation(in our case, hosted in Planet).  For the second case we need to copy our study folder  such as GECCO(the folder should contain meta and data files) to the locally(like in Planet) installed cBioportal instance’s **study** directory.

```bash
cp -r path/to/your/study/folder/made/by/the/tool  /usr/share/cbioportal/cbioportal-docker-compose/study
# cp -r [your study path made by python cBioportal_study_parser.py] [cBioporal_image/study]
# or use scp instead of cp, especially for remote server file transfer
# example : 
# scp D:\EKZF\cbioportal-usage\FOXTROT-CRC-DX_CLINI.xlsx ishrak@pluto:/usr/share/cbioportal/cbioportal_study_parser/clini_file

```

### Step 4 : Import the study with cBioportal

We now use the cBioportal provided script to import the study in our locally installed(in a Docker container) instance of cBioportal. 

```bash
cd /usr/share/cbioportal/cbioportal-docker-compose
#cd cbioportal-docker-compose
docker compose up -d
# or docker-compose up for older versions of Docker and compose
# -d is optional for detached mode
docker compose run cbioportal metaImport.py -u http://cbioportal:8080 -s study/lgg_ucsf_2014/ -o
# docker-compose run for older version

```

### Step 5:  Analysis and visualization in cBioportal

option 1 : If you are already logged into one of the planets like Pluto you can directly broadcast onto your browser. Paste this in your browser if you are logged into Pluto.  

```bash
http://192.168.33.27:8080/datasets
# address_remote_server:8080
```

In case you run into a problem, let us know. That helps with continuous improvement.