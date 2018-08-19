meta
====

Todo States
-----------

### default states

-   unmarked
-   TODO
-   DONE

### Adding a New TODO<span class="tag" data-tag-name="hotkey"></span>

-   S-M &lt;RET&gt;

### changing Todo States<span class="tag" data-tag-name="hotkey"></span>

-   C-c C-t : rotate states (unmarked -&gt; TODO
    -&gt; DONE-&gt;unmarked-&gt;)
-   C-u C-c C-t : select keyword using completion

### Fast Access Todo States & Custom States<span class="tag" data-tag-name="hotkey"></span>

You can set up custom todo states and define special letters to use in
concert with C-c C-t to set them

``` {.commonlisp}
(setq org-todo-keywords
           '((sequence "TODO(t)" "|" "DONE(d)")
             (sequence "REPORT(r)" "BUG(b)" "KNOWNCAUSE(k)" "|" "FIXED(f)")
             (sequence "|" "CANCELED(c)")))
```

### Setting up Multiple State Sequences

Avoid any duplicate states

``` {.commonlisp}
 (setq org-todo-keywords
           '((sequence "TODO" "|" "DONE")
             (sequence "REPORT" "BUG" "KNOWNCAUSE" "|" "FIXED")
             (sequence "|" "CANCELED")))
```

#### changing between sequences

-   C-S-&lt;right&gt; : shift to next sequence
-   C-S-&lt;left&gt; : shift to previous sequence
-   S-&lt;right&gt; : walk through all keys from all sequences
-   S-&lt;left&gt; : walk back through all keys from all sequences

priority cookies
----------------

### defaults

-   3 defaults: A B C
-   shows up as \[\#A\]
-   used to sort

### setting priority<span class="tag" data-tag-name="hotkey"></span>

-   hotkey : C-c ,
    -   prompted for value (A,B&lt;,C) or SPC
    -   &lt;SPC&gt; removes the cookie

### change priority<span class="tag" data-tag-name="hotkey"></span>

-   S-&lt;up&gt; : shift priority up
-   S-&lt;down&gt; : shift priority down

Hotkeys
-------

### Visibility

  key                   description
  --------------------- ----------------------------------
  &lt;tab&gt;           visiblility cycling
  S-&lt;tab&gt;         visibility cycling for whole doc
  C-c C-r               reveal context
  C-u C-u &lt;tab&gt;   Restore startup visibility

### Motion

  key            description
  -------------- ----------------------------------
  C-c C-n        next heading
  C-c C-p        previous heading
  C-c C-u        one level up
  C-c C-j        Jump (goto)
  C-c C-f        Forward same level
  C-c C-b        Backward same level
  M-x org-goto   search within an org mode buffer

### Create and Move Elements

  key     description
  ------- --------------------------------
  C-RET   new heading below
  M-RET   new line matching current line

### Time Stamps

  key              description
  ---------------- ------------------------------------------------
  C-c C-x          start timer
  C-c C-x -        insert list item with time
  M-&lt;RET&gt;    insert heading with time (doesnt seem to work)
  `C-u C-x `       toggle timer pause
  `C-u C-c C-x `   stop timer
  C-c C-x          set countdown

### Sparse Trees

  key           description                                               
  ------------- --------------------------------------------------------- --
  C-c /         Filter sparse trees                                       
  C-c / r       Filter using regex                                        
  M-g n         goto next match                                           
  M-g o         goto previous match                                       
  C-c / m       create a sparse tree with all matching entries            
  C-c           same as above                                             
  C-u C-c / m   same as above. Ignore non-TODO headlines                  
  C-c a m       create a global list of tag/property matches              
  C-c a M       Create global list of tag matches from all agenda files   
  C-c / p       Create sparse tree based on the value of a property       

### Lists and Checklists

  key               description
  ----------------- -------------------------------------------
  M-&lt;ret&gt;     new list item
  M-S-&lt;ret&gt;   create checkbox
  M-&lt;arrow&gt;   Move item
  C-c -             cycle item type OR turn into list item
  M-s-&lt;ret&gt;   new item iwth a checkbox
  C-c C-\*          checkboxes become TODOs
  C-c C-C           toggle checkboxes between \[x\] and \[ \]
  C-u C-u C-c C-C   toggle checkboxes between \[ \] and \[-\]

### Taking notes during a meeting

  key              description
  ---------------- ---------------------------------------------------------
  C-c C-x          (re)start timer
  C-c C-x -        insert description list item iwth current relative time
  M-&lt;ret&gt;    same as above
  `C-c C-x `       pause / continue
  `C-u Cuc C-x `   stop timer

### TODO Items & states

  key               description                  
  ----------------- ---------------------------- --
  C-c C-t           Rotate TODO state            
  C-c / t           spare tree with TODOs        
  C-c a t           global TODO list in agenda   
  C-S-&lt;ret&gt;   new TODO heading             
  S-&lt;up&gt;      increase priority            
  S-&lt;down&gt;    decrease priority            

Todo agendas may have per file keywords set \#+TODO: TODO(T) FEEDBACK(F)
| DONE(d!) CANCELED (c!@)

-   ! timestamp
-   @ add note

### Tags

  key       description
  --------- ------------------------------------
  C-c C-q   set tags
  C-c C-c   set tags if cursor is on a heading
  C-c / m   search for tags in sparse tree
  C-c a m   global list of tag matching
  C-c a M   Same but check only TODO items

-   search syntax
    -   +boss+urgent AND
    -   boss|urgent OR
    -   +boss+urgent-project conbination of tags
    -   `work+TODO="WAITING"|home+TODO="WAINTING"` Waitingtasks both at
        work and home

### Tables

  key               description
  ----------------- -----------------------------------------------
  C-c C-c           update table
  &lt;tab&gt;       move to next field
  &lt;ret&gt;       next row
  M-&lt;arrow&gt;   move rows / columns
  C-c -             insert horizontal bar below
  C-c               convert region into table or insert new table
  C-c \^            sort lines (in region)
  C-c }             visualize rows/columns
  C-c ’\~           edit formula in separate buffer

-   commands
    -   org-table-import : import data from CSV
    -   org-table-export : export data to CSV

### Column View

  key           description            
  ------------- ---------------------- --
  C-c C-x C-c   activate column view   
  n/p           next/previous vlaue    
  q             quit column view       
  a             edit allowed values    
  C-c C-x p     set property           

### Capture, refile, archive

  key             description
  --------------- -------------------------------------
  C-c c           capture
  C-c C-w         refile
  C-c C-x C-a     archive
  C-u C-u C-c c   goto last capture
  C-c C-x a       toggle ARCHIVE tag
  C-u C-c C-x q   check direct children for archiving

### Dates & Time

  key           description                         
  ------------- ----------------------------------- --
  C-c .         insert active date and day          
  C-u C-c .     insert active date and time         
  C-c !         insert inactive date                
  C-u C-c !     insert inactive date and time       
  C-c &lt;      insert today                        
  C-c C-o       open agneda for current timestamp   
  C-c S-d       insert DEADLINE                     
  C-c C-s       insert SCHEDULED                    
  C-c C-c C-d   remove DEADLINE                     
  C-c C-c C-s   remov SCHEDULED                     
  C-c / d       sparse tree with deadlines          
  C-c C-x c     clone event with timeshift          
  C-c C-y       return time range between dates     

todos
=====

TODO \[\#A\] set up AKKABA teragen review with commercials
----------------------------------------------------------

TODO \[\#A\] requirements doc for material publish
--------------------------------------------------

TODO \[\#A\] requirements doc for layoutpipeline
------------------------------------------------

TODO \[\#A\] requirements doc for deferred
------------------------------------------

TODO \[\#A\] create job description for pm role in technology
-------------------------------------------------------------

TODO \[\#A\] influx, graphite, elasticsearch 2.0 - which metrics<span class="tag" data-tag-name="question"></span>
------------------------------------------------------------------------------------------------------------------

TODO \[\#B\] dd-netsys.d2.com - graphs just dont make sense. need to move it to grafana<span class="tag" data-tag-name="systems"></span>
----------------------------------------------------------------------------------------------------------------------------------------

TODO \[\#A\] talk with andy about moving from bugzilla to shotgun<span class="tag" data-tag-name="andy"></span><span class="tag" data-tag-name="dan"></span><span class="tag" data-tag-name="brian"></span>
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

TODO \[\#B\] openssh problems here. Old version in cent6.4
----------------------------------------------------------

TODO \[\#A\] get newest version of git.
---------------------------------------

TODO add bcbunker@d2.om to daily shotgun & bugzilla summary cron
----------------------------------------------------------------

TODO package up craig’s crons and deploy under cronmaster
---------------------------------------------------------

TODO look at Ran’s document set requirements
--------------------------------------------

TODO evaluate whether we need our flavor of OIIO ( BLake has task to build 1.7 )
--------------------------------------------------------------------------------

TODO talk with andy about getting Ran a 27 inch monitor. Or… let him bring one in from home.
--------------------------------------------------------------------------------------------

TODO create an shotgunadmin mailing list with me, brandon, and JW
-----------------------------------------------------------------

TODO create a shotgun council alias producers,dpms,(dfx,department sups)
------------------------------------------------------------------------

done
====

DONE \[\#A\] set up time to talk with dean and rafe about SHIRT
---------------------------------------------------------------

DONE \[\#A\] talk with Andy Pavell about newer rvio licenses<span class="tag" data-tag-name="systems"></span>
-------------------------------------------------------------------------------------------------------------

DONE \[\#B\] dd-web-02.d2.com does not show up in ganglia - talk with andy<span class="tag" data-tag-name="systems"></span>
---------------------------------------------------------------------------------------------------------------------------

DONE come up with initial plan for training
-------------------------------------------

DONE \[\#C\] tx vs tex
----------------------

updates
=======

DONE SYSTEMS: c7000 finally got invoices. financing cutting a check
-------------------------------------------------------------------

DONE SYSTEMS: thompson putting additional memory into shotgun.
--------------------------------------------------------------

TODO SHOTGUN: shotgun downtime soon. Shotgun to repair data duplication
-----------------------------------------------------------------------

Tickets
=======

[support for roles at facility level](sg:30292)
-----------------------------------------------
