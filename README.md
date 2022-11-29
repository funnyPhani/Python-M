# Python-M

```python


#amazon price tracker
import requests 
import smtplib 
from bs4 import BeautifulSoup 
import time 


URL=str(input('Enter the url of the product: '))
headers = { 'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36' }
recieverMail=input('Enter your email for reminders :')


def check_price():
    
    page = requests.get(URL, headers=headers)
    soup = BeautifulSoup(page.content, 'html.parser')
#     title = soup.find('span',id='productTitle').text.strip()
#     price = soup.find(id='priceblock_dealprice').text[2:]
#     price=price.replace(',','')
#     price=float(price)   
#     print("Current Price is: ",price)
    
    
    
    
    title = soup.select("#title")[0].getText().strip()
    for i in soup.find("span",{"class":"a-price-whole"}):
        print(type(i.text))
        price = int("".join(str(i.text).split(",")))
        break
    
    print("Current Price is: ",price)
    urprice=int(input('Enter the Price, at which you want : '))


                        
    if( price < urprice ): #If the price falls below threshold, send an email
        sendmail()
        

# https://myaccount.google.com/security

def sendmail():
    '''Function called when the email needs to be sent '''
    
    # Defines an SMTP client session with the host name, here being, smtp.gmail.com as we will be using Gmail
    # to send our emails. 587 is the port number for Gmail's TLS
    server = smtplib.SMTP('smtp.gmail.com', 587) 
    server.ehlo() #Extended HELO used by email server to identify itself
    server.starttls() #Put the SMTP connection in TLS mode. All SMTP commands that follow will be encrypted
    server.ehlo()
    
    #Your email and app password. Follow the steps in the readme file to get your app password
    server.login('siginamsettyphani@gmail.com', '') 
    
    subject = 'Hey! Price fell down' #Subject of the email
    body = 'Check the link ' + URL #Body of the email
    
    msg = f"Subject: {subject}\n\n{body}" #Aggregation
      
    server.sendmail('siginamsettyphani@gmail.com', recieverMail, msg) #Sending the email
    print('Email has send to {}'.format(recieverMail))
    
    server.quit() #Closing the connection

while(True):
    check_price()
    time.sleep(120) #Checks price every 60*60 seconds i.e, every hour



```
