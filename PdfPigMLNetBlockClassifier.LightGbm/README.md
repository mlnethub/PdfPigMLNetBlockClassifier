# Results
Results are based on PubLayNet's validation dataset, where the page segmentation is known. For real life use, a page segmenter will be needed (see PdfPig's [PageSegmenters](https://github.com/UglyToad/PdfPig/tree/master/src/UglyToad.PdfPig.DocumentLayoutAnalysis/PageSegmenter)). The quality of the page segmentation  will impact the results.
## Metrics for multi-class classification model
```
MicroAccuracy = 0.9369, a value between 0 and 1, the closer to 1, the better
MacroAccuracy = 0.7482, a value between 0 and 1, the closer to 1, the better
LogLoss = 0.2092, the closer to 0, the better

LogLoss for class 0 (title)        = 0.2156, the closer to 0, the better
LogLoss for class 1 (list)         = 3.1245, the closer to 0, the better
LogLoss for class 2 (table)        = 0.3060, the closer to 0, the better
LogLoss for class 3 (text)         = 0.1094, the closer to 0, the better
LogLoss for class 4 (image)        = 0.2472, the closer to 0, the better

F1 Score for class 0 (title)       = 0.9003, a value between 0 and 1, the closer to 1, the better
F1 Score for class 1 (list)        = 0.0213, a value between 0 and 1, the closer to 1, the better
F1 Score for class 2 (table)       = 0.9361, a value between 0 and 1, the closer to 1, the better
F1 Score for class 3 (text)        = 0.9593, a value between 0 and 1, the closer to 1, the better
F1 Score for class 4 (image)       = 0.9161, a value between 0 and 1, the closer to 1, the better
```

## Confusion table
```
          ||=======================================================
PREDICTED ||     0 |     1 |     2 |     3 |     4 | Total | Recall
TRUTH     ||=======================================================
(title) 0 || 1,765 |     0 |     0 |   145 |     2 | 1,912 | 0.9231
(list)  1 ||     1 |     3 |     2 |   273 |     0 | 279   | 0.0108
(table) 2 ||     0 |     0 |   623 |    63 |     5 | 691   | 0.9016
(text)  3 ||   242 |     0 |    13 | 8,709 |     1 | 8,965 | 0.9714
(image) 4 ||     1 |     0 |     2 |     2 |    71 | 76    | 0.9342
          ||=======================================================
Precision ||0.8785 |1.0000 |0.9734 |0.9475 |0.8987 |
```

## Permutation Feature Importance
PFI works by taking a labeled dataset, choosing a feature, and permuting the values for that
feature across all the examples, so that each example now has a random value for the feature
and the original values for all other features. The evaluation metric (e.g. micro-accuracy) is
then calculated for this modified dataset, and the change in the evaluation metric from the
original dataset is computed. The larger the change in the evaluation metric, the more
important the feature is to the model. PFI works by performing this permutation analysis
across all the features of a model, one after another. - [Source]( https://docs.microsoft.com/en-us/dotnet/api/microsoft.ml.permutationfeatureimportanceextensions.permutationfeatureimportance?view=ml-dotnet#Microsoft_ML_PermutationFeatureImportanceExtensions_PermutationFeatureImportance__1_Microsoft_ML_MulticlassClassificationCatalog_Microsoft_ML_ISingleFeaturePredictionTransformer___0__Microsoft_ML_IDataView_System_String_System_Boolean_System_Nullable_System_Int32__System_Int32_)

### Micro Accuracy

|Feature                  | Description | Change in MicroAccuracy | 95% Confidence in the Mean Change in MicroAccuracy |
|------------------------:|:-----------:|:-----------------------:|:--------------------------------------------------:|
|**charsCount**           |Characters count|-0.2192 |0.0008443 |
|**pctNumericChars**      |% of numeric characters|-0.04996 |0.0004363 |
|**deltaToHeight**        |Average delta to average page glyph height|-0.04155 |0.000428|
|**pctBulletChars**       |% of bullet characters|-0.01571 |0.0004034|
|**pctAlphabeticalChars** |% of alphabetical characters|-0.012 |0.0003245 |
|**pctSymbolicChars**     |% of symbolic characters|-0.01187 |0.0004204 |
|**pathsCount**           |Paths count|-0.01089 |0.0002144 |
|**pctHorPaths**          |% of horizontal paths|-0.002695 |0.0001318 |
|**imageAvgProportion**   |Average area covered by images|-0.001895 |0.00003909 |
|**pctVertPaths**         |% of vertical paths|-0.001403 |0.0001069 |
|**pctOblPaths**          |% of oblique paths|-0.0005032 |0.00003612 |
|**pctBezierPaths**       |% of Bezier curve paths|-0.00007828 |0.00001563 |
|**imagesCount**          |Images count|-0.00002796 |0.00002768 |

### Macro Accuracy 

|Feature                  | Description | Change in MacroAccuracy | 95% Confidence in the Mean Change in MacroAccuracy |
|------------------------:|:-----------:|:-----------------------:|:--------------------------------------------------:|
|**charsCount**           |Characters count|-0.1906 |0.001906|
|**pathsCount**           |Paths count|-0.07355 |0.001211|
|**pctNumericChars**      |% of numeric characters|-0.06516 |0.0008356|
|**deltaToHeight**        |Average delta to average page glyph height|-0.05476 |0.001333|
|**pctOblPaths**          |% of oblique paths|-0.01097 |0.0002006|
|**pctAlphabeticalChars** |% of alphabetical characters|-0.009334 |0.001237|
|**imageAvgProportion**   |Average area covered by images|-0.00874 |0.0001771|
|**pctVertPaths**         |% of vertical paths|-0.005001 |0.0003568|
|**pctHorPaths**          |% of horizontal paths|-0.004718 |0.0004244|
|**pctSymbolicChars**     |% of symbolic characters|-0.001522 |0.0008552|
|**pctBulletChars**       |% of bullet characters|0.0009088 |0.0007747|
|**pctBezierPaths**       |% of Bezier curve paths|-0.0003737 |0.00005705|
|**imagesCount**          |Images count|-0.00003805 |0.00003434|

# TO DO
## Features
- Add a [decoration](https://github.com/UglyToad/PdfPig/blob/master/src/UglyToad.PdfPig.DocumentLayoutAnalysis/DecorationTextBlockClassifier.cs) score/flag
- Add block's number of line, [cf.](http://www.cs.rug.nl/~aiellom/publications/ijdar.pdf)
- Add block's aspect ratio: the ratio between width and height of the bounding box, [cf.](http://www.cs.rug.nl/~aiellom/publications/ijdar.pdf)
- Add block's area ratio: the ratio between block area and the page area, [cf.](http://www.cs.rug.nl/~aiellom/publications/ijdar.pdf)
- Add block's font style: an enumerated type, with possible values: regular, bold, italic, underline, [cf.](http://www.cs.rug.nl/~aiellom/publications/ijdar.pdf)
- Use [bookmarks](https://github.com/UglyToad/PdfPig/blob/master/src/UglyToad.PdfPig/Outline/BookmarksProvider.cs) when available with minimum edit distance score
- Add % sparse lines in a block, for better table recognition [cf.](https://clgiles.ist.psu.edu/pubs/CIKM2008-table-boundaries.pdf)
- Font color distance from most common color
