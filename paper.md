---
title: ''
tags:
  - Python
  - introduction
  - molecular dynamics
  - computational chemistry
authors:
  - name: Ashley Ringer McDonald
    orcid: 
    affiliation: "1, 2" # (Multiple affiliations must be quoted)
  - name: Jessica A. Nash
    orcid: 0000-0003-1967-5094
    affiliation: 1
affiliations:
 - name: California Polytechnic State University
   index: 1
 - name: The Molecular Sciences Software Institute
   index: 2
date: 20 November 2019
bibliography: paper.bib
---

## Summary

Computational molecular science is a field which encompasses a number of disciplines including computational chemistry, physics, materials science, and chemical engineering. In order for students to successfully contribute to this field, they must not only build understanding of theoretical aspects of their research, they must also have relevant computer skills including use of the terminal and scripting and programming. We have prepared this module focusing on introducing students to Python programming using Anaconda and the Jupyter notebook, with applications relevant to computational molecular sciences.  The workshop teaches practical skills that enable a student to immediately participate in meaningful research. The materials can be presented in a live 1-2 day workshop, or students can work through the lessons online, at their own pace.  The target audience for the materials are undergraduate or other early career students who have limited prior programming experience, and are designed to be accessible to those with no programming experience.   

The workshop begins by introducing basic python programming syntax and control structures using chemically motivated examples.  It discusses techniques for extracting data from files and working with multiple data files, using electronic structure output files as an example.  Analyzing tabular data, using the `NumPy` library, and plotting using the `Matplotlib` library are introduced using molecular dynamics data.

Central to the design of this workshop is the assignment of project. In an in-person workshop, this is given as a homework assignment to the students to attempt between the first and second day. For this project, students must apply the skills which have been covered in the first part of the workshop to parse an `xyz` file (a common file format in computational molecular science giving the cartesian coordinates of atoms) and measure the distances between atoms.  

After these basics, the workshop introduces the concept of functions and writing function documentation.  The students refactor their geometry analysis code with functions, and learn how to create a command line script. An introduction to unit testing, and collaboration with git and GitHub is presented.  All concepts presented in this workshop slowly build on one another to help students master basic concepts.

This workshop was developed by the Molecular Sciences Software Institute (MolSSI), as part of its mission to enhance software, education, and training in the computational molecular sciences.  MolSSI works to improve software practices in CMS, and this workshop gives a light introduction to several topics we advocate as best practices (version control, testing, and documentation). MolSSI's educational mission builds on and uses strategies from the work of Software Carpentry and Data Carpentry.  

## Statement of Need
Within chemistry, and many other fields related to CMS, the amount of structured programming training students receive as part of their undergraduate education varies widely. Students may have a stand-alone programming course or no instruction at all.  Further, most chemists, even computational chemists who use programming regularly in their research, are rarely trained on teaching programming and often lack resources to provide students this type of training.  Thus, most programming training for chemists is ad-hoc, through interactions with mentors and research advisors, or self-taught by learning what they need to know to solve problems in research.  The Molecular Sciences Software Institute aims to address this problem by providing resources, training, and workshops to teach students best practices in software development.  This will enhance the entire discipline of computational molecular science by making code more stable, reproducible, and maintainable.  

## Learning Objectives and Contents

### Python Basics for Computational Molecular Science

- **Introduction** This lesson introduces basic python programming syntax and control structures like assigning variables, arrays, for loops, and logic.  Data types and recasting are discussed.  

- **File Parsing and Multiple File Parsing** These two lessons focus on extracting data from complex text-based output files.  Students learn to read in information from text files and output information to text files.  Print formatting is discussed.

- **Working with Tabular Data** This lesson introduces the `NumPy` library.  Students learn to read in data from comma separated value (csv) files and organize data in `NumPy` arrays. A project is given at the end of this lesson where the students must apply concepts from the introduction and file parsing lessons.

- **Plotting and Data Visualization** This lesson introduces the `Matplotlib` library.  Students learn to create, modify, and save plots.

- **Writing Functions** - This lesson introduces functions, and why they are useful in programming. Students practice by reformatting their geometry analysis project to contain functions.

- **Running code from the Linux Command Line** - This lessons covers moving from the Jupyter notebook to a text editor and running a python script from the command line. Students are taught how to take user input from the command line using the `argparse` module.

- **Testing Code with pytest** - This lesson gives a very light introduction and unit testing using the `pytest` framework.

- **Version Control and Sharing Code** - This lesson gives an overview of version control using git and hosting code on GitHub.

## Sample Teaching Schedules

## Future Work

## Acknowledgements
This work was supported by The Molecular Sciences Software Institute under NSF grant ACI-1547580.

## References
