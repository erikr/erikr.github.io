---
title: Matlab style guide 
layout: post
tags: tech,startups, draft
---

People often write unreadable and disorganized code. This is especially true in Matlab, which is used by engineers and scientists who lack formal backgrounds in software development.

Using consistent and rational style guidelines will help you, colleagues, and any future users of your code.

Clean code is not just for aesthetics. You'll write faster and think more clearly.

Here is a Matlab style guide I've found helpful.

---

### Add spaces between arguments

+ Add a space before each argument.
+ Add spaces before and after `=`.

**Good**:

```
myCellArray(i, :) = featureValues;
[outputA, outputB] = myFunction(inputX, inputY);
```

**Bad**:

```
myCellArray(i,:)=featureValues;
[outputA,outputB]=myFunction(inputX,inputY);
```

---

### Use a neat and consistent style for comments

+ Add a space between the `%` and the comment text.
+ Capitalize the first letter of each comment sentence.
+ Break up long sentences with newlines.

**Good**:

```
% This is a comment.
% This is the second line of a comment.
% And also an example of a comment that goes on too long.
```

**Bad**:

```
%This is a less readable comment.
%this is the second line of a less readable comment. And also an example of a comment that goes on too long.
```

---

### Comment heavily

Comment each chunk of code that does something useful. Help the reader understand the broad conceptual approach.

```
% Load data from CSV file in Data folder into Matlab
data = loadDataFromCSV(pathToCSV);

% Clean data by removing patients with <50 data points
dataCleaned = cleanData(data);

% Extract features from cleaned data
features = extractFeatures(dataCleaned);

% Initialize arrays to store outputs
idxPatientsToRemove = [];
serumCreatinine = [];
nMedRecords = [];
```


---

### Indent your code

In Python, indentation dictates logic, so you can't be sloppy.

Unfortunately, Matlab allows awful indentation.

Poorly indented code is hard to follow:

```
for i=1:10
a = i*rand(1e3);
for j = 5:20
if j > 10 && a < 50
b(i, j) = a*j;
else
b(i, j) = NaN;
end
end
end
```

Using tabs makes nested logic more understandable:

```
for i=1:10
    a = i*rand(1e3);
    for j = 5:20
        if j > 10 && a < 50
            b(i, j) = a*j;
        else
            b(i, j) = NaN;
        end
    end
end
```

---

### Add space around operators when performing math

This line is easy to read:

```
a = b * c + (i .^ j) / (500 - alpha);
```

This line is harder to read:

```
a=b*c+(i.^j)/(500-alpha);
```

You can also use newlines and indentations to improve readibility of long expressions, especially with fractions:

```
a = ((b + c) .^ beta) / ...
    (b - x * pi);
```

---

### Use a consistent and structured header

The header at the top of your `.m` file should explain the:
+ Purpose and broad approach of the function
+ Inputs
+ Outputs
+ Dependencies

A short example of how to call the function is helpful too.

We use [this matlab header](https://github.com/erikrtn/matlabMisc/blob/master/header.m):

```
% [outputVar] = functionName(inputVar)
% 
% Overview
%    Description of function goes here. 
%     
% Input
%    inputVar:
%
% Output
%    outputVar:
%
% Dependencies
%    https://github.com/...
%
% Reference(s)
% 
% Copyright (C) 2017 FIRSTNAME LASTNAME <email@address.com>
% All rights reserved.
%
% This software may be modified & distributed under the terms
% of the BSD license. See LICENSE file in repo for details.
```

---

### Write modular functions to organize code by task

This example `pipeline.m` script modularizes major tasks into functions, with clear and self-explanatory variable names, input arguments, and outputs:

```
function pipeline(pathToCSV)

data = loadDataFromCSV(pathToCSV);
dataCleaned = cleanData(data);
features = extractFeatures(dataCleaned);
[trainingData, testData, trainingLabels, testLabels] = ...
    createCrossValSets(features, labels);
trainWeights = learnWeights(trainingData, trainingLabels);
yHat = predictOutputs(trainWeights, testData);
classifierPerf = assessPerformance(yHat, testLabels);

end
```

Optimal modularity strikes a balance between depth and breadth.

Too many or too few sub-functions are hard to navigate or delegate.

Don't write the entire codebase of a project in a single file.

---

### Use mixed case for variable, function, and file names

Also called "camel capitalization", this is standard practice for C++.

The first letter is lowercase, and new words are capitalized:

```
activityVariance
sepsisScore
nValidScores
```
Underscores are also popular for variable names, i.e. `activity_variance`, and works fine too. However, the TeX interpreter in Matlab will read undescores as a switch to subscript. This can be problematic if you label parts of a plot using text derived from variable names.

---

### Name variables and functions by meaning or use

Descriptive variable or function names help people understand how your code works.

Even the person who wrote the code forgets how it works if they step away from it for a few days. So make your code easy to understand.

Functions should be *verbs* because functions perform *actions* on objects.

Objects should be *nouns*.

**Good**:

```
featuresMatrix = convertFeatStructToMatrix(featuresStruct);
betas = estimateBetas(featuresMatrix, labels);
```

**Bad**:

```
m = featuresmatrix(x);
b = betascalc(m, y);
```

---

### Prefix variables representing number of items with *n*

```
nFiles
nSegments
```

---

### Suffix variables representing a single entity with *no*

```
patientIdNo
hospitalNo
```

### Prefix iterator variables with the counter, i.e. *i*

```
for iPatient = 1:nPatients
    patientIdNo = patientData(iPatient).id;
end
```

---

### Do not use spaces for folder or file names

Matlab and other languages do not handle spaces well.

Also, spaces must be escaped in Unix. If you want to navigate into a directory named `project results` and view a file named `beta test values.csv`, you have to type:

```
cd project\ results
less beta\ test\ values.csv
```

This is annoying. Use mixed case or underscores instead:

```
cd projectResults
less betaTestValues.csv
```

This simple bash script ([replaceSpaces.sh](https://github.com/erikrtn/bashscripts/blob/master/replacespaces.sh)) replaces spaces with underscores for all files in the current working directory.

---

For a more detailed style guide, I recommend *MATLAB Programming Style Guidelines* [(link to PDF)](www.datatool.com/downloads/matlab_style_guidelines.pdf) by Richard Johnson.
