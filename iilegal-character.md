# val_anagram st:1 Synt_err:1 error:Syntax Error : Illegal Character, How to Solve?

During the transaction validation process, I received illegal errors. Searching for the exact location via logs, I found nothing to indicate where the illegal character was, other than the information that this was a problem arising from the window validation process.

Seeing this, I tried to do the validation directly using the GESAWI function, which resulted in the same error.

 

After checking the error message, I looked for the WGOPIH source in the treatments editor and when validating “Illegal Character” was the message returned.

Note: WGOPIH.src is generated whenever the window is created.


In search of the illegal character, I commented the entire source and uncommented it in parts and validated, which worked, until when I uncommented the $DEF_BOITE block, “Illegal Character”, appeared again.

```
$DEF_BOITE
Gosub HORODAT From WGOPIH
If dim(A_WINDPREV)<=0 : Local Char A_WINDPREV(30) : Endif
If GWEBSERV=0
Local Inpbox "OPIH" From GFONCTION At A_STAMP With A_WINDPREV Mask [PIH0]
& Folder Mask [XQPIH1]
& , Folder Mask [PIH1]
& , Folder Mask [XQPIHTRA]
& , Folder Mask [PIH3]
& , Folder Mask [PIH4]
& , Folder Mask [CSTI]
& , Folder Mask [XQPIH2]
& Listbox [PIH_] GAU_CHE
& [L]SAIRAP(1) = [F:XXQPIH]NUMNFE Using "K:9#" Titled TITSEL(1) ,
& [L]SAIRAP(2) = [F:PIH]NUM Using "K:20X" Titled TITSEL(2) ,
& [L]SAIRAP(3) = [F:PIH]BPRNAM Using "K:45X" Titled TITSEL(3) ,
& [L]SAIRAP(4) = [F:PIH]AMTNOT Using "Nz3:13.4#" Titled TITSEL(4) ,
& [L]SAIRAP(5) = [F:PIH]ACCDAT Using "" Titled TITSEL(5) ,
& [L]SAIRAP(6) = [F:PIH]FCY Using "K:5c" Titled TITSEL(6) ,
& [L]SAIRAP(7) = [F:PIH]BPR Using "K:15c" Titled TITSEL(7) ,
& [L]SAIRAP(8) = Using "" Titled TITSEL(8) ## <------------------------------------ (01)
& Listbox [PIH9] GAU_CHE9
& [L]SAIRAP(1) = [F:PIH9]A_TAB1 Using "K:9#" Titled TITSEL9(1)
....
...
..
.
Return
```

It was then that I realized that in the identifier line (01), there was no assignment for [L]SAIRAP(8), which generated the error.


To test, assign any value and the validation worked.
After encountering this problem, I realized that this section represented the window object and when navigating through the object's fields in the GESAOB function, I noticed a line that was created, but everything was blank.


After removing it, everything worked perfectly.

I end here with my observations
- When you get this type of error, check the GESAWI, GESAMK and GESAOB functions if there are blank lines registered, this is probably the problem
- If you don't find the problem in the functions above, find the source (.src) created for the problem window and look for assignments without values. I did this by commenting my entire source and uncommenting it one by one, but you don't need to do that, you can simply use control 'F' and search for “= Using” or “=Using”.