# Project Journal: CryptoTrace

## 6 Nov 2026
- Did: Today I have created the cryptotrace repository on GITHub and set up the project folders, .gitignore file and he CI  check in vs code 
- Problem: Managed to mess up the initial setup by accidentally cloning the repository recursively inside itself three times, leaving my Git config on the default "krishana bikram Shahi" placeholder, and starting out with a completely blank .gitignore.
- Fix: Cleaned it all up by stripping out the nested duplicate directories, properly configuring my real name and email in Git, and writing out a custom .gitignore file tailored to the project.
- Learned: Left the .gitignore blank, which risked accidentally leaking passwords, environment keys, and private data to GitHub.
- Next: I am  doing signup in databrics and linked to my github repository


## 2026-11-07: Repository and Databricks setup

**What I did**
- Created the GitHub repository `cryptotrace`.
- Added a `.gitignore` file so data files are never uploaded.
- Linked a Databricks Git folder to the repository.

**What I learned**
- The Elliptic data licence does not allow sharing, so the CSV files must stay out of GitHub.

## 2026-11-17: Raw data upload and schemas

**What I did**
- Created the schemas `bronze`, `silver` and `gold` in Databricks.
- Created a volume `bronze.raw` and uploaded the three Elliptic CSV files.

**What went wrong**
- I expected the features file to be about 100 MB. It was 657.73 MB.

**How I fixed it**
- I checked the numbers. About 204,000 rows with 167 columns is a large file, so the size is normal.

**What I learned**
- Check real file sizes before estimating.

## 2026-11-18: Bronze layer

**What I did**
- Loaded the features file with the header turned off and named all 167 columns.
- Loaded the classes and edgelist files with the header turned on.
- Added two tracking columns to each table: `_ingested_at` (load time) and `_source_file` (file name).
- Saved the three tables as `bronze.elliptic_txs_features`, `bronze.elliptic_txs_classes` and `bronze.elliptic_txs_edgelist`.

**What went wrong and how I fixed it**
- The first load cell contained only a file name, so Python raised a NameError. I replaced it with a proper read statement.
- The count cell raised TABLE_OR_VIEW_NOT_FOUND because the save step had not run yet. I ran the save cell and the tables appeared.
- The GitHub pull was blocked by a local `.gitignore` change. The same lines were already on GitHub, so I discarded the local change in the Web Terminal and pulled again.
- The Web Terminal would not start because the compute was switched off. I started Default Interactive Compute.

**Result**
- Row counts: features 203,769, classes 203,769, edgelist 234,355. These match the dataset description.
- Committed `pipeline/01_bronze_ingest.ipynb` as "Bronze layer: load raw Elliptic data" (commit b6feb50) and pushed it to `main`.
- No data files were committed.

**AI use**
- I used an AI assistant for explanations and code suggestions during the bronze stage. This is declared in my AI Use Statement.

**Next**
- Silver layer: profile the raw data, clean it, and run data quality checks.
