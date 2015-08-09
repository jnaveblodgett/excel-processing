# excel-processing
Processing excel files for ERP data analysis

Input files will be EEG data.

We need to discover which groups of onsets correspond with the playing of an audio file followed by a finite subject response time window.

The start of the data file will be the start of the first audio file.  In order to find the end of playback, we need to seek until we find two onsets with a gap of approximately 4 seconds.  once we find that end point, take all of those onsets and capture them to a new file.  repeat this process for the rest of the input.

if 41 codes are found, they indicate the unpausing of the experiment, and in this case we must find the next onset, as that represents the start of audio file playback