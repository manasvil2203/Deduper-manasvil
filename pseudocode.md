## **Goal** 
PCR duplicates are a problem because they inflate coverage and lead to overestimation. This can lead to problems in our downstream analysis. So in order to avoid this, after we align the sequence(so we have the coordinates and do not collapse the wrong molecule), we deduplicate our sam file. So in order to do that, we need to make sure that they have the same chromosome number, same 5' start position, same strand and same UMI. If they are and the UMIs are part of valid UMI list, then we collapse it into a single molecule.

## **Pseudocode**
### High level functions
def determine_strand(flag:int) ->string 
'''This function is taking in the flag and giving us the strand'''        
Determine the strand using the the flag
    if ((flag & 16) == 16):
        Strand = negative
    else 
        Strand = positive

return strand
 Input:
      flag=16

    Output:
      strand = negative


def compute_five_prime(flag: int, pos: int, cigar: str) -> int:
    """
    This function figures out where the first base of the read (in the read’s
    own 5′ to 3′ direction) maps on the reference genome. It accounts for the strand
    orientation, how far the alignment extends along the reference, and whether part of
    the read was soft-clipped off the start or end.
    """ 
     Take into account soft clipping 
        s_left = length of leading soft clip, so if CIGAR starts with S
        s_right = length of trailing soft clip, so if CIGAR end with S


    Compute 5' position 
        if strand is positve
            5' = position from coloum 4 - s_left
        else
            Compute reference consuming length from cigar:
            ref_length = sum(lengths of CIGAR leading numbers in {M, X, D, N})
            5' = position from coloumn 4 + ref_len - 1 + s_right

    returns five_prime (int)

    Input:
      flag=16, pos=1000, cigar="90M10S"

    Output:
      1099
      (strand='-', ref_len=90, right aligned edge = 1000+90-1=1089, S_right=10 → five_prime=1089+10=1099)





Make a set of valid UMI

Initialize temp variable(current_chrom) to store current chromosome number
Initialize temp variable(current_pos) to store the current 5 prime position
Initialze empty set for keeping track of (unique strand, UMI pairs, 5 prime)

Open an output file:
    For every line 
        If starting with @ in input sam file:
            Write into output
            continue
        Grab the line
        Strip and split with colom
        grab last element
        Then I would validate this against my set
            if it is not in my validation set:
                continue

                chrom = Grab the chromosome number from col 3
                flag =  Grab from coloumn 2
                strand = determine_strand(flag: int)
                pos = Grab coloumn left most start position 
                cigar = Grab coloumn 6 to determine 5' position and ref length
                five_prime = compute_five_prime(flag: int, pos: int, cigar: str)
        

    
if chrom is not equal to current_chrom:
    Clear out the set
    current_chrom will be now set to chromosome
    current_pos will now be set to compute_five_prime()


Now for the core of the logic

mini_key = Create a variable to store a tuple of the 5', strand, UMI

if this mini_key is not in my set:
    write the whole line to my output file
    And add the mini_key into my set
else:
    Keep going

