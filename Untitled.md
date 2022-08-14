Hey! This is Khanh. Thank you for giving me the opportunity to have this interview with Simple Group.

As part of the interview, my task is using C or Python to scrape data from a given website. The output should include a list of company names, websites, phone numbers, and email addresses. I'm using Jupyter notebook to present my work. 

I must say that the assignment is a lot harder than I expected. The structure of the HTML is incredibly difficult to scrape. It contains "nth type of" elements which haven't classes for each self. Not even mention tons of null values. 

Anyways, Let's dive in:


```python
# For data scrapping, I'm using BeautifulSoup for parsing HTML:

from bs4 import BeautifulSoup
import requests
import parser
import pandas as pd
import numpy as np
from time import sleep
from csv import writer
from random import randint
# If you run this cell, you should get a CSV file in the file section on the left bar.
```


```python
#Create an empty list so that we can append values:
company_name = []
website = []
phone_number = []
email = []

```


```python
# For demonstration purposes, I would only extract data from the first 5 pages:
page_number = np.arange(1,5,1)
pd.set_option('display.max_rows', None)

# Using a nested for-loop to iterate through each page and company section:
for i in page_number:
    url = "https://www.agri-biz.com/company-listings?page="+str(i)+""
    page = requests.get(url)
    soup = BeautifulSoup(page.content, 'html.parser')
    lists = soup.find_all('div', class_='company-listing')
    for list in lists:
    
    #Extract Company Name
        name = list.find('h3').text.replace('\n','') if list.find('h3') else ""
        company_name.append(name)
        
    #Extract Website
        web = list.find('a', target="_blank").text.replace('\n','') if list.find('a', target="_blank") else ""
        website.append(web)
        
    #Extract Phone number
        try:
            phone = list.find_all('span')[4].text.replace('\n','')
        except IndexError:
            phone = ""
        phone_number.append(phone)
        
```


```python
#Put them all in out Dataframe:
pd.set_option('display.max_colwidth', None)
info = pd.DataFrame({"Company Name" : company_name, "Website" : website, "Phone number" : phone_number})
info
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
      <th>Company Name</th>
      <th>Website</th>
      <th>Phone number</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>LEONG HUP FOOD PTE. LTD.</td>
      <td>www.lhfreshmart.com.sg,</td>
      <td>(65) 6757 2121</td>
    </tr>
    <tr>
      <th>1</th>
      <td>LIFA PTE LTD</td>
      <td>www.lifa.sg</td>
      <td>(65) 6250 4388</td>
    </tr>
    <tr>
      <th>2</th>
      <td>LIM THIAM CHWEE FOOD SUPPLIER PTE LTD</td>
      <td>www.Ltcfood.com.sg</td>
      <td>(65) 6778 1884</td>
    </tr>
    <tr>
      <th>3</th>
      <td>FOODNET INTERNATIONAL HOLDINGS PTE LTD</td>
      <td>www.foodnetinternational.com</td>
      <td>(65) 6841 6068</td>
    </tr>
    <tr>
      <th>4</th>
      <td>GREEN VALLEY 61 TRADING LLP</td>
      <td>greenvalley61.com.sg/en/home/</td>
      <td>(65) 6802 3661</td>
    </tr>
    <tr>
      <th>5</th>
      <td>JTECH AUTOMATION PTE LTD</td>
      <td>www.jtechautomation.com.sg</td>
      <td>(65) 6339 3616</td>
    </tr>
    <tr>
      <th>6</th>
      <td>KIM TECK HONG PTE LTD</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>7</th>
      <td>MS VENTURE PTE LTD</td>
      <td>www.msventure.com.sg</td>
      <td>(65) 6292 2888</td>
    </tr>
    <tr>
      <th>8</th>
      <td>N &amp; N AGRICULTURE PTE LTD</td>
      <td>www.eggstory.com.sg</td>
      <td>(65) 6792 9745,                                                                                      (65) 6792 9746</td>
    </tr>
    <tr>
      <th>9</th>
      <td>ALPHA CELSIUS PTE LTD</td>
      <td>www.alphacelsius.com.sg</td>
      <td>(65) 6753 6177</td>
    </tr>
    <tr>
      <th>10</th>
      <td>ALS TECHNICHEM (S) PTE LTD</td>
      <td>www.alsglobal.com</td>
      <td>(65) 6589 0118</td>
    </tr>
    <tr>
      <th>11</th>
      <td>BAN CHOON MARKETING PTE LTD</td>
      <td>www.banchoon.com.sg</td>
      <td>(65) 6777 7333</td>
    </tr>
    <tr>
      <th>12</th>
      <td>CHEE SENG OIL FACTORY (PTE) LTD</td>
      <td>www.cheeseng-oil.com</td>
      <td>(65) 6284 1062</td>
    </tr>
    <tr>
      <th>13</th>
      <td>CHUN CHENG FISHERY ENTERPRISE PTE LTD</td>
      <td>www.chuncheng.com</td>
      <td>(65) 6266 6566</td>
    </tr>
    <tr>
      <th>14</th>
      <td>GARDEN PICKS FOOD MANUFACTURING LLP</td>
      <td>www.gardenpicks.com.sg</td>
      <td>(65) 6659 4859</td>
    </tr>
    <tr>
      <th>15</th>
      <td>HIAP GIAP FOOD MANUFACTURE PTE LTD</td>
      <td>www.hiapgiapfood.com.sg</td>
      <td>(65) 6280 0361</td>
    </tr>
    <tr>
      <th>16</th>
      <td>HONG KONG NOODLE MFG (S) PTE LTD</td>
      <td>www.hongkongnoodle.sg</td>
      <td>(65) 8383 1668</td>
    </tr>
    <tr>
      <th>17</th>
      <td>HUP HENG POULTRY INDUSTRIES PTE LTD</td>
      <td>huphengp.com.sg/</td>
      <td>(65) 6257 0366 (6 Lines)</td>
    </tr>
    <tr>
      <th>18</th>
      <td>LEE HUAT SEAFOOD SUPPLIER</td>
      <td>www.leehuatseafood.com</td>
      <td>(65) 6262 1393</td>
    </tr>
    <tr>
      <th>19</th>
      <td>LEONG GUAN FOOD MANUFACTURER PTE LTD</td>
      <td>www.leongguan.com</td>
      <td>(65) 6754 7911,                                                                                      (65) 6752 4188</td>
    </tr>
    <tr>
      <th>20</th>
      <td>MEGA PACKERS ASSOCIATE PTE LTD</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>21</th>
      <td>MM STAR PTE LTD</td>
      <td>mmstar.com.sg</td>
      <td>(65) 6826 1069</td>
    </tr>
    <tr>
      <th>22</th>
      <td>NG CHEE LEE PTE LTD</td>
      <td>www.ngcheelee.com</td>
      <td>(65) 6269 4822</td>
    </tr>
    <tr>
      <th>23</th>
      <td>SENG CHOON FARM PTE LTD</td>
      <td>www.sengchoonfarm.com</td>
      <td>(65) 6762 2858</td>
    </tr>
    <tr>
      <th>24</th>
      <td>SONG FISH DEALER PTE LTD</td>
      <td>www.songfish.com.sg</td>
      <td>(65) 6777 3939</td>
    </tr>
    <tr>
      <th>25</th>
      <td>XIAO SAN FRUITS PTE LTD</td>
      <td>www.xiaosanfruits.com.sg</td>
      <td>(65) 9050 8833</td>
    </tr>
    <tr>
      <th>26</th>
      <td>ALAMANDA SINGAPORE PTE LTD</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>27</th>
      <td>ALLIANCE COLD STORAGE PTE LTD</td>
      <td>www.alliancecs.com</td>
      <td>(65) 6515 4253</td>
    </tr>
    <tr>
      <th>28</th>
      <td>ANTHONY P.J.K. SUPPLIER</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>29</th>
      <td>AROMA TRUFFLE &amp; CO.</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>30</th>
      <td>ATLAS ICE (SINGAPORE) PRIVATE LIMITED</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>31</th>
      <td>BIO-FLORA (SINGAPORE) PTE LTD</td>
      <td>www.bioflora.com.sg</td>
      <td>(65) 6582 0896</td>
    </tr>
    <tr>
      <th>32</th>
      <td>CHAROEN POKPHAND INTERTRADE SINGAPORE (PTE) LTD</td>
      <td>www.cpbrand.com/sg/</td>
      <td>(65) 6538 7020</td>
    </tr>
    <tr>
      <th>33</th>
      <td>CHENGTAI NURSERY PTE LTD</td>
      <td>www.chengtainursery.com</td>
      <td>(65) 6765 3333</td>
    </tr>
    <tr>
      <th>34</th>
      <td>CHEW'S AGRICULTURE PTE LTD</td>
      <td>www.chewsegg.com</td>
      <td>(65) 6793 7674,                                                                                      (65) 6793 7678</td>
    </tr>
    <tr>
      <th>35</th>
      <td>CHIT HUAT PTE LTD</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>36</th>
      <td>CHUA PENG SENG FOOD MANUFACTURING</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>37</th>
      <td>CKT FOOD PTE LTD</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>38</th>
      <td>CP FOODS SINGAPORE PTE. LTD.</td>
      <td>www.cpshopz.sg</td>
      <td>(65) 6538 7020</td>
    </tr>
    <tr>
      <th>39</th>
      <td>CRETEL FOOD EQUIPMENT PTE LTD</td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>




```python
#Saving data in Excel file:
info.to_excel("Khanh Assignment.xlsx")
```

Unfortunately, I couldn't manage to get the email addresses even with tons of hours on Youtube and Stack Overflow. I might though if I were given more time. I think It requires a bit of experience with HTML, CSS, or even Selenium to pull it off. Nevertheless, I'm not the type of guy who gives up easily. I tried using a variety of scraping tools but still couldn't get the email addresses. I guess the website is trying to prevent us from using its data.

The tools in action:
![image.png](attachment:image.png)

Yet, the final result still couldn't get the email addresses:
![image.png](attachment:image.png)


I'm kind of a result-oriented guy. I believe it's not how you start that's important, it's how you finish. It's sensible to try different ways to achieve the same result. (I still prefer working with Python though.)

Anyways, This is the end of my presentation. That's basically how I would approach this problem. 

Thank you for your time and I wish you a great day! Looking forward to your feedback.

Khanh.



```python

```
