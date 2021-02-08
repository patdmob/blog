---
layout: post
title: "Taking a .bat to scripts"
description: How to use a .bat file to run files on Windows.
categories: [blog, bat files, automation, Windows]
comments: true
---

Let's suppose that we need to periodically run the same scripts (written in R or Python perhaps), and furthermore we're using Windows. One of many possible solutions to this problem is to use a .bat file and then use a task scheduler to periodically run those scripts. 

But why use a .bat file? If you are automating scripts, then just run the script! In Windows, a .bat file has the advantage of being flexibly executed. I can double click on it, use the cmd prompt, or I can throw it into a task scheduler and it will run. This flexibility is especially useful if you inconsistently execute the script or have not-so-tech-savvy end users. 

But with this flexibility does come a challenge. How do .bat files pass absolute/relative paths to internal script calls? To solve this problem, I developed a quick test case to experiment. I wrote my test case in R but could have just as easily written it in Python. 

## Test case

My project directory contains the .bat file and separate folders for data and the R script. 

The directory looks like this: 

![](/images/Taking_a_bat_to_scripts_files/image002.png)

In the R script, I reference a file in the data folder. 

```r
test_data <- readRDS("./data/test_data.rds")

print(getwd())
print(test_data)

fileConn <- file("result.txt")
writeLines(getwd(), fileConn)
close(fileConn)
```

This code will use a relative path to retrieve data from a separate folder and expects the root path "./" to be the project directory. 

I'm not an expert at writing .bat files so I wrote a couple different scenarios to see what would happen. Eventually, I came up with the following code with comments. To the batch file (.bat) experts out there, please let me know if there is a better way. 


```bat
@echo off
REM This is a test to determine the working path 
REM when running RScript in BAT
REM %CD% gives the path the script was called from
REM %~dp0 will give the path of the script itself

echo Called path:
echo %CD%
echo.
echo Script path:
echo %~dp0
echo.

pause

echo Works only if you run it from the .bat's directory:
Rscript "%~dp0\R\bat_test.R"

echo.
echo Works even if you run it from another directory:
cd %~dp0
Rscript "R\bat_test.R"
echo.

echo Press enter to close . . . 
pause >nul

exit
```

Here were my results:

![](/images/Taking_a_bat_to_scripts_files/image001.png)

As you can see, I called the .bat file from my user profile directory. The first attempt fails but the second is a success. By using this second method, a user can flexibly execute the .bat file by double clicking, navigating from the cmd prompt, or using the task scheduler. 

