# RiskAnalyzer

> A tool for quantitative risk analysis of Android applications based on machine
> learning techniques.


**RiskAnalyzer** (**Risk Index for Android**) is a tool for quantitative risk
analysis of Android applications written in Java (used to check the permissions of the
apps) and Python (used to compute a risk value based on apps' permissions). The tool
uses classification techniques through *scikit-learn*, a machine learning library for
Python, in order to generate a numeric risk value between 0 and 100 for a given app.
In particular, the following classifiers of *scikit-learn* are used in **RiskAnalyzer**
(this list is chosen after extensive empirical assessments):
* Support Vector Machines (SVM)
* Multinomial Naive Bayes (MNB)
* Gradient Boosting (GB)
* Logistic Regression (LR)

Unlike other tools, **RiskAnalyzer** does not take into consideration only the
permissions declared into the app manifest, but carries out reverse engineering on
the apps to retrieve the bytecode and then infers (through static analysis) which
permissions are actually used and which not, extracting in this way 4 sets of
permissions for every analyzed app:
* Declared permissions - extracted from the app manifest
* Exploited permissions - declared and actually used in the bytecode
* Ghost permissions - not declared but with usages in the bytecode
* Useless permissions - declared but never used in the bytecode

From the above sets of permissions (and considering only the official list of Android
permissions), feature vectors (made by `0`s and `1`s) are built and given to the
classifiers, which then compute a risk value. 


## ❱ Demo

You can browse the full experimental results through a web interface and calculate the
risk of new applications (by uploading the `.apk` file). Below you can see a brief
demo of RiskAnalyzer:

![Web](https://raw.githubusercontent.com/ClaudiuGeorgiu/RiskAnalyzer/master/docs/demo/web.gif)



## ❱ Installation & Usage

To get the working copy of RiskAnalyzer on your own computer:
By [using directly the source code](#from-source) in a `Python 3` environment. The first thing to do is to get a local copy of this repository, so open up a
terminal in the directory where you want to save the project and clone the repository:

```Shell
$ git clone https://github.com/akriti-upadhyay/RiskAnalyzer.git
```

----------------------------------------------------------------------------------------

#### Prerequisites

To use RiskAnalyzer you need `Python 3` (at least `3.7`), `Java` (at least version `8`)
and a tool to extract the content of `RiskAnalyzer/app/database/permission_db.7z`
archive (e.g., `p7zip-full` can be used for this task in Ubuntu). Note: although
possible, the installation of some of the Python libraries is not straightforward
on Windows, the usage of a Linux distribution such as Ubuntu is advised.

#### Install

Run the following commands in the main directory of the project (`RiskAnalyzer/`)
to install the needed dependencies:

```Shell
$ # Make sure to run the commands in RiskAnalyzer/ directory.

$ # Extract permission_db.db from app/database/permission_db.7z archive and put 
$ # it into app/database/ directory.

$ # The usage of a virtual environment is highly recommended, e.g., virtualenv.
$ # If not using virtualenv (https://virtualenv.pypa.io/), skip the next 2 lines.
$ virtualenv -p python3 venv
$ source venv/bin/activate

$ # Install RiskAnalyzer's requirements.
$ python3 -m pip install -r requirements.txt
```

#### Start RiskAnalyzer

RiskAnalyzer is now ready to be used, run the following command to start the web
interface of the tool:

```Shell
$ # Make sure to run the command in RiskAnalyzer/ directory.
$ python3 app/app.py

$ # Navigate to http://localhost:5000/ to use RiskAnalyzer.
```

`NOTE:` the repository already contains the pre-trained models for the used
classifiers, if you want to train the models again (e.g., to use a newer version of
*scikit-learn*) just delete the contents of `app/models/` directory. The models will
be recreated from the source data the next time an application is analyzed.


