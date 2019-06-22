# utl-create-a-list-of-male-students-at-achme-high-school-using_a_hash
Create a list of male students at achme high school usin a hash 
    Create a list of male students at achme high school usin a hash                                                    
                                                                                                                       
    Very elegant HASH solution                                                                                         
                                                                                                                       
        Two Solutions                                                                                                  
                                                                                                                       
             1. HASH (no sort needed - With todays inexpensive large memory, HASH is ascendant)                        
             2. Sort nodupkey                                                                                          
                                                                                                                       
    Posted soution does not seem to work.                                                                              
                                                                                                                       
    github                                                                                                             
    https://tinyurl.com/yxkge5x2                                                                                       
    https://github.com/rogerjdeangelis/utl-create-a-list-of-male-students-at-achme-high-school-using_a_hash            
                                                                                                                       
    SAS Forum                                                                                                          
    https://tinyurl.com/yxtxyzva                                                                                       
    https://communities.sas.com/t5/SAS-Programming/how-to-derive-only-n-flag-records/m-p/568109                        
                                                                                                                       
    data have ;                                                                                                        
      input subject $ name $ gender $ ;                                                                                
    cards4 ;                                                                                                           
    Music Marge F                                                                                                      
    Math John M                                                                                                        
    Art Tom M                                                                                                          
    Math George M                                                                                                      
    Music Mary F                                                                                                       
    Art Mary F                                                                                                         
    Music Tom M                                                                                                        
    Math Jerry M                                                                                                       
    Science Mica F                                                                                                     
    Science John M                                                                                                     
    ;;;;                                                                                                               
    run;quit;                                                                                                          
                                                                                                                       
    /* Sort is not needed in hash solution                                                                             
       Used to documemt the prblem */                                                                                  
                                                                                                                       
    proc sort data=have out=havSrt;                                                                                    
    by name;                                                                                                           
    run;quit;                                                                                                          
                                                                                                                       
    /*                                                                                                                 
    Up to 40 obs WORK.HAVSRT total obs=10                                                                              
                                                                                                                       
                                   |  RULES                                                                            
                                   |  =====                                                                            
      SUBJECT    NAME      GENDER  |                                                                                   
                                   |                                                                                   
      Math       George      M     |  Y  One Male student                                                              
                                   |                                                                                   
      Math       Jerry       M     |  Y  One Male student                                                              
                                   |                                                                                   
      Math       John        M     |  Y  Select just one                                                               
      Science    John        M     |                                                                                   
                                   |                                                                                   
      Music      Marge       F     |                                                                                   
                                   |                                                                                   
      Music      Mary        F     |                                                                                   
      Art        Mary        F     |                                                                                   
                                   |                                                                                   
      Science    Mica        F     |                                                                                   
                                   |                                                                                   
      Art        Tom         M     |  Y  Select just one                                                               
      Music      Tom         M     |                                                                                   
                                                                                                                       
    */                                                                                                                 
                                                                                                                       
    *            _               _                                                                                     
      ___  _   _| |_ _ __  _   _| |_                                                                                   
     / _ \| | | | __| '_ \| | | | __|                                                                                  
    | (_) | |_| | |_| |_) | |_| | |_                                                                                   
     \___/ \__,_|\__| .__/ \__,_|\__|                                                                                  
                    |_|                                                                                                
    ;                                                                                                                  
                                                                                                                       
    Up to 40 obs from WANT total obs=4                                                                                 
                                                                                                                       
    Obs    NAME      GENDER                                                                                            
                                                                                                                       
     1     John        M                                                                                               
     2     George      M                                                                                               
     3     Jerry       M                                                                                               
     4     Tom         M                                                                                               
                                                                                                                       
    *                                                                                                                  
     _ __  _ __ ___   ___ ___  ___ ___                                                                                 
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                                                
    | |_) | | | (_) | (_|  __/\__ \__ \                                                                                
    | .__/|_|  \___/ \___\___||___/___/                                                                                
    |_|                                                                                                                
    ;                                                                                                                  
                                                                                                                       
    HASH                                                                                                               
    ====                                                                                                               
                                                                                                                       
    data _null_;                                                                                                       
       if _N_=1 then do;                                                                                               
         if 0 then set have;                                                                                           
         declare hash test(dataset:'have(where=(gender="M"))',duplicate:'r');                                          
         test.definekey('name');                                                                                       
         test.definedata( 'name', 'gender');                                                                           
         test.definedone();                                                                                            
    end;                                                                                                               
    test.output(dataset:'want');                                                                                       
    stop;                                                                                                              
    run;quit;                                                                                                          
                                                                                                                       
                                                                                                                       
    SORT                                                                                                               
    ====                                                                                                               
                                                                                                                       
    proc sort data=have out=want(where=(gender="M") drop=subject) nodupkey;                                            
      by name;                                                                                                         
    run;quit;                                                                                                          
                                                                                                                       
                                                                                                                       
                                                                                                                       
