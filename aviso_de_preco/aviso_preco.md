from bs4 import BeautifulSoup
import requests
import smtplib
import email.message

URL = "https://www.brasiltronic.com.br/camera-digital-fujifilm-gfx-50r-medio-formato-somente-corpo"

headers = {"User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.2.1 Safari/605.1.15"}
           
site = requests.get (URL, headers=headers)

soup = BeautifulSoup(site.content, "html.parser")

title = soup.find('h1', class_ ="product-name").get_text()
                  
price = soup.find("span", class_ ='PrecoPrincipal color-tone-2').get_text().strip()

num_price = price[3:8]
num_price = num_price.replace('.','')
num_price = float(num_price)

preco_minimo = 29000

def send_email():
    email_content = """https://www.brasiltronic.com.br/camera-digital-fujifilm-gfx-50r-medio-formato-somente-corpo"""

    msg = email.message.Message()
    msg['Subject'] = 'Preco Câmera FUJIFILM GFX 50R BAIXOU! !!!'
    
    msg[ 'From'] = 'EMAIL DO REMETENTE'
    msg['To'] = 'EMAIL DO DESTINATÁRIO'

    password ='SENHA DO REMETENTE'

    msg. add_header ('Content-Type', 'text/html')
    msg.set_payload(email_content)

    s = smtplib.SMTP('smtp.gmail.com: 587')
    s.starttls()
    s.login(msg['From'], password)
    s.sendmail(msg['From'], [msg['To']], msg.as_string())

    print("Sucesso ao enviar enail.")

if (num_price < preco_minimo):
    send_email()
