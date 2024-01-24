# Python-Final-Odevi
Öncelikle Visual Studio Code programı içerisinde .py uzantılı bir dosya oluşturulmuştur. Mekansal analiz yapılacağı için geopandas, pandas ve matplotlib.pyplot çalıştırılmıştır. 
import geopandas as gpd
import pandas as pd
import matplotlib.pyplot as plt

Mekansal analiz olarak Türkiye'nin illere göre nüfus dağılımı haritası oluşturulmuştur. Bu doğrultuda hem il sınırlarının yer aldığı .shp uzantılı dosya hem de nüfus verilerinin yer aldığı .csv uzantılı dosya ham veriler üzerinden yeniden düzenlenmiştir. Sonrasında ise shape ve data olarak kod tanımlanmıştır. 
data_path = r"C:\Users\User\Desktop\mineozdemmir\final\turkey81.csv"
shape_path = r"C:\Users\User\Desktop\mineozdemmir\final\turkey81.shp"

df = pd.read_csv(data_path)
print(df)
gdf = gpd.read_file(shape_path)
print(gdf) 

print("df columns:", df.columns)
print("gdf columns:", gdf.columns)

Her iki dosyada da turkey81_n ve nufus sütunları ortaktır. Tanımlanan kodlar birleştirilmiştir ve kaydedilmiştir.
merged_data = pd.merge(df, gdf, left_on='nufus', right_on='turkey81_n')

print(merged_data.head())

output_path = r"C:\Users\User\Desktop\mineozdemmir\final\merged_data.shp"
merged_data.to_pickle(output_path)
![image](https://github.com/mineozdemmir/Python-Final-Odevi/assets/146944312/9bd7dfeb-c581-42a9-a07a-fe9417f7c964)


Birleştilen kod QGis programı içerisinde Python eklentisi kullanılarak katman olarak açılmıştır.
from qgis.core import QgsVectorLayer
vector_file_path = r"C:\Users\User\Desktop\mineozdemmir\final\merged_data.shp"
vector_layer = QgsVectorLayer(vector_file_path, "My Vector Layer", "ogr")
QgsProject.instance().addMapLayer(vector_layer)
![image](https://github.com/mineozdemmir/Python-Final-Odevi/assets/146944312/95e69ab0-7b4d-42de-88a1-2f187c6d81e3)

En son olarak katman olarak açılan birleştirilmiş dosya seçenekler-semboloji yardımı ile turkey81_n sütunu içerisinde bulunan nüfus verilerine göre sınıflara ayrılmış ve renkler nüfusa göre yeniden düzenlenmiştir.
![image](https://github.com/mineozdemmir/Python-Final-Odevi/assets/146944312/fdf90b91-5527-46cb-8825-6c99ef412109)



