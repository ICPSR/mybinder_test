
# National Longitudinal Study of Adolescent to Adult Health (Add Health), 1994-2008 [Public Use] (ICPSR 21600)


#### DEMO PYTHON NOTEBOOK- This notebook is a demostration of how to use an interactive R notebook with the Add Health study
## Principal Investigator(s):  Harris, Kathleen Mullan, University of North Carolina-Chapel Hill; Udry, J. Richard, University of North Carolina-Chapel Hill

## Summary: 
The National Longitudinal Study of Adolescent to Adult Health (Add Health), 1994-2008 [Public Use] is a longitudinal study of a nationally representative sample of U.S. adolescents in grades 7 through 12 during the 1994-1995 school year. The Add Health cohort was followed into young adulthood with four in-home interviews, the most recent conducted in 2008 when the sample was aged 24-32. Add Health combines longitudinal survey data on respondents' social, economic, psychological, and physical well-being with contextual data on the family, neighborhood, community, school, friendships, peer groups, and romantic relationships.

Add Health Wave I data collection took place between September 1994 and December 1995, and included both an in-school questionnaire and in-home interview. The in-school questionnaire was administered to more than 90,000 students in grades 7 through 12, and gathered information on social and demographic characteristics of adolescent respondents, education and occupation of parents, household structure, expectations for the future, self-esteem, health status, risk behaviors, friendships, and school-year extracurricular activities. All students listed on a sample school's roster were eligible for selection into the core in-home interview sample. In-home interviews included topics such as health status, health-facility utilization, nutrition, peer networks, decision-making processes, family composition and dynamics, educational aspirations and expectations, employment experience, romantic and sexual partnerships, substance use, and criminal activities. A parent, preferably the resident mother, of each adolescent respondent interviewed in Wave I was also asked to complete an interviewer-assisted questionnaire covering topics such as inheritable health conditions, marriages and marriage-like relationships, neighborhood characteristics, involvement in volunteer, civic, and school activities, health-affecting behaviors, education and employment, household income and economic assistance, parent-adolescent communication and interaction, parent's familiarity with the adolescent's friends and friends' parents.

Add Health data collection recommenced for Wave II from April to August 1996, and included almost 15,000 follow-up in-home interviews with adolescents from Wave I. Interview questions were generally similar to Wave I, but also included questions about sun exposure and more detailed nutrition questions. Respondents were asked to report their height and weight during the course of the interview, and were also weighed and measured by the interviewer.

From August 2001 to April 2002, Wave III data were collected through in-home interviews with 15,170 Wave I respondents (now 18 to 26 years old), as well as interviews with their partners. Respondents were administered survey questions designed to obtain information about family, relationships, sexual experiences, childbearing, and educational histories, labor force involvement, civic participation, religion and spirituality, mental health, health insurance, illness, delinquency and violence, gambling, substance abuse, and involvement with the criminal justice system. High School Transcript Release Forms were also collected at Wave III, and these data comprise the Education Data component of the Add Health study.

Wave IV in-home interviews were conducted in 2008 and 2009 when the original Wave I respondents were 24 to 32 years old. Longitudinal survey data were collected on the social, economic, psychological, and health circumstances of respondents, as well as longitudinal geographic data. Survey questions were expanded on educational transitions, economic status and financial resources and strains, sleep patterns and sleep quality, eating habits and nutrition, illnesses and medications, physical activities, emotional content and quality of current or most recent romantic/cohabiting/marriage relationships, and maltreatment during childhood by caregivers. Dates and circumstances of key life events occurring in young adulthood were also recorded, including a complete marriage and cohabitation history, full pregnancy and fertility histories from both men and women, an educational history of dates of degrees and school attendance, contact with the criminal justice system, military service, and various employment events, including the date of first and current jobs, with respective information on occupation, industry, wages, hours, and benefits. Finally, physical measurements and biospecimens were also collected at Wave IV, and included anthropometric measures of weight, height and waist circumference, cardiovascular measures such as systolic blood pressure, diastolic blood pressure, and pulse, metabolic measures from dried blood spots assayed for lipids, glucose, and glycosylated hemoglobin (HbA1c), measures of inflammation and immune function, including High sensitivity C-reactive protein (hsCRP) and Epstein-Barr virus (EBV).

## DataSet:
This data is hosted locally ( will eventually be available on a github repository).

## Citation
Harris, Kathleen Mullan, and J. Richard Udry. National Longitudinal Study of Adolescent to Adult Health (Add Health), 1994-2008 [Public Use]. ICPSR21600-v17. Chapel Hill, NC: Carolina Population Center, University of North Carolina-Chapel Hill/Ann Arbor, MI: Inter-university Consortium for Political and Social Research [distributors], 2016-05-25. https://doi.org/10.3886/ICPSR21600.v17

## Geographic Coverage : United States 
## Time Period : 1994-2008
## Date of Collection: 
* 1994-01--1995-12
* 1996-04--1996-09
* 2001-04--2002-04
* 2007-04--2009-01

## Unit of Observation: Individual 
## Data Types(s) : Survey Data 
## Data Collection Notes:

The current release represents a full collection update. All data and documentation files have been resupplied by the Principal Investigators and have been fully curated by ICPSR. ICPSR has revised dataset names and numbers to better reflect the organization of the collection by study wave. Users should be aware that version history notes dated prior to 2015-11-09 do not apply to the current organization of the datasets.




```python
import ipywidgets as widgets 
from IPython.display import display
from IPython.display import clear_output

import bqplot
from bqplot import pyplot as pl
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import plotly as py
import plotly.plotly as ply
import plotly.graph_objs as go
from plotly.tools import FigureFactory as FF
import scipy

import matplotlib as matplot
import seaborn as sns
import rpy2
from __future__ import print_function
from ipywidgets import interact, interactive, fixed, interact_manual

%matplotlib inline
%load_ext rpy2.ipython
%R require(ggplot2); require(tidyr)

```

    The rpy2.ipython extension is already loaded. To reload it, use:
      %reload_ext rpy2.ipython
    




    array([1], dtype=int32)




```python
Wave_one_df = pd.read_table("21600-0001-Data.tsv", low_memory=False, na_values= [' '])
```


```python
def click(b):
    with out:
        clear_output()
        display(Wave_one_df.head(n_wid.value))
        

style = {'description_width': 'initial'}

n_wid = widgets.IntText(
    value=10,
    description='Number of rows to display:',
    style=style
)

out=widgets.Output()
button=widgets.Button(description='Click To Show Data')
button.on_click(click)

display(n_wid)
display(button)
display(out)
```


    A Jupyter Widget



    A Jupyter Widget



    A Jupyter Widget



```python
variables = Wave_one_df.dtypes.index 


```


```python
from rpy2.robjects import pandas2ri
pandas2ri.activate()
from rpy2.robjects.packages import importr
base = importr('base')


```


```python

def drop_down(drop_down_input):
    with out:
        clear_output()
        print(base.summary(Wave_one_df[drop_down_input]))
        print(base.table(Wave_one_df[drop_down_input]))
        plt.hist(Wave_one_df[drop_down_input].dropna()) #dropped Na values 
        plt.title("Histogram of Frequency Counts")
        plt.xlabel("Value")
        plt.ylabel("Frequency")
        plt.show()
        
       # print (base.table(Wave_one_data[drop_down])
        #display(Wave_one_df[drop_down].describe()) This is the python version but I was able to pull R version
        
        #do plost using BQ plot 
        

#notebook viewer - for default values, for documentation add list of publications 

        
drop_down_selction = widgets.Select(
    options = variables,
    description = "Variables"
    
    
    
)
out = widgets.Output()

display(drop_down_selction)
display(out)
Select_update = widgets.interactive(drop_down, drop_down_input = drop_down_selction)
Select_update.observe(drop_down, 'value')

```


    A Jupyter Widget



    A Jupyter Widget



```python
import traitlets

from ipywidgets import Button, HBox, VBox, Layout, Box
```


```python

def on_button_clicked (button):
    with out:
        clear_output()
        print (pd.crosstab(Wave_one_df[d1.value], Wave_one_df[d2.value], margins =True))
        plt.hist(pd.crosstab(Wave_one_df[d1.value], Wave_one_df[d2.value]).dropna()) #dropped Na values 
        plt.title("Graph of Cross Tabulations")
        plt.xlabel(d1.value)
        plt.ylabel(d2.value)
        plt.show()
        
        
    

style = {'description_width': 'initial'}



d1  = widgets.Select(
    options = variables,
    description = "Select First Variable",
    style=style
)

        
d2 = widgets.Select(
    options = variables,
    description = "Select Second Variable",
    style=style
    
   
)


a1 = widgets.Button(
    description='Click to generate cross tabulation',
    disabled=False,
    layout=Layout(width='50%', height='80px')
)


out = widgets.Output()
#display(out)

a1.on_click(on_button_clicked)

select_update1 = widgets.interactive(d1)
select_update1.observe(d1, 'vaule')


select_update2 = widgets.interactive(d2)
select_update2.observe(d2, 'vaule')


items = [d1,d2,a1]
box_layout = Layout(display='flex',
                    flex_flow='row',
                    align_items='stretch',
                    border='solid',
                    width='50%')
box = Box(children=items, layout=box_layout)


#display(d1)
#display(d2)
#display(a1)
display(box)
display(out)

#Link all three widgets 


```


    A Jupyter Widget



    A Jupyter Widget


## Related Publications

2018 Pollard, M.S.,  Tucker, J.S.,  Green, H.D.,  de la Haye, K.,  Espelage, D.L. . Adolescent peer networks and the moderating role of depressive symptoms on developmental trajectories of cannabis use. Addictive Behaviors. 76, 34-40.
Full Text Options: DOI Worldcat	Google Scholar	
Export Options: RIS/EndNote

2017 Ajilore, O.,  Alberda, G. Peer effects and political participation: What is the role of coursework clusters?. Review of Regional Studies. 47, (1), 47-62.
Full Text Options: Worldcat	Google Scholar	
Export Options: RIS/EndNote

2017 Amato, Paul R.,  Patterson, Sarah E. The intergenerational transmission of union instability in early adulthood. Journal of Marriage and Family.
Full Text Options: DOI Worldcat	Google Scholar	
Export Options: RIS/EndNote

2017 Bainbridge, K.E.,  Roy, N.,  Losonczy, K.G.,  Hoffman, H.J.,  Cohen, S.M. . Voice disorders and associated risk markers among young adults in the United States. Laryngoscope.
Full Text Options: DOI Worldcat	Google Scholar	
Export Options: RIS/EndNote

2017 Barnert, Elizabeth S.,  Dudovitz, Rebecca,  Nelson, Bergen B.,  Coker, Tumaini R.,  Biely, Christopher,  Li, Ning,  Chung, Paul J. How does incarcerating young people affect their adult health outcomes?. Pediatrics. 139, (2), e20162624
Full Text Options: DOI Worldcat	Google Scholar	
Export Options: RIS/EndNote

2017 Bradshaw, Matt,  Kent, Blake Victor,  Henderson, W. Matthew,  Setar, Anna Catherine . Subjective social status, life course SES, and BMI in young adulthood. Health Psychology.
Full Text Options: DOI Worldcat	Google Scholar	
Export Options: RIS/EndNote

2017 Brumley, Lauren D.,  Jaffee, Sara R.,  Brumley, Benjamin P. Pathways from childhood adversity to problem behaviors in young adulthood: The mediating role of adolescents’ future expectations. Journal of Youth and Adolescence. 46, (1), 1-14.
Full Text Options: DOI Worldcat	Google Scholar	

Export Options: RIS/EndNote
2017 Chen, Frances R.,  Rothman, Emily F.,  Jaffee, Sara R. Early puberty, friendship group characteristics, and dating abuse in US girls. Pediatrics. 139, (6), e20162847
Full Text Options: DOI Worldcat	Google Scholar	
Export Options: RIS/EndNote

2017 Chen, P.,  Hussey, J.M.,  Monbureau, T.O. . Depression and antidepressant use among Asian and Hispanic adults: Association with immigrant generation and language use. Journal of Immigrant and Minority Health.
Full Text Options: DOI Worldcat	Google Scholar	
Export Options: RIS/EndNote

2017 Cheng, T.C.,  Lo, C.C. . Social risk and protective factors in adolescents' reduction and cessation of alcohol use. Substance Use and Misuse.
Full Text Options: DOI Worldcat	Google Scholar	
Export Options: RIS/EndNote

# End of Notebook


```python

```
