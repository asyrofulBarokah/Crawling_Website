##### Clustering

```
for i in range(2, len(fiturBaru)-1):
    kmeans = KMeans(n_clusters=5, random_state=0).fit(fiturBaru);
print('kmeans = ',kmeans.labels_)
classnya = kmeans.labels_
s_avg = silhouette_score(fiturBaru, classnya, random_state=0)
print ('silhouette = ', s_avg)
print("silhouette untuk",i,"cluster :",s_avg)
print(kmeans.labels_)
```

Metode **K-Means Clustering** berusaha mengelompokkan data yang ada ke dalam beberapa kelompok, dimana data dalam satu kelompok mempunyai karakteristik yang sama satu sama lainnya dan mempunyai karakteristik yang berbeda dengan data yang ada di dalam kelompok yang lain.

Code di atas adalah code untuk melakukan clustering dengan menggunakan metode kmeans. Pada contoh ini cluster dibagi menjadi 5. Banyak cluster bisa diubah sesuai kebutuhan.

**Silhouette** dilakukan untuk mengetahui seberapa dekat relasi antara objek dalam sebuah cluster dan seberapa jauh sebuah cluster terpisah dengan cluster lain, gabungan dari dua metode yaitu metode cohesion yang berfungsi untuk mengukur seberapa dekat relasi antara objek dalam sebuah cluster, dan metode separation yang berfungsi untuk mengukur seberapa jauh sebuah cluster terpisah dengan cluster lain.

