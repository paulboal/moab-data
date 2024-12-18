# moab-data
Moab data project for Mike

**Author:** Paul Boal

**Date:** 12/18/2024 1:25 AM CT


## Introduction
The purpose of this project is to aggregate readings from a sampling device that records location in SPC coordinates using a three-level grid system. Examples of this grid system are available in the PDF files.

## Usage
Review the [moab.ipynb] Python notebook

## Process Steps
1. Read in the raw data file with y, x, and reading in csv format
2. Use our convert_xy() function to map the (y,x) SPC coordinates in the Survey Unit, Cell, and Subcell location
3. Compute arithmetic and geometric means at the Subcell level
4. Compute arithmetic means of the underly Subcell-level means at the Cell level (average of averages to minimize inappropriate weighting based on uneven sampling across the Subcell)
5. Compute the arithmetic mean of the entire Cell
6. Combine all the results into an Excel file for review

## Results File

### 1. Glossary
Explains the other three tabs in the worksheet

### 2. Points
The raw input readings along with the mapped Survey Unit, Cell, and Subcell information

### 3. Subcells
An aggregation of readings at the Subcell level
* SU - Survey Unit
* Cell - Cell number
* Subcell - Subcell number
* reading_mean - The arithmetic mean of all readings in that Subcell
* reading_gmean - The geometric mean of all readings in that Subcell. Note that if any readings are 0 or negative, the geometric mean will be blank.
* reading_count - The number of readings in this Subcell.
* diff - The percent difference between the geometric mean and arithmetic mean. Note that this will be blank if the geomtric mean could not be calculated.

### 4. Cells
An aggregation of readings at the Cell level
* SU - Survey Unit
* Cell - Cell number
* subcell_gmean - The arithmetic mean of the geometric mean from all the Subcells in that Cell
* subcell_mean - The arithmetic mean of the arithmetic mean from all the Subcells in that Cell (i.e., an average of averages)
* subcell_count - Number of Subcells in this Cell that had readings
* reading_count - Total number of readings in this Cell as a sum of readings in all the Subcells in that Cell
* gmean_diff - Percent difference between the subcell_gmean and subcell_mean
* point_mean - The overage arithmetic mean of all readings in that Cell
* point_count - Total number of readings in that Cell used in the point_mean
* point_diff - Percent difference between the overall mean reading in this cell and the subcell mean (i.e., the difference between the overall average and the average of averages)

Our main goal here is to understand the difference between using the three averaging methods. 

Primarily, we're concerned with the difference between taking an overall average (point_mean) and an average of the average from the Subcells (subcell_mean).

If you sort the Cells sheet by point_diff, you'll find there are a few cells where the deviation is quite signficant.
* LH-104 has a difference of 50% between the overall average (1.697) and the average of the Subcell averages (3.054)
  * When you drill down into this further, you'll see there are 81 readings in Subcell LH-104-77, which is the likely reason for the difference
  * When using an average of averages method, the data from LH-104-77 only counts as one data point
  * When using the overall average method, the data from LH-104-77 counts as 81 data points, a much higher weight
