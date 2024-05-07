# Bili-im-Sekt-r-lan-Analizi
from bs4 import BeautifulSoup as bs
import requests
import pandas as pd

Firma = []
Konum = []
Pozisyon = []
Calisma_Sekli=[]

url = "https://www.kariyer.net/is-ilanlari/bilisim?utm_campaign=DSA_Genel_&utm_source=google&utm_medium=cpc&gad_source=1&gclid=EAIaIQobChMIiK3vzobihQMVpACiAx3l-QvQEAAYASAAEgJ4UvD_BwE"
kullanıcı = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36"}

urlreq = requests.get(url, headers=kullanıcı)
Html = urlreq.text

parsedHtml = bs(Html, "html.parser")

Firma_Veri_dizisi = parsedHtml.find_all("div", {"class": "subtitle"})
Konum_Veri_dizisi = parsedHtml.find_all("span", {"class": "location"})
Pozisyon_Veri_dizisi = parsedHtml.find_all("span", {"class": "k-ad-card-title multiline"})
Calisma_Sekli_Veri_dizisi = parsedHtml.find_all("span",{"class":"work-model"})

for firma, konum, pozisyon, calisma_sekli in zip(Firma_Veri_dizisi, Konum_Veri_dizisi, Pozisyon_Veri_dizisi, Calisma_Sekli_Veri_dizisi):
    Firma.append(firma.find("span").text.strip()) 
    Konum.append(konum.text.strip()) 
    Pozisyon.append(pozisyon.text.strip()) 
    Calisma_Sekli.append(calisma_sekli.text.strip())

data = {"Firma": Firma, "Konum": Konum, "Pozisyon": Pozisyon,"çalışma şekli":Calisma_Sekli}
df = pd.DataFrame(data)
df.to_excel("Veri311.xlsx", index=False)
