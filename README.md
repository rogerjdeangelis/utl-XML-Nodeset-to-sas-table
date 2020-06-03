# utl-XML-Nodeset-to-sas-table
XML Nodeset to sas table
    XML Nodeset to sas table                                                                        
                                                                                                    
    github                                                                                          
    https://tinyurl.com/y8b4fs8m                                                                    
    https://github.com/rogerjdeangelis/utl-XML-Nodeset-to-sas-table                                 
                                                                                                    
    StackOverflow                                                                                   
    https://stackoverflow.com/questions/62053421/xml-nodeset-to-r-dataframe                         
                                                                                                    
    related repos                                                                                   
    https://github.com/rogerjdeangelis?tab=repositories&q=xml&type=&language=                       
                                                                                                    
    *_                   _                                                                          
    (_)_ __  _ __  _   _| |_                                                                        
    | | '_ \| '_ \| | | | __|                                                                       
    | | | | | |_) | |_| | |_                                                                        
    |_|_| |_| .__/ \__,_|\__|                                                                       
            |_|                                                                                     
    ;                                                                                               
                                                                                                    
    filename ft15f001 "d:/xml/nodeset.xml";                                                         
    parmcards4;                                                                                     
    <diag>                                                                                          
      <name>A00</name>                                                                              
      <desc>Cholera</desc>                                                                          
      <diag>                                                                                        
        <name>A00.0</name>                                                                          
        <desc>Cholera due to Vibrio cholerae 01, biovar cholerae</desc>                             
        <inclusionTerm>                                                                             
          <note>Classical cholera</note>                                                            
        </inclusionTerm>                                                                            
      </diag>                                                                                       
      <diag>                                                                                        
        <name>A00.1</name>                                                                          
        <desc>Cholera due to Vibrio cholerae 01, biovar eltor</desc>                                
        <inclusionTerm>                                                                             
          <note>Cholera eltor</note>                                                                
        </inclusionTerm>                                                                            
      </diag>                                                                                       
      <diag>                                                                                        
        <name>A00.9</name>                                                                          
        <desc>Cholera, unspecified</desc>                                                           
      </diag>                                                                                       
    </diag>                                                                                         
    ;;;;                                                                                            
    run;quit;                                                                                       
                                                                                                    
    *            _               _                                                                  
      ___  _   _| |_ _ __  _   _| |_                                                                
     / _ \| | | | __| '_ \| | | | __|                                                               
    | (_) | |_| | |_| |_) | |_| | |_                                                                
     \___/ \__,_|\__| .__/ \__,_|\__|                                                               
                    |_|                                                                             
    ;                                                                                               
    Up to 40 obs from WANT total obs=10                                                             
                                                                                                    
    Obs    NAME                       VALUE                                                         
                                                                                                    
      1    name                       A00                                                           
      2    desc                       Cholera                                                       
      3    diag.name                  A00.0                                                         
      4    diag.desc                  Cholera due to Vibrio cholerae 01, biovar cholerae            
      5    diag.inclusionTerm.note    Classical cholera                                             
      6    diag.name                  A00.1                                                         
      7    diag.desc                  Cholera due to Vibrio cholerae 01, biovar eltor               
      8    diag.inclusionTerm.note    Cholera eltor                                                 
      9    diag.name                  A00.9                                                         
     10    diag.desc                  Cholera, unspecified                                          
                                                                                                    
    *                                                                                               
     _ __  _ __ ___   ___ ___  ___ ___                                                              
    | '_ \| '__/ _ \ / __/ _ \/ __/ __|                                                             
    | |_) | | | (_) | (_|  __/\__ \__ \                                                             
    | .__/|_|  \___/ \___\___||___/___/                                                             
    |_|                                                                                             
    ;                                                                                               
                                                                                                    
    proc datasets lib=work nolist;                                                                  
      delete want;                                                                                  
    run;quit;                                                                                       
                                                                                                    
    %utlfkil(d:/xpt/want.xpt);* just in case it exists and R fails;                                 
                                                                                                    
    %utl_submit_r64('                                                                               
    library(XML);                                                                                   
    library(plyr);                                                                                  
    library(SASxport);                                                                              
    library(data.table);                                                                            
    dat  <- XML::xmlParse("d:/xml/nodeset.xml");                                                    
    want <- unlist(XML::xmlToList(dat), use.names = TRUE, recursive = TRUE);                        
    want <- lapply(want, function(x) if(is.factor(x)) as.character(x) else x);                      
    want <- ldply(want, as.data.frame);                                                             
    want <- as.data.table(lapply(want, function(x) if(is.factor(x)) as.character(x) else x));       
    colnames(want)<-c("name","value");                                                              
    write.xport(want,file="d:/xpt/want.xpt");                                                       
    ');                                                                                             
                                                                                                    
    libname xpt xport "d:/xpt/want.xpt";                                                            
    data want ;                                                                                     
      set xpt.want;                                                                                 
    run;quit;                                                                                       
    libname xpt clear;                                                                              
                                                                                                    
