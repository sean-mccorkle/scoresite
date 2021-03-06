## scoresite README


Scoresite searches FASTA formatted DNA sequences for regulator sites defined by
a case matrix. Case matrices consist of a template and the probability that a 
given base will be found at that position.

For every substring of the input sequence of the same length as the template, 
scoresite:
1. generates both a forward and reverse complement sequence to test against the
suppliced template/matrix, and for each: 
 
2. calculates the number of mismatches from the template.

3. calculates the score by adding together the probabilities of the actual
bases found and dividing that sum by the length of the template.

4. For both the forward and reverse complement sequences, if the number of 
mismatches is less that the maximum mismatches specified, AND/OR the score is 
greater than the minimum score specified, it prints out the "hit." The choice
between AND and OR depends on what is specified in the config file for that
regulator, or if it is overridden on the command line. 

The "hit" output format is:
regulator name:         As specified in the config file, e.g. "re1-jizu".
actual sequence:        Sequence of hit from file.
score:                  Between 0.0 and 1.0.
mismatches:             Mismatches between sequence and template.
position:               Position of "leftmost" base on *either* strand.
strand:                 Which strand (plus or minus) the hit was on.
FASTA header:           The header of the input sequence the hit was in.


## NOTES

Calculation of mismatches and scores are de-coupled. There is no validation of 
the case matrix read from the config file, i.e. you could create completely 
inconsistent matrices and scoresite will use them. The useful aspect of this 
behavior involves the use of IUPAC ambiguity codes in the template, where you 
can define what to consider a mismatch, but still encode preferences in the
case matrix probabilities. E.g., you could specify a K in the template (which 
encodes G or T) and specify different score preferences:  0.60/0.40 : G/T. One 
could even use K and yet specify a set of partially inconsistent probabilities:
0.40/0.40/0.20: G/T/C.


