# excel-processing
Processing excel files for ERP data analysis

Input files will be .csv or .xlsx data, representing trigger and timing information from an EEG experiment (.BDF files from Actiview/Biosemi ActiveTwo EEG recording environment, stripped of their event file information, originally saved as .evt but converted to .csv for the purposes of data cleaning)

There will be 4 columns of data: "Tmu" "Code" "TriNo" and "Comnt" (1-4)

Tmu refers to time in microseconds (microseconds/1,000,000 = seconds) and will be used for most calculations

Code is either a value of "1" or "41". Rows with a code of "41" **always** need to be read/saved by the program

TriNo and Comnt are the numerical codes and a re-hash in string format with "Trig." added in front of the number. they can be the numbers 16, 32, or 48. 
    For our purposes, the numbers are irrelevant. However, just for full clarification - 16 means that the right audio channel had an onset, 32 was the left audio channel onset, and 48 was a simultaneous audio onset (L&R channels)

We need to discover which groups of onsets correspond with the playing of an audio file followed by a finite subject response time window.

The first two rows in the .csv need to be saved/read into the new file (headings and the time point of 0)

From then on, only two types of rows need to be saved/read into a new file:

1) rows where the value in "Code" (column 2) is 41
2) rows that mark the onset of a stimulus
    This second point is far more complicated, as there is no magic cue that will always tell us which line is an onset of a trial and which line is not

Stimuli are approximately 4 seconds in length (vary from 3.9 seconds to 4.1 seconds), and the inter-stimulus/inter-trial interval is always exactly 4 seconds. 

Because some (but not all) responses are recorded in the event file log, there will not always be a gap of 4 seconds between the last onset in a sound file and the first onset of a new sound file.
    This has now been changed - there are now no responses recorded in the event file log. Participants are responding using the keyboard, not the response box, and the keyboard is no longer giving input to Actiview. Thus, keyboard responses will no longer be in the logfiles. 

Onsets of a sound file are generally indicated by a timing difference of >1.5 seconds since last onset
    However, now, we have an even better metric - if it's not a code 41 line, it's always going to be 4 seconds since the last onset. Just to make sure there aren't any goofs, though, we might want to set it to be something like >2 seconds (in case of some random error like a pause in the file - these happen occasionally, if an electrode is screwy and I have to pause the experiment to go in and find out what is going wrong with either the participant or the electrodes)

The start of the data file will be the start of the first audio file. (the first line with a code of "1" after the line with the code of "41")  After that, we're going to have to compare the time difference between onsets in each line to find the next start of an audio file. 
    Compare two lines at a time - maybe? ugh, how would we do this - regardless - line 2 "tmu" - line 1 "tmu". If result is > 2 seconds (or however many microseconds that would be), then line2 is the beginning of a stimulus presentation. Read line 2 to the new file. Repeat this process for the rest of the input; e.g., line3 - line2, line4 - line3, etc.; those results where the "tmu" answer is greater than the equivalent of 2 seconds get read/printed into a new file, or alternatively, deleted from the current file (whatever is easier? Probably best to print into a new file).
    We do want to also print all lines with a code of "41" into the new file, regardless of what the "tmu" difference is (that numerical column that represents timing in microseconds). That way our new file has basically two types of lines in it: the "unpause' lines (which will line up with the behavioral "start of block" lines) and the stimulus presentation onset lines (which will line up with the codes from the Presentation log files)
