## Summary

This module focuses on teaching students basic python programming for computational molecular science.  The materials can be presented in a live 1-2 day workshop, or students can work through the materials online, at their own pace.   The target audience for the materials are undergraduate students, or other early career students, who have no prior programming experience.  The workshop focuses on practical skills that would enable a student to participate in meaningful research in computational molecular sciences.   The workshop begins by introducing basic python programming syntax and control structures using chemically motivated examples.  It discusses techniques for extracting data from files and working with multiple data files, using electronic structure output files as an example.  Examples of analyzing tabular data and using the numpy library are given using molecular dynamics data, which is also used to discuss plotting and data visualization.   This section concludes with a geometry analysis project, where students must parse an xyz file (a common file format in computational molecular science) and measure the distances between atoms.  

After these basics, the workshop introduces the concept of functions and writing function documentation.  The students refactor their geometry analysis code with functions, and then write unit tests for these functions.  Finally, a lesson about code sharing and collaboration with git and GitHub is presented.  

This workshop was developed by the Molecular Sciences Software Institute (MolSSI), as part of its mission to enhance software, education, and training in the computational molecular sciences.  (I feel like we should work somethign in here about best practices and how this workshop introduces a few of them.)  It builds on the work of Software Carpentry and Data Carpentry.  

## Statement of Needs
Within chemistry, the amount of structured programming training students receive as part of their undergraduate education varies widely, ranging from a stand-alone programming course to no instruction at all.  Further, most chemists, even computational chemists who use programming regularly in their research, are rarely trained on teaching programming and often lack resources to provide students this type of training.  Thus, most programming training for chemists is ad-hoc, through interactions with mentors and research advisors, or self-taught by learning what they need to know to solve problems in research.  The Molecular Sciences Software Institute aims to address this problem by providing resources, training, and workshops to teach students best practices in software development.  This will enhance the entire discipline of computational molecular science by making code more stable, reproducible, and maintainable.  

## Learning Objectives and Contents

### Python Basics for Computational Molecular Science

- **Introduction** This lesson introduces basic python programming syntax and control structures like assigning variables, arrays, for loops, and logic.  Data types and recasting are discussed.  

- **File Parsing and Multiple File Parsing** These two lessons focus on extracting data from complex text-based output files.  Students learn to read in information from text files and output information to text files.  Print formatting is discussed.

- **Working with Tabular Data** This lesson introduces the numpy library.  Students learn to read in data from csv files and organize it in numpy arrays.  

- **Graphing and Data Visualization** This lesson introduces the matplotlib library.  Students learn to create, modify, and save plots.

## Future Work
