# web-scraper
from bs4 import BeautifulSoup 
import requests 
import csv
import pandas as pd
url="https://www.flipkart.com/search?q=laptop&sid=6bo%2Cb5g&as=on&as-show=on&otracker=AS_QueryStore_OrganicAutoSuggest_1_6_na_na_na&otracker1=AS_QueryStore_OrganicAutoSuggest_1_6_na_na_na&as-pos=1&as-type=RECENT&suggestionId=laptop%7CLaptops&requestId=7ec220e8-4f02-4150-9e0b-9e90cf692f4b&as-searchtext=laptop"
response = requests.get(url)
htmlcontent = response.content
soup = BeautifulSoup(htmlcontent,"html.parser")
products=[]
prices=[]

product=soup.find('div',attrs={'class':'_4rR01T'})
for a in soup.findAll('a',href=True, attrs={'class':'_1fQZEK'}):
    name=a.find('div',attrs={'class':'_4rR01T'})
    price=a.find('div',attrs={'class':'_30jeq3 _1_WHN1'})

    products.append(name.text)   
    prices.append(price.text)
    
df = pd.DataFrame({'Product Name':products,'Prices':prices})
df.head()
df.to_csv('products.csv')
