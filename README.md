# Python-Final-Odevi
Öncelikle Visual Studio Code programı içerisinde .py uzantılı bir dosya oluşturulmuştur. Mekansal analiz yapılacağı için geopandas, pandas ve matplotlib.pyplot çalıştırılmıştır. 
import geopandas as gpd
import pandas as pd
import matplotlib.pyplot as plt

Mekansal analiz olarak Türkiye'nin illere göre nüfus dağılımı haritası oluşturulmuştur. Bu doğrultuda hem il sınırlarının yer aldığı .shp uzantılı dosyaya hem de nüfus verilerinin yer aldığı .csv uzantılı dosya ham veriler üzerinden yeniden düzenlenmiştir. Her iki dosyada da turkey81_n sütunu ortaktır. Sonrasında ise shape ve data olarak kod tanımlanmıştır. 
data_path = r"C:\Users\User\Desktop\mineozdemmir\final\turkey81.csv"
shape_path = r"C:\Users\User\Desktop\mineozdemmir\final\turkey81.shp"

df = pd.read_csv(data_path)
print(df)
gdf = gpd.read_file(shape_path)
print(gdf) 

print("df columns:", df.columns)
print("gdf columns:", gdf.columns)

Tanımlanan kodlar birleştirilmiştir ve kaydedilmiştir.
merged_data = pd.merge(df, gdf, left_on='turkey81_n', right_on='turkey81_n')

print(merged_data.head())

output_path = r"C:\Users\User\Desktop\mineozdemmir\final\merged_data.shp"
merged_data.to_pickle(output_path)
![image](https://github.com/mineozdemmir/Python-Final-Odevi/assets/146944312/c454cfa9-348d-4d0b-8580-03d3513e201c)

Birleştilen kod QGis programı içerisinde Python eklentisi kullanılarak katman olarak açılmıştır.
from qgis.core import QgsVectorLayer
vector_file_path = r"C:\Users\User\Desktop\mineozdemmir\final\merged_data.shp"
vector_layer = QgsVectorLayer(vector_file_path, "My Vector Layer", "ogr")

QgsProject.instance().addMapLayer(vector_layer)
<QgsVectorLayer: 'My Vector Layer' (ogr)>
![image](https://github.com/mineozdemmir/Python-Final-Odevi/assets/146944312/95e69ab0-7b4d-42de-88a1-2f187c6d81e3)

En son olarak katman olarak açılan birleştirilmiş dosya seçenekler-semboloji yardımı ile sınıflara ayrılmış ve renkler nüfus aralıklarına göre yeniden düzenlenmiştir.
![image](https://github.com/mineozdemmir/Python-Final-devi/assets/146944312/59450f64-cb4e-44cb-a304-d8f2b18ee7f3)


