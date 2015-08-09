# excel-processing
Processing excel files for ERP data analysis

Input files will be .csv or .xlsx data, representing trigger and timing information from an EEG experiment (.BDF files from Actiview/Biosemi ActiveTwo EEG recording environment, stripped of their event file information, originally saved as .evt but converted to .csv for the purposes of data cleaning)

There will be 4 columns of data: "Tmu" "Code" "TriNo" and "Comnt" (1-4)

Tmu refers to time in microseconds (microseconds/1,000,000 = seconds) and will be used for most calculations

Code is either a value of "1" or "41". Rows with a code of "41" need to be read/saved by the program

TriNo and Comnt are the numerical codes and a re-hash in string format with "Trig." added in front of the number

We need to discover which groups of onsets correspond with the playing of an audio file followed by a finite subject response time window.

The first two rows in the .csv need to be saved/read into the new file (headings and the time point of 0)

From then on, only two types of rows need to be saved/read into a new file:

1) rows where the value in "Code" (column 2) is 41
2) rows that mark the onset of a stimulus

Stimuli are approximately 4 seconds in length (vary from 3.9 seconds to 4.1 seconds), and the inter-stimulus/inter-trial interval is always exactly 4 seconds. 

Because some (but not all) responses are recorded in the event file log, there will not always be a gap of 4 seconds between the last onset in a sound file and the first onset of a new sound file.

Onsets of a sound file are generally indicated by a timing difference of 

The start of the data file will be the start of the first audio file.  In order to find the end of playback, we need to seek until we find two onsets with a gap of approximately 4 seconds.  once we find that end point, take all of those onsets and capture them to a new file.  repeat this process for the rest of the input.

if 41 codes are found, they indicate the unpausing of the experiment, and in this case we must find the next onset, as that represents the start of audio file playback
