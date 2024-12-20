# moab-data
Moab data project for Mike

**Author:** Paul Boal

**Update:** 12/19/2024 8:45 PM CT


## Introduction
The purpose of this project is to aggregate readings from a sampling device that records location local project coordinates (US survey feet) into a three-level grid system. Examples of this grid system are available in the PDF files.

## Usage
Review the [moab.ipynb] Python notebook

This is a Python Jupyter notebook. You can run it using [Anaconda Python](https://www.anaconda.com/download) in the provided Jupyter server or using the Jupyter interface in your favorite IDE (e.g., Microsoft Visual Studio Code).  Required libraries are provided in the requirements.txt file.

To change which files you are processing, put the input file in this directory and then change the input and output file names in section 2 of the [moab.ipynb].

## Process Steps
1. Read in the raw data file with y, x, and reading in csv format
2. Use our convert_xy() function to map the inputcoordinates in the Tile, Survey Unit, and Subcell location
3. Compute arithmetic means of the underly Subcell-level means at the Survey Unit level (average of averages to minimize inappropriate weighting based on uneven sampling across the Subcell)
4. Compute the arithmetic mean of the entire Survey Unit
6. Combine all the results into an Excel file for review

## Results File

### 1. Glossary
Explains the other three tabs in the worksheet

### 2. Points
The raw input readings along with the mapped Tile, Survey Unit, and Subcell information

### 3. Subcells
An aggregation of readings at the Subcell level
* Tile - Tile location (e.g., AA, AB, AC, BA, etc)
* SU - Survey Unit
* Subcell - Subcell number
* reading_mean - The arithmetic mean of all readings in that Subcell
* reading_count - The number of readings in this Subcell.

### 4. SurveyUnits
An aggregation of readings at the Survey Unit level
* Tile - Tile location (e.g., AA, AB, AC, BA, etc)
* SU - Survey Unit
* subcell_mean - The arithmetic mean of the arithmetic mean from all the Subcells in that Survey Unit (i.e., an average of averages)
* subcell_count - Number of Subcells in this Survey Unit that had readings
* reading_count - Total number of readings in this Survey Unit as a sum of readings in all the Subcells in that Survey Unit
* point_mean - The overage arithmetic mean of all readings in that Survey Unit
* point_count - Total number of readings in that Survey Unit used in the point_mean
* point_diff - Percent difference between the overall mean reading in this Survey Unit and the subcell mean (i.e., the difference between the overall average and the average of averages)

Our main goal here is to understand the difference between using the two averaging methods. 

Primarily, we're concerned with the difference between taking an overall average (point_mean) and an average of the average from the Subcells (subcell_mean).

If you sort the SurveyUnits sheet by point_diff, you'll find there are a few Survey Units where the deviation is quite signficant.
* LH-104 has a difference of 50% between the overall average (1.697) and the average of the Subcell averages (3.054)
  * When you drill down into this further, you'll see there are 81 readings in Subcell LH-104-77, which is the likely reason for the difference
  * When using an average of averages method, the data from LH-104-77 only counts as one data point
  * When using the overall average method, the data from LH-104-77 counts as 81 data points, a much higher weight
