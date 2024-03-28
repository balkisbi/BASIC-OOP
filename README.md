Task 1:
Basic OOP
Buatlah class MarketingDataETL yang memiliki tiga metode:
extract(): akan membaca data dari sebuah file CSV (Misalkan, marketing_data.csv)

import pandas as pd

class MarketingDataETL():

    def __init__(self):
        self.data = pd.read_csv("marketing_data.csv", sep=';')

    def extract(self):
        print("Data Before Transform:")
        print(self.data)
        return self.data
transform(): akan melakukan pembersihan dan transformasi sederhana pada data (seperti mengubah format tanggal atau membersihkan nilai yang kosong)

    def transform(self):
        if 'purchase_date' in self.data.columns:
            self.data.dropna(subset=['purchase_date'], inplace=True)
            self.data['purchase_date'] = pd.to_datetime(self.data['purchase_date'], errors='coerce')
            print("Data After Transform:")
            print(self.data)
        return self.data
        
store(): akan menyimpan data yang telah ditransformasi ke dalam struktur data DataFramet.





    def store(self):
        if hasattr(self, 'data'):
            self.data.to_csv('marketing_data_processed.csv', index=False)
            print("Data stored successfully as CSV.")
        else:
            print("Data storage failed. Please check the file and try again.")

data = MarketingDataETL()

data_before_transform = data.extract()

data_after_transform = data.transform()

data.store()
