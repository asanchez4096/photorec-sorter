# photorec-sorter

A tool to sort/organize files recovered by the [PhotoRec](https://www.cgsecurity.org/wiki/PhotoRec) tool

## Description

PhotoRec does a great job when recovering deleted files. But the result is a huge, unsorted, unnamed amount of files. Especially for external hard drives serving as backup of all the personal data, sorting them is an endless job.

This program helps you sorting your files. It does the following steps:
1. Files are copied to folders for each file type.
2. Using exif data, `.jpg` files are distinguished by the year (and optionally by month) captured, and by the event

We define an "event" as a time span during them photos are taken. It has a delta of 4 days without a photo to another event. If no date from the past can be detected, these jpgs are put into one folder to be sorted manually.

## Installation

Install the required packages:

```bash
pip install -r requirements.txt
```


## Run the sorter

Then run the sorter:

`python recovery.py <path to files recovered by PhotoRec> <destination>`

This copies the recovered files to their file type folder in the destination directory. The recovered files are not modified. If a file already exists in the destination directory, it is skipped. This means that the program can be interrupted with Ctrl+C and then continued at a later point by running it again.

The first output of the program is the number of files to copy. To count them might take some minutes depending on the amount of recovered files. Afterwards, you get some feedback on the processed files.

### Parameters

For an overview of all arguments, run with the `-h` option: `python recovery.py -h`.

#### Max numbers of files per folder

All directories contain a maximum of 500 files by default. If there are more for a file type, numbered subdirectories are created. If you want another file-limit, e.g. 1000, pass that number as the third parameter when running the program:

`python recovery.py <path to files recovered by PhotoRec> <destination> -n1000`

#### Folder for each month

By default, `photorec-sorter` sorts your photos by year:

```
destination
|- 2015
    |- 1.jpg
    |- 2.jpg
    |- ...
|- 2016
    |- ...
```

Sometimes you might want to sort each year by month:

`python recovery.py <path to files recovered by PhotoRec> <destination> -m`

Now you get:

```
destination
|- 2015
    |- 1
      |- 1.jpg
      |- 2.jpg
    |- 2
      |- 3.jpg
      |- 4.jpg
    |- ...
|- 2016
    |- ...
```

#### Keep original filenames

Use the -k parameter to keep the original filenames:

`python recovery.py <path to files recovered by PhotoRec> <destination> -k`

#### Adjust event distance

For the case you want to reduce or increase the timespan between events, simply use the parameter -d. The default is 4:

```python recovery.py <path to files recovered by PhotoRec> <destination> -d10```

#### Rename jpg-files with ```<Date>_<Time>``` from EXIF data if possible

If the original jpg image files were named by ```<Date>_<Time>``` it might be useful to rename the recovered files in the same way. This can be done by adding the parameter -j.

```python recovery.py <path to files recovered by PhotoRec> <destination> -j```

If no EXIF data can be retrieved the original filename is kept.

In case there are two or more files with the same EXIF data the filename is extended by an index to avoid overwritng files.

The result will look like:
```
20210121_134407.jpg
20210122_145205.jpg
20210122_145205(1).jpg
20210122_145205(2).jpg
20210122_145813.jpg
20210122_153155.jpg
```
