# ANALYTICS
import requests 
from bs4 import BeautifulSoup 
import csv
from datetime import datetime
from multiprocessing import Pool

def get_html(url):
    r=request.get(url) #Response
    return r.text      #Возвращает HTML-код станицы (url)

def get_all_links(html):
    soup=BeautifulSoup(html, 'lxml')
    
    art=soup.find('div', class_='panel widget crum_partners_widget').find_all('article', class_='post hentry')
    links=[]
    
    for article in art:
        a=article.find('a').get('href')
        link='http://www.rabotainvalidam.ru/' + a
        links.append(link)
    return links

def get_page_data(html):
    soup=BeautifulSoup(html, 'lxml')
    try:
        name=soup.find('h1', class_='page-title').text.strip()
    except:
        name=''    
    
    try:
        location=soup.find('div', class_='field-item odd').text.strip()
    except:
        location=''    
    
    try:
        money=soup.find('div', class_='field-item even').text.strip()
    except:
        money=''
        
    try:
        description=soup.find('div', class_='field-item even').text.strip()
    except:
        description=''
        
    try:
        description=soup.find('div', class_='field-item even').text.strip()
    except:
        description=''
    
    try:
        schedule=soup.find('div', class_='field-item even').text.strip()
    except:
        schedule=''
        
    try:
        employment=soup.find('div', class_='field-item even').text.strip()
    except:
        employment=''
        
    try:
        quota=soup.find('div', class_='field-item even').text.strip()
    except:
        quota=''  
    
    try:
        place=soup.find('div', class_='field-item even').text.strip()
    except:
        place=''
        
    try:
        housing=soup.find('div', class_='field-item even').text.strip()
    except:
        housing=''
    
    try:
        phone=soup.find('div', class_='field-item even').text.strip()
    except:
        phone=''
        
    try:
        email=soup.find('div', class_='field-item even').text.strip()
    except:
        email=''
        
    try:
        address=soup.find('div', class_='field-item even').text.strip()
    except:
        address=''
        
    try:
        quantity=soup.find('div', class_='field-item even').text.strip()
    except:
        quantity='' 
        
    try:
        company=soup.find('div', class_='field-item even').text.strip()
    except:
        company=''
        
    data={'name':name,
          'location':location,
          'money':money,
          'description':description,
          'schedule':schedule,
          'employment':employment,
          'quota':quota,
          'place':place,
          'housing':housing,
          'phone':phone,
          'email':email,
          'address':address,
          'quantity':quantity,
          'company':company}
    return data

def write_csv(data):
    with open ('rabotainvalidam.csv', 'a') as f:
        writer=csv.writer(f)
        writer.writerow( (data['name'],
                          data['location'],
                          data['money'],
                          data['description'],
                          data['schedule'],
                          data['employment'],
                          data['quota'],
                          data['place'],
                          data['housing'],
                          data['phone'],
                          data['email'],
                          data['address'],
                          data['quantity'],
                          data['company']) )
        
        print (data['name'], 'parsed')
               
def make_all(url):
    html= get_html(url)
    data=get_page_data(html)
    write_csv(data)
        
def main ():
    start=datetime.now()
    url='http://www.rabotainvalidam.ru/jobs'
    all_links=get_all_links(get_html(url))   
        
    with Pool(30) as  p:
        p.map(make_all, all_links)
        
        
    end=datetime.now()
    
    total=end-start
    
    print(str(total))
