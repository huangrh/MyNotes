# https://stackoverflow.com/questions/2793389/scheduling-r-script  
- Open the scheduler: START -> All Programs -> Accesories -> System Tools -> Scheduler  
- Create a new Task  
- under tab Action, create a new action    
  - choose Start Program  and browse to Rscript.exe which should be placed e.g. here:  "C:\Program Files\R\R-3.0.2\bin\x64\Rscript.exe"  
  - input the name of your file in the parameters field  
  - input the path where the script is to be found in the Start in field  
- go to the Triggers tab  and create new trigger  and choose that task should be done each day, month, ... repeated several times, or whatever you like 
- Set up R

```
Sys.getenv("RSTUDIO_PANDOC")
Sys.setenv(RSTUDIO_PANDOC="--- insert directory here ---")
rmarkdown::render(input = input_file, output_dir = output_dir, output_file = output_file)
```

# How to set up New SSD Drive on Windows. 
- Use disk management
-  https://www.alphr.com/install-second-ssd/
  -  use GPT
  -  Right-click and choose “New Simple Volume,” then “Next.”
# windows rsync: 
- https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/robocopy  
- https://stackoverflow.com/questions/36407166/synchronize-windows-folders  
- robocopy <source> <destination> /mir /copyall  

# https://winscp.net/eng/download.php 

# Software Distribution and Building Platform for Windows
- https://www.msys2.org/

# https://www.geeksforgeeks.org/microsoft-azure/  
