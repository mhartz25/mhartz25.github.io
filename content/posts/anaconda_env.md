---
title: "Setting up a PyTorch Environment"
date: "2023-07-11"
menu: "main"
description: "some best practices I've found for using Anaconda with ML"
---


It's no secret that setting up a development environment for Machine Learning is a nightmare. There are a lot of tools that try to abstract some of that complexity away, but it's easy to still run into problems(1). 

My tool of choice has been Anaconda. Is it the best? I have no idea, but it's what I've become most comfortable with(2). I've banged my head against the wall enough times that I have finally found some success with it. So -- because I’ve learned more than I would ever like to know about managing a python environment with Anaconda -- I decided to write down some of that hard-won knowledge here.


### Define Environments Using YAML

Defining a YAML file at the start of the project with all the dependencies you think you’ll need and using that to create a new conda environment has been the most useful thing I learned. This method helps to resolve all dependencies and conflicts in one fell swoop. **I have an example file at the bottom of this page**

A few things to be aware of, some packages are not supported by Conda, so these will need to be explicitly defined as a Pip install. There are other workarounds to get conda to build packages, but I have not tried these in the YAML step. The channels portion determines where in the environment packages are downloaded from, for the most part you can ignore this.

Command: `conda env create -f <file_name.yml> -n <env_name>`


Once you’ve registered your environment, use `conda install <package_name>` wherever possible. Sometimes you will have to use Pip which is fine. But be aware that once you do it breaks conda. Anaconda actually suggests to any do pip installs at the end, and that once you do that if you need to go back to using Conda install you should just recreate a new environment. 


### Create a New Conda Environment For Each Project

Creating a new conda environment for each project is worth it. An environment per project is probably overkill, but if you’re doing anything with PyTorch or TensorFlow, then I recommend it. 

If you're using Jupyter, you will want to register that environment, be sure to run the following command so that you're Notebook can access that environment: 
    
Command: `python -m ipykernel install --user --name <env_name> --display-name "<display_name>"`



### YAML File Example


```
name: torch
channels:
  - pytorch-nightly
  - conda-forge
dependencies:
    - python=3.9
    - pip>=19.0
    - pytorch 
    - torchvision 
    - jupyter
    - scikit-learn
    - scipy
    - pandas
    - matplotlib
    - pillow
    - tqdm
    - requests
    - h5py
    - pyyaml
    - boto3
    - ipykernel
    - beautifulsoup4
    - transformers
    - datasets
    - gensim
    - numpy
    - pip:
        - bayesian-optimization
        - gym
```


From everything I’ve read python packaging is nowhere close to being solved and even the people who understand what is going on under the hood have trouble. So if you find yourself in python package hell, keep going. When in doubt I rip out the environment and start over. 

If you have any questions or thoughts, always happy to chat about these types of things. My email is mthartz25[at]gmail[dot]com


#### Footnotes

1. This is completely ignoring managing any jobs in Production, which is a pretty massive omision
2. I've also used some combination of Poetry, pip, and traditional virtual enviornments to varying levels of success