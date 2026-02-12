# Introduction

### **This book record some ideas from paper and the notes from my own experience.**

-------------------------------
------

### **👍👍reference to build a own book**

0. create a new repository, and copy the address, like https://github.com/cynthier/single-cell-analysis.git, then connect the github
```
git remote add origin https://github.com/cynthier/single-cell-analysis.git
```

1. install package of jupyter-book
```
pip install "jupyter-book>=2.0.0" 
or
conda install conda-forge::jupyter-book -y
```

2. create a folder and a xx.md (your content or for test only) file, then initiate the book using command below in this directory (eg. cd xxx/directory). 

```
jupyter book init
```

3. A myst.yml file will automatically generated in this folder, then add you github repository link to this file (where labelled (!!!!!!)) 
```
# See docs at: https://mystmd.org/guide/frontmatter
version: 1

project:
  id: 4da9cb15-177c-41f5-8c4e-6a24b4e87eab
  # title:
  # description:
  # keywords: []
  # authors: []
  # github: https://github.com/cynthier/single-cell-analysis.git (!!!!!!!)
  # To autogenerate a Table of Contents, run "jupyter book init --write-toc"
site:
  template: book-theme
  # options:
  #   favicon: favicon.ico
  #   logo: site_logo.png
```

4. Then publish the websit
```
jupyter book init --gh-pages
```

5. every time after updating, using the commands to push the updates
```
git add . 
git commit -m "new"
git push
```