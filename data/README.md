# Data directories

Don't ever edit your raw data. Especially not manually. And especially not in Excel.

The most important features of a quality data analysis are correctness and reproducibility —a nyone should be able to re-run your analysis using only your code and raw data and produce the same final products.

These directories are setup for this desire.

- Raw data is kept in a cloud service or other server and downloaded via the script `../modules/get_data.sh`
- When you clone this repo, the first thing you need to do is run the get_data script!
- You never manipulate the raw files in `00-raw/`
- Any pre-processing or wrangling of the data is done by scripts or programs you write; they load content from `00-raw/` and write outputs into `01-interim/` or `02-processed/`
- Data may be pre-processed and wrangled in steps.  Anything other than the final product is stored in (and later reloaded from) `01-interim/`. Final products of pre-processing/wrangling go in `02-processed`
- Analysis and modeling steps will load the data only from `02-processed`

## Opinions and suggestions

Raw data must be treated as immutable — it's okay to read and copy raw data to manipulate it into new outputs, but never okay to change it in place. 

Some dos and don'ts: 

- ✅ Do write code that moves the raw data through a pipeline to your final analysis.
- ✅ Do serialize or cache the intermediate outputs of long-running steps.
- ✅ Do make it possible (and ideally, documented and automated) for anyone to reproduce your final data products with only the code in {{ cookiecutter.module_name }} and the data in data/raw/ (and data/external/).
- ⛔ Don't ever edit your raw data, especially not manually, and especially not in Excel. This includes changing file formats or fixing errors that might break a tool that's trying to read your data file.
- ⛔ Don't overwrite your raw data with a newly processed or cleaned version.
- ⛔ Don't save multiple versions of the raw data.

Data should (mostly) not be kept in source control. A consequence of treating data as immutable is that data doesn't need source control in the same way that code does. Therefore, by default, the data/ folder is included in the .gitignore file. If you have a small amount of data that rarely changes, you may want to include the data in the repository. GitHub currently warns you if files are over 50MB and rejects any files over 100MB.

If you have larger amounts of data, consider storing and syncing with a cloud service like Amazon S3, Azure Blob Storage, or Google Cloud Storage. We've had a good experience with Amazon S3, if you're not tied to any particular cloud provider. Syncing tools can help you manage the data. Some examples:

- Amazon S3: awscli, s3cmd, s5cmd, geesefs
- Azure Blob Storage: azcopy
- Google Cloud Platform: gcloud
- Supports multiple clouds: cloudpathlib, fsspec

There is also the Git Large File Storage (LFS) extension which lets you track large files in git but stores the files on a separate server. GitHub provides some storage compatible with Git LFS.

