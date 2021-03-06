Project Summary

This assignment uses data from the UC Irvine Machine Learning Repository, a popular repository for machine learning datasets. In particular, we will be using the “Individual household electric power consumption Data Set” which I have made available on the course web site:

Dataset: Electric power consumption [20Mb]
Description: Measurements of electric power consumption in one household with a one-minute sampling rate over a period of almost 4 years. Different electrical quantities and some sub-metering values are available.
The following descriptions of the 9 variables in the dataset are taken from the UCI web site:

Date: Date in format dd/mm/yyyy
Time: time in format hh:mm:ss
Global_active_power: household global minute-averaged active power (in kilowatt)
Global_reactive_power: household global minute-averaged reactive power (in kilowatt)
Voltage: minute-averaged voltage (in volt)
Global_intensity: household global minute-averaged current intensity (in ampere)
Sub_metering_1: energy sub-metering No. 1 (in watt-hour of active energy). It corresponds to the kitchen, containing mainly a dishwasher, an oven and a microwave (hot plates are not electric but gas powered).
Sub_metering_2: energy sub-metering No. 2 (in watt-hour of active energy). It corresponds to the laundry room, containing a washing-machine, a tumble-drier, a refrigerator and a light.
Sub_metering_3: energy sub-metering No. 3 (in watt-hour of active energy). It corresponds to an electric water-heater and an air-conditioner.

Review Criteria

Was a valid GitHub URL containing a git repository submitted?
Does the GitHub repository contain at least one commit beyond the original fork?
Please examine the plot files in the GitHub repository. Do the plot files appear to be of the correct graphics file format?
Does each plot appear correct?
Does each set of R code appear to create the reference plot?
Reviewing the Assignments

 
Loading Data:

The dataset has 2,075,259 rows and 9 columns. First calculate a rough estimate of how much memory the dataset will require in memory before reading into R. Make sure your computer has enough memory (most modern computers should be fine).
We will only be using data from the dates 2007-02-01 and 2007-02-02. One alternative is to read the data from just those dates rather than reading in the entire dataset and subsetting to those dates.
You may find it useful to convert the Date and Time variables to Date/Time classes in R using the strptime()  and as.Date() functions.
Note that in this dataset missing values are coded as ?.

Making Plots


For each plot you should

Construct the plot and save it to a PNG file with a width of 480 pixels and a height of 480 pixels.
Name each of the plot files as plot1.png, plot2.png, etc.
Create a separate R code file (plot1.R, plot2.R, etc.) that constructs the corresponding plot, i.e. code in plot1.R constructs the plot1.png plot. Your code file should include code for reading the data so that the plot can be fully reproduced. You must also include the code that creates the PNG file.
Add the PNG file and R code file to the top-level folder of your git repository (no need for separate sub-folders)
When you are finished with the assignment, push your git repository to GitHub so that the GitHub version of your repository is up to date. There should be four PNG files and four R code files, a total of eight files in the top-level folder of the repo.

Code

####
###Exploratory Data Analysis Course Project 1
setwd('C:/Users/denfordm/Desktop/Coursera/EDA')
if(!file.exists('data')) dir.create('data')
fileUrl <- 'https://d396qusza40orc.cloudfront.net/exdata%2Fdata%2Fhousehold_power_consumption.zip'
download.file(fileUrl, destfile = './data/household_power_consumption.zip')
unzip('./data/household_power_consumption.zip', exdir = './data')

### read data into R
files <- file('./data/household_power_consumption.txt')
data <- read.table(text = grep("^[1,2]/2/2007",readLines(files),value=TRUE), sep = ';', col.names = c("Date", "Time", "Global_active_power", "Global_reactive_power", "Voltage", "Global_intensity", "Sub_metering_1", "Sub_metering_2", "Sub_metering_3"), na.strings = '?')

### Plot 1

if(!file.exists('figures')) dir.create('figures')
png(filename = './figures/plot1.png', width = 480, height = 480, units='px')
### plot figure
with(data, hist(Global_active_power, xlab = 'Global Active Power (kilowatt)', main = 'Global Active Power', col = 'red'))
### close device
dev.off()

### Plot 2
### data and time converted to specific format
data$Date <- as.Date(data$Date, format = '%d/%m/%Y')
data$DateTime <- as.POSIXct(paste(data$Date, data$Time))
### open device

png(filename = './figures/plot2.png', width = 480, height = 480, units='px')
### plot figure

plot(data$DateTime, data$Global_active_power, xlab = '', ylab = 'Global Active Power (kilowatt)', type = 'l')
### close device
dev.off()

### Plot 3 open device

png(filename = './figures/plot3.png', width = 480, height = 480, units='px')
### plot figure

plot(data$DateTime, data$Sub_metering_1, xlab = '', ylab = 'Energy sub metering', type = 'l')
lines(data$DateTime, data$Sub_metering_2, col = 'red')
lines(data$DateTime, data$Sub_metering_3, col = 'blue')
legend('topright', col = c('black', 'red', 'blue'), legend = c('Sub_metering_1', 'Sub_metering_2', 'Sub_metering_3'), lwd = 1)

dev.off()

### Plot 4 
png(filename = './figures/plot4.png', width = 480, height = 480, units='px')
### plot figure

par(mfrow = c(2, 2))
plot(data$DateTime, data$Global_active_power, xlab = '', ylab = 'Global Active Power (kilowatt)', type = 'l')
plot(data$DateTime, data$Voltage, xlab = 'datetime', ylab = 'Voltage', type = 'l')
plot(data$DateTime, data$Sub_metering_1, xlab = '', ylab = 'Energy sub metering', type = 'l')
lines(data$DateTime, data$Sub_metering_2, col = 'red')
lines(data$DateTime, data$Sub_metering_3, col = 'blue')
legend('topright', col = c('black', 'red', 'blue'), legend = c('Sub_metering_1', 'Sub_metering_2', 'Sub_metering_3'), lwd = 1)
plot(data$DateTime, data$Global_reactive_power, xlab = 'datetime', ylab = 'Global_reactive_power', type = 'l')

dev.off()