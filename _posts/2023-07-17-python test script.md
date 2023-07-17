---
layout: post
title:  "Python Test Script"
date: "2023-07-17"
categories: misc
tags: [python]
---

The following is a script I made to test features in Python, using a dataset I generated from a local PostgreSQL database. 

```python
import pandas as pd 
import numpy as np 
import matplotlib.pyplot as plt 
import seaborn as sns 

#itables for viewing data
from itables import init_notebook_mode
from itables import show
```


```python
#Read data
hr_data = pd.read_csv('~/Documents/Projects/SQL/Generated Data/HR_employee_list_and_salaries.csv')
hr_data = pd.DataFrame(hr_data)

show(hr_data)
```


<style>.itables table td {
    text-overflow: ellipsis;
    overflow: hidden;
}

.itables table th {
    text-overflow: ellipsis;
    overflow: hidden;
}

.itables thead input {
    width: 100%;
    padding: 3px;
    box-sizing: border-box;
}

.itables tfoot input {
    width: 100%;
    padding: 3px;
    box-sizing: border-box;
}
</style>
<div class="itables">
<table id="08e15f6f-b50b-4a9b-9bb1-a1ddf450be64" class="display nowrap"style="table-layout:auto;width:auto;margin:auto;caption-side:bottom"><thead>
    <tr style="text-align: right;">

      <th>Name</th>
      <th>Department</th>
      <th>Email Address</th>
      <th>Title</th>
      <th>Salary</th>
      <th>Min Salary</th>
      <th>Max Salary</th>
      <th>Hire Date</th>
      <th>City</th>
      <th>ZIP Code</th>
      <th>State/Province</th>
      <th>Country</th>
    </tr>
  </thead><tbody><tr><td>Loading... (need <a href=https://mwouts.github.io/itables/troubleshooting.html>help</a>?)</td></tr></tbody></table>
<link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/1.13.1/css/jquery.dataTables.min.css">
<script type="module">
    // Import jquery and DataTable
    import 'https://code.jquery.com/jquery-3.6.0.min.js';
    import dt from 'https://cdn.datatables.net/1.12.1/js/jquery.dataTables.mjs';
    dt($);

    // Define the table data
    const data = [["Steven King", "Executive", "steven.king@sqltutorial.org", "President", 240000, 200000, 400000, "2007-06-12", "Seattle", "98199", "Washington", "United States of America"], ["Lex De Haan", "Executive", "lex.de haan@sqltutorial.org", "Administration Vice President", 170000, 150000, 300000, "2013-01-08", "Seattle", "98199", "Washington", "United States of America"], ["Neena Kochhar", "Executive", "neena.kochhar@sqltutorial.org", "Administration Vice President", 170000, 150000, 300000, "2009-09-16", "Seattle", "98199", "Washington", "United States of America"], ["John Russell", "Sales", "john.russell@sqltutorial.org", "Sales Manager", 140000, 100000, 200000, "2016-09-26", "Oxford", "OX9 9ZB", "Oxford", "United Kingdom"], ["Karen Partners", "Sales", "karen.partners@sqltutorial.org", "Sales Manager", 135000, 100000, 200000, "2016-12-31", "Oxford", "OX9 9ZB", "Oxford", "United Kingdom"], ["Michael Hartstein", "Marketing", "michael.hartstein@sqltutorial.org", "Marketing Manager", 130000, 90000, 150000, "2016-02-12", "Toronto", "M5V 2L7", "Ontario", "Canada"], ["Nancy Greenberg", "Finance", "nancy.greenberg@sqltutorial.org", "Finance Manager", 120000, 82000, 160000, "2014-08-12", "Seattle", "98199", "Washington", "United States of America"], ["Shelley Higgins", "Accounting", "shelley.higgins@sqltutorial.org", "Accounting Manager", 120000, 82000, 160000, "2014-06-02", "Seattle", "98199", "Washington", "United States of America"], ["Den Raphaely", "Purchasing", "den.raphaely@sqltutorial.org", "Purchasing Manager", 110000, 80000, 150000, "2014-12-02", "Seattle", "98199", "Washington", "United States of America"], ["Hermann Baer", "Public Relations", "hermann.baer@sqltutorial.org", "Public Relations Representative", 100000, 45000, 105000, "2014-06-02", "Munich", "80925", "Bavaria", "Germany"], ["Alexander Hunold", "IT", "alexander.hunold@sqltutorial.org", "Programmer", 90000, 40000, 100000, "2009-12-29", "Southlake", "26192", "Texas", "United States of America"], ["Daniel Faviet", "Finance", "daniel.faviet@sqltutorial.org", "Accountant", 90000, 42000, 90000, "2014-08-11", "Seattle", "98199", "Washington", "United States of America"], ["Jonathon Taylor", "Sales", "jonathon.taylor@sqltutorial.org", "Sales Representative", 86000, 60000, 120000, "2018-03-19", "Oxford", "OX9 9ZB", "Oxford", "United Kingdom"], ["Jack Livingston", "Sales", "jack.livingston@sqltutorial.org", "Sales Representative", 84000, 60000, 120000, "2018-04-18", "Oxford", "OX9 9ZB", "Oxford", "United Kingdom"], ["William Gietz", "Accounting", "william.gietz@sqltutorial.org", "Public Accountant", 83000, 42000, 90000, "2014-06-02", "Seattle", "98199", "Washington", "United States of America"], ["John Chen", "Finance", "john.chen@sqltutorial.org", "Accountant", 82000, 42000, 90000, "2017-09-23", "Seattle", "98199", "Washington", "United States of America"], ["Adam Fripp", "Shipping", "adam.fripp@sqltutorial.org", "Stock Manager", 82000, 55000, 85000, "2017-04-05", "South San Francisco", "99236", "California", "United States of America"], ["Matthew Weiss", "Shipping", "matthew.weiss@sqltutorial.org", "Stock Manager", 80000, 55000, 85000, "2016-07-13", "South San Francisco", "99236", "California", "United States of America"], ["Payam Kaufling", "Shipping", "payam.kaufling@sqltutorial.org", "Stock Manager", 79000, 55000, 85000, "2015-04-26", "South San Francisco", "99236", "California", "United States of America"], ["Jose Manuel Urman", "Finance", "jose manuel.urman@sqltutorial.org", "Accountant", 78000, 42000, 90000, "2018-03-02", "Seattle", "98199", "Washington", "United States of America"], ["Ismael Sciarra", "Finance", "ismael.sciarra@sqltutorial.org", "Accountant", 77000, 42000, 90000, "2017-09-25", "Seattle", "98199", "Washington", "United States of America"], ["Kimberely Grant", "Sales", "kimberely.grant@sqltutorial.org", "Sales Representative", 70000, 60000, 120000, "2019-05-19", "Oxford", "OX9 9ZB", "Oxford", "United Kingdom"], ["Luis Popp", "Finance", "luis.popp@sqltutorial.org", "Accountant", 69000, 42000, 90000, "2019-12-02", "Seattle", "98199", "Washington", "United States of America"], ["Susan Mavris", "Human Resources", "susan.mavris@sqltutorial.org", "Human Resources Representative", 65000, 40000, 90000, "2014-06-02", "London", "NaN", "NaN", "United Kingdom"], ["Shanta Vollman", "Shipping", "shanta.vollman@sqltutorial.org", "Stock Manager", 65000, 55000, 85000, "2017-10-05", "South San Francisco", "99236", "California", "United States of America"], ["Charles Johnson", "Sales", "charles.johnson@sqltutorial.org", "Sales Representative", 62000, 60000, 120000, "2019-12-30", "Oxford", "OX9 9ZB", "Oxford", "United Kingdom"], ["Pat Fay", "Marketing", "pat.fay@sqltutorial.org", "Marketing Representative", 60000, 40000, 90000, "2017-08-12", "Toronto", "M5V 2L7", "Ontario", "Canada"], ["Bruce Ernst", "IT", "bruce.ernst@sqltutorial.org", "Programmer", 60000, 40000, 100000, "2011-05-16", "Southlake", "26192", "Texas", "United States of America"], ["David Austin", "IT", "david.austin@sqltutorial.org", "Programmer", 48000, 40000, 100000, "2017-06-20", "Southlake", "26192", "Texas", "United States of America"], ["Valli Pataballa", "IT", "valli.pataballa@sqltutorial.org", "Programmer", 48000, 40000, 100000, "2018-01-31", "Southlake", "26192", "Texas", "United States of America"], ["Jennifer Whalen", "Administration", "jennifer.whalen@sqltutorial.org", "Administration Assistant", 44000, 30000, 60000, "2007-09-12", "Seattle", "98199", "Washington", "United States of America"], ["Diana Lorentz", "IT", "diana.lorentz@sqltutorial.org", "Programmer", 42000, 40000, 100000, "2019-02-02", "Southlake", "26192", "Texas", "United States of America"], ["Sarah Bell", "Shipping", "sarah.bell@sqltutorial.org", "Shipping Clerk", 40000, 25000, 55000, "2016-01-30", "South San Francisco", "99236", "California", "United States of America"]];

    // Define the dt_args
    let dt_args = {"order": []};
    dt_args["data"] = data;

    $(document).ready(function () {

        $('#08e15f6f-b50b-4a9b-9bb1-a1ddf450be64').DataTable(dt_args);
    });
</script>
</div>




```python
# Testing Information and Summary functions.

## Information calls: 
### Shape
hr_data.shape
```




    (33, 12)




```python
# Index
hr_data.index
```




    RangeIndex(start=0, stop=33, step=1)




```python
# Columns
hr_data.columns
```




    Index(['Name', 'Department', 'Email Address', 'Title', 'Salary', 'Min Salary',
           'Max Salary', 'Hire Date', 'City', 'ZIP Code', 'State/Province',
           'Country'],
          dtype='object')




```python
# Info (seems to be simular to head in R)
hr_data.info
```




    <bound method DataFrame.info of                  Name        Department                      Email Address  \
    0         Steven King         Executive        steven.king@sqltutorial.org   
    1         Lex De Haan         Executive        lex.de haan@sqltutorial.org   
    2       Neena Kochhar         Executive      neena.kochhar@sqltutorial.org   
    3        John Russell             Sales       john.russell@sqltutorial.org   
    4      Karen Partners             Sales     karen.partners@sqltutorial.org   
    5   Michael Hartstein         Marketing  michael.hartstein@sqltutorial.org   
    6     Nancy Greenberg           Finance    nancy.greenberg@sqltutorial.org   
    7     Shelley Higgins        Accounting    shelley.higgins@sqltutorial.org   
    8        Den Raphaely        Purchasing       den.raphaely@sqltutorial.org   
    9        Hermann Baer  Public Relations       hermann.baer@sqltutorial.org   
    10   Alexander Hunold                IT   alexander.hunold@sqltutorial.org   
    11      Daniel Faviet           Finance      daniel.faviet@sqltutorial.org   
    12    Jonathon Taylor             Sales    jonathon.taylor@sqltutorial.org   
    13    Jack Livingston             Sales    jack.livingston@sqltutorial.org   
    14      William Gietz        Accounting      william.gietz@sqltutorial.org   
    15          John Chen           Finance          john.chen@sqltutorial.org   
    16         Adam Fripp          Shipping         adam.fripp@sqltutorial.org   
    17      Matthew Weiss          Shipping      matthew.weiss@sqltutorial.org   
    18     Payam Kaufling          Shipping     payam.kaufling@sqltutorial.org   
    19  Jose Manuel Urman           Finance  jose manuel.urman@sqltutorial.org   
    20     Ismael Sciarra           Finance     ismael.sciarra@sqltutorial.org   
    21    Kimberely Grant             Sales    kimberely.grant@sqltutorial.org   
    22          Luis Popp           Finance          luis.popp@sqltutorial.org   
    23       Susan Mavris   Human Resources       susan.mavris@sqltutorial.org   
    24     Shanta Vollman          Shipping     shanta.vollman@sqltutorial.org   
    25    Charles Johnson             Sales    charles.johnson@sqltutorial.org   
    26            Pat Fay         Marketing            pat.fay@sqltutorial.org   
    27        Bruce Ernst                IT        bruce.ernst@sqltutorial.org   
    28       David Austin                IT       david.austin@sqltutorial.org   
    29    Valli Pataballa                IT    valli.pataballa@sqltutorial.org   
    30    Jennifer Whalen    Administration    jennifer.whalen@sqltutorial.org   
    31      Diana Lorentz                IT      diana.lorentz@sqltutorial.org   
    32         Sarah Bell          Shipping         sarah.bell@sqltutorial.org   
    
                                  Title  Salary  Min Salary  Max Salary  \
    0                         President  240000      200000      400000   
    1     Administration Vice President  170000      150000      300000   
    2     Administration Vice President  170000      150000      300000   
    3                     Sales Manager  140000      100000      200000   
    4                     Sales Manager  135000      100000      200000   
    5                 Marketing Manager  130000       90000      150000   
    6                   Finance Manager  120000       82000      160000   
    7                Accounting Manager  120000       82000      160000   
    8                Purchasing Manager  110000       80000      150000   
    9   Public Relations Representative  100000       45000      105000   
    10                       Programmer   90000       40000      100000   
    11                       Accountant   90000       42000       90000   
    12             Sales Representative   86000       60000      120000   
    13             Sales Representative   84000       60000      120000   
    14                Public Accountant   83000       42000       90000   
    15                       Accountant   82000       42000       90000   
    16                    Stock Manager   82000       55000       85000   
    17                    Stock Manager   80000       55000       85000   
    18                    Stock Manager   79000       55000       85000   
    19                       Accountant   78000       42000       90000   
    20                       Accountant   77000       42000       90000   
    21             Sales Representative   70000       60000      120000   
    22                       Accountant   69000       42000       90000   
    23   Human Resources Representative   65000       40000       90000   
    24                    Stock Manager   65000       55000       85000   
    25             Sales Representative   62000       60000      120000   
    26         Marketing Representative   60000       40000       90000   
    27                       Programmer   60000       40000      100000   
    28                       Programmer   48000       40000      100000   
    29                       Programmer   48000       40000      100000   
    30         Administration Assistant   44000       30000       60000   
    31                       Programmer   42000       40000      100000   
    32                   Shipping Clerk   40000       25000       55000   
    
         Hire Date                 City ZIP Code State/Province  \
    0   2007-06-12              Seattle    98199     Washington   
    1   2013-01-08              Seattle    98199     Washington   
    2   2009-09-16              Seattle    98199     Washington   
    3   2016-09-26               Oxford  OX9 9ZB         Oxford   
    4   2016-12-31               Oxford  OX9 9ZB         Oxford   
    5   2016-02-12              Toronto  M5V 2L7        Ontario   
    6   2014-08-12              Seattle    98199     Washington   
    7   2014-06-02              Seattle    98199     Washington   
    8   2014-12-02              Seattle    98199     Washington   
    9   2014-06-02               Munich    80925        Bavaria   
    10  2009-12-29            Southlake    26192          Texas   
    11  2014-08-11              Seattle    98199     Washington   
    12  2018-03-19               Oxford  OX9 9ZB         Oxford   
    13  2018-04-18               Oxford  OX9 9ZB         Oxford   
    14  2014-06-02              Seattle    98199     Washington   
    15  2017-09-23              Seattle    98199     Washington   
    16  2017-04-05  South San Francisco    99236     California   
    17  2016-07-13  South San Francisco    99236     California   
    18  2015-04-26  South San Francisco    99236     California   
    19  2018-03-02              Seattle    98199     Washington   
    20  2017-09-25              Seattle    98199     Washington   
    21  2019-05-19               Oxford  OX9 9ZB         Oxford   
    22  2019-12-02              Seattle    98199     Washington   
    23  2014-06-02               London      NaN            NaN   
    24  2017-10-05  South San Francisco    99236     California   
    25  2019-12-30               Oxford  OX9 9ZB         Oxford   
    26  2017-08-12              Toronto  M5V 2L7        Ontario   
    27  2011-05-16            Southlake    26192          Texas   
    28  2017-06-20            Southlake    26192          Texas   
    29  2018-01-31            Southlake    26192          Texas   
    30  2007-09-12              Seattle    98199     Washington   
    31  2019-02-02            Southlake    26192          Texas   
    32  2016-01-30  South San Francisco    99236     California   
    
                         Country  
    0   United States of America  
    1   United States of America  
    2   United States of America  
    3             United Kingdom  
    4             United Kingdom  
    5                     Canada  
    6   United States of America  
    7   United States of America  
    8   United States of America  
    9                    Germany  
    10  United States of America  
    11  United States of America  
    12            United Kingdom  
    13            United Kingdom  
    14  United States of America  
    15  United States of America  
    16  United States of America  
    17  United States of America  
    18  United States of America  
    19  United States of America  
    20  United States of America  
    21            United Kingdom  
    22  United States of America  
    23            United Kingdom  
    24  United States of America  
    25            United Kingdom  
    26                    Canada  
    27  United States of America  
    28  United States of America  
    29  United States of America  
    30  United States of America  
    31  United States of America  
    32  United States of America  >




```python
# Count number of non-NA values
hr_data.count()
```




    Name              33
    Department        33
    Email Address     33
    Title             33
    Salary            33
    Min Salary        33
    Max Salary        33
    Hire Date         33
    City              33
    ZIP Code          32
    State/Province    32
    Country           33
    dtype: int64




```python
# Summary Statistics: 
## Sum
hr_data['Salary'].sum()
```




    3019000




```python
# Cumulative Sum

hr_data['Salary'].cumsum()
```




    0      240000
    1      410000
    2      580000
    3      720000
    4      855000
    5      985000
    6     1105000
    7     1225000
    8     1335000
    9     1435000
    10    1525000
    11    1615000
    12    1701000
    13    1785000
    14    1868000
    15    1950000
    16    2032000
    17    2112000
    18    2191000
    19    2269000
    20    2346000
    21    2416000
    22    2485000
    23    2550000
    24    2615000
    25    2677000
    26    2737000
    27    2797000
    28    2845000
    29    2893000
    30    2937000
    31    2979000
    32    3019000
    Name: Salary, dtype: int64




```python
# Describe

hr_data.describe
```




    <bound method NDFrame.describe of                  Name        Department                      Email Address  \
    0         Steven King         Executive        steven.king@sqltutorial.org   
    1         Lex De Haan         Executive        lex.de haan@sqltutorial.org   
    2       Neena Kochhar         Executive      neena.kochhar@sqltutorial.org   
    3        John Russell             Sales       john.russell@sqltutorial.org   
    4      Karen Partners             Sales     karen.partners@sqltutorial.org   
    5   Michael Hartstein         Marketing  michael.hartstein@sqltutorial.org   
    6     Nancy Greenberg           Finance    nancy.greenberg@sqltutorial.org   
    7     Shelley Higgins        Accounting    shelley.higgins@sqltutorial.org   
    8        Den Raphaely        Purchasing       den.raphaely@sqltutorial.org   
    9        Hermann Baer  Public Relations       hermann.baer@sqltutorial.org   
    10   Alexander Hunold                IT   alexander.hunold@sqltutorial.org   
    11      Daniel Faviet           Finance      daniel.faviet@sqltutorial.org   
    12    Jonathon Taylor             Sales    jonathon.taylor@sqltutorial.org   
    13    Jack Livingston             Sales    jack.livingston@sqltutorial.org   
    14      William Gietz        Accounting      william.gietz@sqltutorial.org   
    15          John Chen           Finance          john.chen@sqltutorial.org   
    16         Adam Fripp          Shipping         adam.fripp@sqltutorial.org   
    17      Matthew Weiss          Shipping      matthew.weiss@sqltutorial.org   
    18     Payam Kaufling          Shipping     payam.kaufling@sqltutorial.org   
    19  Jose Manuel Urman           Finance  jose manuel.urman@sqltutorial.org   
    20     Ismael Sciarra           Finance     ismael.sciarra@sqltutorial.org   
    21    Kimberely Grant             Sales    kimberely.grant@sqltutorial.org   
    22          Luis Popp           Finance          luis.popp@sqltutorial.org   
    23       Susan Mavris   Human Resources       susan.mavris@sqltutorial.org   
    24     Shanta Vollman          Shipping     shanta.vollman@sqltutorial.org   
    25    Charles Johnson             Sales    charles.johnson@sqltutorial.org   
    26            Pat Fay         Marketing            pat.fay@sqltutorial.org   
    27        Bruce Ernst                IT        bruce.ernst@sqltutorial.org   
    28       David Austin                IT       david.austin@sqltutorial.org   
    29    Valli Pataballa                IT    valli.pataballa@sqltutorial.org   
    30    Jennifer Whalen    Administration    jennifer.whalen@sqltutorial.org   
    31      Diana Lorentz                IT      diana.lorentz@sqltutorial.org   
    32         Sarah Bell          Shipping         sarah.bell@sqltutorial.org   
    
                                  Title  Salary  Min Salary  Max Salary  \
    0                         President  240000      200000      400000   
    1     Administration Vice President  170000      150000      300000   
    2     Administration Vice President  170000      150000      300000   
    3                     Sales Manager  140000      100000      200000   
    4                     Sales Manager  135000      100000      200000   
    5                 Marketing Manager  130000       90000      150000   
    6                   Finance Manager  120000       82000      160000   
    7                Accounting Manager  120000       82000      160000   
    8                Purchasing Manager  110000       80000      150000   
    9   Public Relations Representative  100000       45000      105000   
    10                       Programmer   90000       40000      100000   
    11                       Accountant   90000       42000       90000   
    12             Sales Representative   86000       60000      120000   
    13             Sales Representative   84000       60000      120000   
    14                Public Accountant   83000       42000       90000   
    15                       Accountant   82000       42000       90000   
    16                    Stock Manager   82000       55000       85000   
    17                    Stock Manager   80000       55000       85000   
    18                    Stock Manager   79000       55000       85000   
    19                       Accountant   78000       42000       90000   
    20                       Accountant   77000       42000       90000   
    21             Sales Representative   70000       60000      120000   
    22                       Accountant   69000       42000       90000   
    23   Human Resources Representative   65000       40000       90000   
    24                    Stock Manager   65000       55000       85000   
    25             Sales Representative   62000       60000      120000   
    26         Marketing Representative   60000       40000       90000   
    27                       Programmer   60000       40000      100000   
    28                       Programmer   48000       40000      100000   
    29                       Programmer   48000       40000      100000   
    30         Administration Assistant   44000       30000       60000   
    31                       Programmer   42000       40000      100000   
    32                   Shipping Clerk   40000       25000       55000   
    
         Hire Date                 City ZIP Code State/Province  \
    0   2007-06-12              Seattle    98199     Washington   
    1   2013-01-08              Seattle    98199     Washington   
    2   2009-09-16              Seattle    98199     Washington   
    3   2016-09-26               Oxford  OX9 9ZB         Oxford   
    4   2016-12-31               Oxford  OX9 9ZB         Oxford   
    5   2016-02-12              Toronto  M5V 2L7        Ontario   
    6   2014-08-12              Seattle    98199     Washington   
    7   2014-06-02              Seattle    98199     Washington   
    8   2014-12-02              Seattle    98199     Washington   
    9   2014-06-02               Munich    80925        Bavaria   
    10  2009-12-29            Southlake    26192          Texas   
    11  2014-08-11              Seattle    98199     Washington   
    12  2018-03-19               Oxford  OX9 9ZB         Oxford   
    13  2018-04-18               Oxford  OX9 9ZB         Oxford   
    14  2014-06-02              Seattle    98199     Washington   
    15  2017-09-23              Seattle    98199     Washington   
    16  2017-04-05  South San Francisco    99236     California   
    17  2016-07-13  South San Francisco    99236     California   
    18  2015-04-26  South San Francisco    99236     California   
    19  2018-03-02              Seattle    98199     Washington   
    20  2017-09-25              Seattle    98199     Washington   
    21  2019-05-19               Oxford  OX9 9ZB         Oxford   
    22  2019-12-02              Seattle    98199     Washington   
    23  2014-06-02               London      NaN            NaN   
    24  2017-10-05  South San Francisco    99236     California   
    25  2019-12-30               Oxford  OX9 9ZB         Oxford   
    26  2017-08-12              Toronto  M5V 2L7        Ontario   
    27  2011-05-16            Southlake    26192          Texas   
    28  2017-06-20            Southlake    26192          Texas   
    29  2018-01-31            Southlake    26192          Texas   
    30  2007-09-12              Seattle    98199     Washington   
    31  2019-02-02            Southlake    26192          Texas   
    32  2016-01-30  South San Francisco    99236     California   
    
                         Country  
    0   United States of America  
    1   United States of America  
    2   United States of America  
    3             United Kingdom  
    4             United Kingdom  
    5                     Canada  
    6   United States of America  
    7   United States of America  
    8   United States of America  
    9                    Germany  
    10  United States of America  
    11  United States of America  
    12            United Kingdom  
    13            United Kingdom  
    14  United States of America  
    15  United States of America  
    16  United States of America  
    17  United States of America  
    18  United States of America  
    19  United States of America  
    20  United States of America  
    21            United Kingdom  
    22  United States of America  
    23            United Kingdom  
    24  United States of America  
    25            United Kingdom  
    26                    Canada  
    27  United States of America  
    28  United States of America  
    29  United States of America  
    30  United States of America  
    31  United States of America  
    32  United States of America  >




```python
# Mean

hr_data['Salary'].mean()
```




    91484.84848484848




```python
hr_data['Salary'].median()
```




    82000.0




```python
# Testing data cleaning functions
## Drop na values and then recount for correctness

hr_data = hr_data.dropna()

hr_data.count()

```




    Name              32
    Department        32
    Email Address     32
    Title             32
    Salary            32
    Min Salary        32
    Max Salary        32
    Hire Date         32
    City              32
    ZIP Code          32
    State/Province    32
    Country           32
    dtype: int64




```python
# Print rows that have executives making over $100,000

hr_data[hr_data['Salary'] >= 100000 ]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Department</th>
      <th>Email Address</th>
      <th>Title</th>
      <th>Salary</th>
      <th>Min Salary</th>
      <th>Max Salary</th>
      <th>Hire Date</th>
      <th>City</th>
      <th>ZIP Code</th>
      <th>State/Province</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Steven King</td>
      <td>Executive</td>
      <td>steven.king@sqltutorial.org</td>
      <td>President</td>
      <td>240000</td>
      <td>200000</td>
      <td>400000</td>
      <td>2007-06-12</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Lex De Haan</td>
      <td>Executive</td>
      <td>lex.de haan@sqltutorial.org</td>
      <td>Administration Vice President</td>
      <td>170000</td>
      <td>150000</td>
      <td>300000</td>
      <td>2013-01-08</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Neena Kochhar</td>
      <td>Executive</td>
      <td>neena.kochhar@sqltutorial.org</td>
      <td>Administration Vice President</td>
      <td>170000</td>
      <td>150000</td>
      <td>300000</td>
      <td>2009-09-16</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John Russell</td>
      <td>Sales</td>
      <td>john.russell@sqltutorial.org</td>
      <td>Sales Manager</td>
      <td>140000</td>
      <td>100000</td>
      <td>200000</td>
      <td>2016-09-26</td>
      <td>Oxford</td>
      <td>OX9 9ZB</td>
      <td>Oxford</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Karen Partners</td>
      <td>Sales</td>
      <td>karen.partners@sqltutorial.org</td>
      <td>Sales Manager</td>
      <td>135000</td>
      <td>100000</td>
      <td>200000</td>
      <td>2016-12-31</td>
      <td>Oxford</td>
      <td>OX9 9ZB</td>
      <td>Oxford</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Michael Hartstein</td>
      <td>Marketing</td>
      <td>michael.hartstein@sqltutorial.org</td>
      <td>Marketing Manager</td>
      <td>130000</td>
      <td>90000</td>
      <td>150000</td>
      <td>2016-02-12</td>
      <td>Toronto</td>
      <td>M5V 2L7</td>
      <td>Ontario</td>
      <td>Canada</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Nancy Greenberg</td>
      <td>Finance</td>
      <td>nancy.greenberg@sqltutorial.org</td>
      <td>Finance Manager</td>
      <td>120000</td>
      <td>82000</td>
      <td>160000</td>
      <td>2014-08-12</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Shelley Higgins</td>
      <td>Accounting</td>
      <td>shelley.higgins@sqltutorial.org</td>
      <td>Accounting Manager</td>
      <td>120000</td>
      <td>82000</td>
      <td>160000</td>
      <td>2014-06-02</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Den Raphaely</td>
      <td>Purchasing</td>
      <td>den.raphaely@sqltutorial.org</td>
      <td>Purchasing Manager</td>
      <td>110000</td>
      <td>80000</td>
      <td>150000</td>
      <td>2014-12-02</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Hermann Baer</td>
      <td>Public Relations</td>
      <td>hermann.baer@sqltutorial.org</td>
      <td>Public Relations Representative</td>
      <td>100000</td>
      <td>45000</td>
      <td>105000</td>
      <td>2014-06-02</td>
      <td>Munich</td>
      <td>80925</td>
      <td>Bavaria</td>
      <td>Germany</td>
    </tr>
  </tbody>
</table>
</div>




```python
# String replace within a column 

hr_data['Email Address'] = hr_data['Email Address'].str.replace("sqltutorial.org", "repugs.edu")

print(hr_data['Email Address'])
```

    0           steven.king@repugs.edu
    1           lex.de haan@repugs.edu
    2         neena.kochhar@repugs.edu
    3          john.russell@repugs.edu
    4        karen.partners@repugs.edu
    5     michael.hartstein@repugs.edu
    6       nancy.greenberg@repugs.edu
    7       shelley.higgins@repugs.edu
    8          den.raphaely@repugs.edu
    9          hermann.baer@repugs.edu
    10     alexander.hunold@repugs.edu
    11        daniel.faviet@repugs.edu
    12      jonathon.taylor@repugs.edu
    13      jack.livingston@repugs.edu
    14        william.gietz@repugs.edu
    15            john.chen@repugs.edu
    16           adam.fripp@repugs.edu
    17        matthew.weiss@repugs.edu
    18       payam.kaufling@repugs.edu
    19    jose manuel.urman@repugs.edu
    20       ismael.sciarra@repugs.edu
    21      kimberely.grant@repugs.edu
    22            luis.popp@repugs.edu
    24       shanta.vollman@repugs.edu
    25      charles.johnson@repugs.edu
    26              pat.fay@repugs.edu
    27          bruce.ernst@repugs.edu
    28         david.austin@repugs.edu
    29      valli.pataballa@repugs.edu
    30      jennifer.whalen@repugs.edu
    31        diana.lorentz@repugs.edu
    32           sarah.bell@repugs.edu
    Name: Email Address, dtype: object
    

    C:\Users\bruce\AppData\Local\Temp\ipykernel_22892\3629824364.py:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      hr_data['Email Address'] = hr_data['Email Address'].str.replace("sqltutorial.org", "repugs.edu")
    


```python
print(hr_data.iloc[0:5])
```

                 Name Department              Email Address  \
    0     Steven King  Executive     steven.king@repugs.edu   
    1     Lex De Haan  Executive     lex.de haan@repugs.edu   
    2   Neena Kochhar  Executive   neena.kochhar@repugs.edu   
    3    John Russell      Sales    john.russell@repugs.edu   
    4  Karen Partners      Sales  karen.partners@repugs.edu   
    
                               Title  Salary  Min Salary  Max Salary   Hire Date  \
    0                      President  240000      200000      400000  2007-06-12   
    1  Administration Vice President  170000      150000      300000  2013-01-08   
    2  Administration Vice President  170000      150000      300000  2009-09-16   
    3                  Sales Manager  140000      100000      200000  2016-09-26   
    4                  Sales Manager  135000      100000      200000  2016-12-31   
    
          City ZIP Code State/Province                   Country  
    0  Seattle    98199     Washington  United States of America  
    1  Seattle    98199     Washington  United States of America  
    2  Seattle    98199     Washington  United States of America  
    3   Oxford  OX9 9ZB         Oxford            United Kingdom  
    4   Oxford  OX9 9ZB         Oxford            United Kingdom  
    


```python
# Query 
hr_data.query('Salary > 100000')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Department</th>
      <th>Email Address</th>
      <th>Title</th>
      <th>Salary</th>
      <th>Min Salary</th>
      <th>Max Salary</th>
      <th>Hire Date</th>
      <th>City</th>
      <th>ZIP Code</th>
      <th>State/Province</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Steven King</td>
      <td>Executive</td>
      <td>steven.king@repugs.edu</td>
      <td>President</td>
      <td>240000</td>
      <td>200000</td>
      <td>400000</td>
      <td>2007-06-12</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Lex De Haan</td>
      <td>Executive</td>
      <td>lex.de haan@repugs.edu</td>
      <td>Administration Vice President</td>
      <td>170000</td>
      <td>150000</td>
      <td>300000</td>
      <td>2013-01-08</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Neena Kochhar</td>
      <td>Executive</td>
      <td>neena.kochhar@repugs.edu</td>
      <td>Administration Vice President</td>
      <td>170000</td>
      <td>150000</td>
      <td>300000</td>
      <td>2009-09-16</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John Russell</td>
      <td>Sales</td>
      <td>john.russell@repugs.edu</td>
      <td>Sales Manager</td>
      <td>140000</td>
      <td>100000</td>
      <td>200000</td>
      <td>2016-09-26</td>
      <td>Oxford</td>
      <td>OX9 9ZB</td>
      <td>Oxford</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Karen Partners</td>
      <td>Sales</td>
      <td>karen.partners@repugs.edu</td>
      <td>Sales Manager</td>
      <td>135000</td>
      <td>100000</td>
      <td>200000</td>
      <td>2016-12-31</td>
      <td>Oxford</td>
      <td>OX9 9ZB</td>
      <td>Oxford</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Michael Hartstein</td>
      <td>Marketing</td>
      <td>michael.hartstein@repugs.edu</td>
      <td>Marketing Manager</td>
      <td>130000</td>
      <td>90000</td>
      <td>150000</td>
      <td>2016-02-12</td>
      <td>Toronto</td>
      <td>M5V 2L7</td>
      <td>Ontario</td>
      <td>Canada</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Nancy Greenberg</td>
      <td>Finance</td>
      <td>nancy.greenberg@repugs.edu</td>
      <td>Finance Manager</td>
      <td>120000</td>
      <td>82000</td>
      <td>160000</td>
      <td>2014-08-12</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Shelley Higgins</td>
      <td>Accounting</td>
      <td>shelley.higgins@repugs.edu</td>
      <td>Accounting Manager</td>
      <td>120000</td>
      <td>82000</td>
      <td>160000</td>
      <td>2014-06-02</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Den Raphaely</td>
      <td>Purchasing</td>
      <td>den.raphaely@repugs.edu</td>
      <td>Purchasing Manager</td>
      <td>110000</td>
      <td>80000</td>
      <td>150000</td>
      <td>2014-12-02</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
  </tbody>
</table>
</div>




```python
# String matching and query 
hr_data[hr_data['Title'].str.contains("President|Manager")]
hr_data.query('Salary > 100000')
    
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Department</th>
      <th>Email Address</th>
      <th>Title</th>
      <th>Salary</th>
      <th>Min Salary</th>
      <th>Max Salary</th>
      <th>Hire Date</th>
      <th>City</th>
      <th>ZIP Code</th>
      <th>State/Province</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Steven King</td>
      <td>Executive</td>
      <td>steven.king@repugs.edu</td>
      <td>President</td>
      <td>240000</td>
      <td>200000</td>
      <td>400000</td>
      <td>2007-06-12</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Lex De Haan</td>
      <td>Executive</td>
      <td>lex.de haan@repugs.edu</td>
      <td>Administration Vice President</td>
      <td>170000</td>
      <td>150000</td>
      <td>300000</td>
      <td>2013-01-08</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Neena Kochhar</td>
      <td>Executive</td>
      <td>neena.kochhar@repugs.edu</td>
      <td>Administration Vice President</td>
      <td>170000</td>
      <td>150000</td>
      <td>300000</td>
      <td>2009-09-16</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>3</th>
      <td>John Russell</td>
      <td>Sales</td>
      <td>john.russell@repugs.edu</td>
      <td>Sales Manager</td>
      <td>140000</td>
      <td>100000</td>
      <td>200000</td>
      <td>2016-09-26</td>
      <td>Oxford</td>
      <td>OX9 9ZB</td>
      <td>Oxford</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Karen Partners</td>
      <td>Sales</td>
      <td>karen.partners@repugs.edu</td>
      <td>Sales Manager</td>
      <td>135000</td>
      <td>100000</td>
      <td>200000</td>
      <td>2016-12-31</td>
      <td>Oxford</td>
      <td>OX9 9ZB</td>
      <td>Oxford</td>
      <td>United Kingdom</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Michael Hartstein</td>
      <td>Marketing</td>
      <td>michael.hartstein@repugs.edu</td>
      <td>Marketing Manager</td>
      <td>130000</td>
      <td>90000</td>
      <td>150000</td>
      <td>2016-02-12</td>
      <td>Toronto</td>
      <td>M5V 2L7</td>
      <td>Ontario</td>
      <td>Canada</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Nancy Greenberg</td>
      <td>Finance</td>
      <td>nancy.greenberg@repugs.edu</td>
      <td>Finance Manager</td>
      <td>120000</td>
      <td>82000</td>
      <td>160000</td>
      <td>2014-08-12</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Shelley Higgins</td>
      <td>Accounting</td>
      <td>shelley.higgins@repugs.edu</td>
      <td>Accounting Manager</td>
      <td>120000</td>
      <td>82000</td>
      <td>160000</td>
      <td>2014-06-02</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Den Raphaely</td>
      <td>Purchasing</td>
      <td>den.raphaely@repugs.edu</td>
      <td>Purchasing Manager</td>
      <td>110000</td>
      <td>80000</td>
      <td>150000</td>
      <td>2014-12-02</td>
      <td>Seattle</td>
      <td>98199</td>
      <td>Washington</td>
      <td>United States of America</td>
    </tr>
  </tbody>
</table>
</div>




```python
# I'm telling Python to print column names and list their type, every column in this particular dataset is an object 
print("<{}>".format(hr_data.columns))

# When I run this particular line, Python is telling me which columns are of a particular type
hr_data.dtypes
```

    <Index(['Name', 'Department', 'Email Address', 'Title', 'Salary', 'Min Salary',
           'Max Salary', 'Hire Date', 'City', 'ZIP Code', 'State/Province',
           'Country'],
          dtype='object')>
    




    Name              object
    Department        object
    Email Address     object
    Title             object
    Salary             int64
    Min Salary         int64
    Max Salary         int64
    Hire Date         object
    City              object
    ZIP Code          object
    State/Province    object
    Country           object
    dtype: object




```python
# X axis automatically defaults to count, which is something I wish R did on its own! 

hr_data.plot(kind = 'line', 
             alpha = .5, 
             rot = 45)
```




    <Axes: >




    
![png](/assets/images/output_19_1.png)
    

