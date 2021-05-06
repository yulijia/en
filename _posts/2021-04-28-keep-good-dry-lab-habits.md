---
published: ture
layout: post
title: "Keep good dry lab habits"
author: Yu
categories: bioinfo
tags:
- Rstudio
- tmux
- Ctrl-z
---


I have been worked in multiple dry labs during the last 10 years. Each laboratory has its own style, but few of them training students with good dry lab habits. I learned a lot of good habits from Dr. Malay Basu's lab. Now, I would like to share these good habits and tips when doing bioinformatics study in a dry lab. All the suggestions based on the Linux environment.

## Habtis 

#### No.1

Maintain a good documentary for all analysis. I learned this since the first week at Malay's lab. We recommand follow the style of Rmarkdown reprot from [MD Anderson Cancer Center](https://bioinformatics.mdanderson.org/Supplements/ResidualDisease/Reports/)

The report should includes:

1. Executive Summary (\*)
  - Introduction
  - Data and Methods
  - Results
2. Data Mungling
3. Analysis
  - Step 1
  - Step 2
4. Appendix (\*)
  - Working dir
  - Script descriptions
  - List of Figures
  - List of Tables


If you want to share the report with others, it is better to generate a HTML report, if you want to see the report as a MD file on Github, you need to generate a Github MD report.

Always use `sessionInfo()` to print the collect information about the current R session in the bottom of the report.


#### No.2

Each research project should also follow a clean structure:

- bin
- original_data
    * README - containing where the data is from
    * Data in compressed form.
        1. If the raw data is huge. Then only the process data. The the processed step should be captured in the "exp" directory.
- "exp" - Actual experiments. Each directory must be named as
  1. YYYYMMDD-some_identifiable_info-#githubissue-INITIALS
  2. each directory should link input files in `in` folder and generate outputfiles in `out` folder. The outfiles should have the name date and time appended to it.
  3. The entire experiments should be done through and RMD file or single "run.sh" or a "Makefile". There must be the RMD/MD file in each directory. You must write the executable summary and the list of figures and tables and their descriptions in the report.
- "design" (?)
  * Reserved for PI.
- docs
  * Reserved for PI.
- reports
  1. "index.htm": A top level index files listing every file and few lines description of the files in the directory.
  2. A list of files names matching with the each directory names of "exp", preferebly in html format generated from RMDs
  
  
You can upload the project structure and code to any cloud storage, make a backup every day/week. 


#### No.3

Whatever, back up the server data every week, lost data is a huge pain for every researcher.

#### No.4

After a project is finished. You need to review the whole project's data and make sure every folder in the project has a document to record what you had done before, every script should also have a note to record the meaning of it.

#### No.5

when link a data from one sub-folder to another sub-folder in one project folder, you should use relative symbolic link `ln -rs`. When you need to move a project folder to other place, the data structure will not be broken.

## Tips 

#### No.1

Use `tmux` or `screen` when you want to run some program after exit the terminal session, they are usable with interactive commands. I perfer using `tmux` , because it has more function compare with `screen`. Please try to avoid to use `nohup` which you couldn't control the program later. 

When you have a Rstudio-server, sometime you may wish to run program via Rstudio termianl after you close the Rstudio interface, but I still suggest you run the program using `tmux` inside the Rstudio terminal.

The main reason for this is when the Rstudio terminal session is not reponse, you still can do interactive in tmux from other ssh session.

#### No.2

Always monitor the memory usage via `ps aux --sort -rss | head`, some parallel program may cause momory leak, you maynot know about memory leak even the program is running sucessfully. 

#### No.3

`Control+C` aborts the application almost immediately while `Control+Z` shunts it into the background, suspended. If you shunt any application into the background, **please keep in mind that the program is still waiting for your command**.

#### No.4

Do not save the large files to `/home` directory if your server has limited storage for `/home`. Use `df -h` to check the size of each system disk.

Use `du -sh * | sort -rh` to check the size of each folder.

#### No.5

Better to learn one of [Pipeline Building Framework](https://www.biostars.org/p/91301/) and Docker, it makes everything reproducible more easily.

#### No.6

Bioinformatics is a field that has rapid changes. Stay hungry, keep learning.
