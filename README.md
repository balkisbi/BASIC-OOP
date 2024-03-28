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




Task 2:

Gunakan inheritance untuk membuat class TargetedMarketingETL yang mewarisi dari MarketingDataETL.

    class TargetedMarketingETL(MarketingDataETL):

    def __init__(self):
        super().__init__()

Tambahkan metode segment_customers() yang mengelompokkan pelanggan berdasarkan kriteria tertentu (misalnya, pengeluaran total atau kategori produk yang dibeli).

    def segment_customers(self):
        if self.data is not None:
            conditions = [
                (self.data['amount_spent'] <= 100),
                (self.data['amount_spent'] > 100) & (self.data['amount_spent'] <= 200),
                (self.data['amount_spent'] > 200)
            ]
            choices = ['Low Spending', 'Medium Spending', 'High Spending']
            self.data['spending_segment'] = pd.Series(np.select(conditions, choices), index=self.data.index)
            print("Segmenting Customers Based on Spending:")
            print(self.data)



Demonstrasi polymorphism dengan meng-override metode transform() dalam TargetedMarketingETL untuk menambahkan logika segmentasi pelanggan ke dalam proses transformasi.

    def transform(self):
        super().transform()
        self.segment_customers()
    def store_segmented_data(self, output_file_path):
        if self.data is not None:
            try:
                self.data.to_excel(output_file_path, index=False)
                print(f"Segmented Data Successfully Saved To {output_file_path}")
            except Exception as e:
                print(f"Error Saving Segmented Data: {e}")

                targeted_marketing_etl = TargetedMarketingETL()
                targeted_marketing_etl.extract()
                targeted_marketing_etl.transform()
                targeted_marketing_etl.store_segmented_data("segmented_data_etl.xlsx")
