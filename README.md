# Extraction_NLP



import spacy
import re
from datetime import datetime

# spaCy modelini yükleyin
nlp = spacy.load("en_core_web_sm")

# Anlaşma büyüklüğünü ve tarihini çıkarmak istediğiniz metni tanımlayın
text = """
On January 15, 2023, Company A signed a deal worth $5 million with Company B.
Another deal was signed on February 20, 2024, for an amount of €3.5 million.
"""

def extract_deal_info(text):
    doc = nlp(text)
    
    # Tarihleri çıkarmak için spaCy'nin tarih tanıma yeteneklerini kullanın
    dates = []
    for ent in doc.ents:
        if ent.label_ == "DATE":
            try:
                parsed_date = datetime.strptime(ent.text, "%B %d, %Y")
                dates.append(parsed_date)
            except ValueError:
                # Tarihi doğru formatta çıkaramazsa, hatayı yoksayın
                pass
    
    # Anlaşma büyüklüğünü (fiyat) çıkarmak için düzenli ifadeleri kullanın
    prices = re.findall(r'\$\d+\.?\d*\s*million|\€\d+\.?\d*\s*million', text)

    # Anlaşma bilgilerini bir liste olarak döndürün
    deals = []
    for date, price in zip(dates, prices):
        deals.append({"date": date, "price": price})
    
    return deals

# Anlaşma bilgilerini çıkarın
deal_info = extract_deal_info(text)
print(deal_info)







[
    {'date': datetime.datetime(2023, 1, 15, 0, 0), 'price': '$5 million'},
    {'date': datetime.datetime(2024, 2, 20, 0, 0), 'price': '€3.5 million'}
]





