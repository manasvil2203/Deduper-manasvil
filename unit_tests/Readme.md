**Test 1:**
Testing to see if a valid UMI will be written to temp(keep)(Lines 1 and 2)

**Test 2:**
Testing to see if invalid UMI is discarded(Lines 3 and 4)

**Test 3**
Same chromosome and 5 prime, and same strand(+) and UMI One keep and one dup(Lines 5 and 6)

**Test 4**
Same chromosome and 5' on +starnd different UMI...keep both(Lines 7 and 8 )

**Test 5**
Same chromosome and 5' but different strand(adjusted start position accordingly)...keep both(Lines 9 and 10 )

**Test 6**
Same chromosome. Start position the same but due to leading soft clipping 5' should be different...keep both (Lines 11 and 12) 

**Test 7**
Same chromosome. Same start position but due to trailing soft clipping5' should be different.(Lines 13 and 14)

**Test 8**
Chromosome change...memory reset(Lines 15 and 16)