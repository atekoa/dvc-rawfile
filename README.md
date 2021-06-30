# dvc-rawfile
*We have compressed it because it occupies about 60MB, but the test is against the unzipped file (214089_JAI.raw)*

The RAW file inside the tar.gz has a md5sum

fd0de1350b92b00d60afd53b015f6aea  214089_JAI.raw

But DVC calculates it as 
- md5: 0b4d86bc06ee3260e8172b2196805382
  size: 63232000
  path: 214089_JAI.raw
  
This happens because it considers it a text file and performs a dos2unix replacement. So 

https://github.com/iterative/dvc/blob/1.11/dvc/utils/__init__.py#L39
-> https://github.com/iterative/dvc/blob/1.11/dvc/istextfile.py#L34

It still happens in version 2.4.3
https://github.com/iterative/dvc/blob/2.4.3/dvc/utils/__init__.py#L37
-> https://github.com/iterative/dvc/blob/2.4.3/dvc/istextfile.py#L22

When uploading it through the gocloud.dev library, it fails because of the MD5 check, since the one calculated by DVC and the real one of the file is not the same
https://github.com/google/go-cloud/blob/v0.23.0/blob/blob.go#L328
