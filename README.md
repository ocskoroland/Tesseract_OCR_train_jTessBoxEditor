# Tesseract_OCR_train_jTessBoxEditor
Cant train my ocr

Followed tutorial: https://www.youtube.com/watch?v=1v8BPw0Dn0I
I followed this steps:

Step 1: Make box files for images that we want to train
Syntax: tesseract [langname].[fontname].[expN].[file-extension] [langname].[fontname].[expN] batch.nochop makebox
Eg:tesseract train.my.exp0.tif train.my.exp0 batch.nochop makebox

{*Note: After making box files we have to change or modify wrongly identified characters in box files.}

Step 2: Create .tr file (Compounding image file and box file)
Syntax: tesseract [langname].[fontname].[expN].[file-extension] [langname].[fontname].[expN] box.train
Eg: tesseract train.my.exp.tif train.my.exp0 box.train

step 3: Extract the charset from the box files (Output for this command is unicharset file)
Syntax: unicharset_extractor [langname].[fontname].[expN].box 
Eg: unicharset_extractor train.my.exp0.box

step 4: Create a font_properties file based on our needs.
Syntax: echo "[fontname] [italic (0 or 1)] [bold (0 or 1)] [monospace (0 or 1)] [serif (0 or 1)] [fraktur (0 or 1)]" [angle bracket should be here] font_properties 
Eg: echo "arial 0 0 1 0 0" [angled bracket] font_properties

Step 5: Training the data.
Syntax: mftraining -F font_properties -U unicharset -O [langname].unicharset [langname].[fontname].[expN].tr
Eg: mftraining -F font_properties -U unicharset -O train.unicharset train.my.exp0.tr

Step 6:
Syntax: cntraining [langname].[fontname].[expN].tr
Eg: cntraining train.my.exp0.tr
{*Note:After step 5 and step 6 four files were created.(shapetable,inttemp,pffmtable,normproto) }

Step 7: Rename four files (shapetable,inttemp,pffmtable,normproto) into ([langname].shapetable,[langname].inttemp,[langname].pffmtable,[langname].normproto)
Syntax: rename filename1 filename2
Eg:
    rename shapetable train.shapetable
    rename inttemp train.inttemp
    rename pffmtable train.pffmtable
    rename normproto train.normproto

Step 8: Create .traineddata file
Syntax: combine_tessdata [langname].
Eg: combine_tessdata train.

Move .traineddata file to tesseract programs tessdata directory
C:\Program Files\Tesseract-OCR\tessdata

Run tesseract for trained fronts

tesseract Test2.png stdout -l train
