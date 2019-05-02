##### Proprocessing

```
#vsm
factory = StopWordRemoverFactory()
stopword = factory.create_stop_word_remover ()

factory = StemmerFactory ()
stemmer = factory.create_stemmer ()

tmp = ''
for i in isi:
    tmp = tmp + ' ' +i

hasil = []
for i in tmp.split():
    if i.isalpha() and not i in hasil:

# Menghilangkan Kata tidak penting

​        stop = stopword.remove(i)
​        stem = stemmer.stem(stop)
​        hasil.append((stem  + ''))
print('menghilangkan kata tidak penting', hasil)
katadasar = hasil

#KBBI
koneksi = sqlite3.connect('KBI.db')
cur_kbi = koneksi.execute("SELECT* from KATA")
    
def LinearSearch (kbi,kata):
    found=False
    posisi=0
    while posisi < len (kata) and not found :
        if kata[posisi]==kbi:
            found=True
        posisi=posisi+1
    return found
berhasil=[]
for kata in cur_kbi :
    ketemu=LinearSearch(kata[0],katadasar)
    if ketemu :
        kata = kata[0]
        berhasil.append(kata)
print('hasil KBI = ',berhasil)
katadasar = np.array(berhasil)

#(katadasar)
matrix = []
for row in isi :
    tamp_isi=[]
    for a in katadasar:
        tamp_isi.append(row.lower().count(a))
    matrix.append(tamp_isi)
    
with open ('data_matrix.csv', newline='', mode='w')as employee_file :
    employee_writer = csv.writer(employee_file, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)
    employee_writer.writerow(katadasar)
    for i in matrix :
        employee_writer.writerow(i)

#tf-idf
df = list()
for d in range (len(matrix[0])):
    total = 0
    for i in range(len(matrix)):
        if matrix[i][d] !=0:
            total += 1
    df.append(total)

idf = list()
for i in df:
    tmp = 1 + log10(len(matrix)/(1+i))
    idf.append(tmp)

tf = matrix
tfidf = []
for baris in range(len(matrix)):
    tampungBaris = []
    for kolom in range(len(matrix[0])):
        tmp = tf[baris][kolom] * idf[kolom]
        tampungBaris.append(tmp)
    tfidf.append(tampungBaris)

with open('tf-idf.csv', newline='', mode='w') as employee_file:
    employee_writer = csv.writer(employee_file, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)
    employee_writer.writerow(katadasar)
    for i in tfidf:
        employee_writer.writerow(i)
```




###### Penjelasan Code Pre Processing :

```
factory = StopWordRemoverFactory()
stopword = factory.create_stop_word_remover ()

factory = StemmerFactory ()
stemmer = factory.create_stemmer ()

tmp = ''
for i in isi:
    tmp = tmp + ' ' +i

hasil = []
for i in tmp.split():
    if i.isalpha() and not i in hasil:
        # Menghilangkan Kata tidak penting
        stop = stopword.remove(i)
        stem = stemmer.stem(stop)
        hasil.append((stem  + ''))
print('menghilangkan kata tidak penting', hasil)
katadasar = hasil
```

PreProcessing yang pertama adalah Vector Space Model (VSM) 

pre processing pada VSM dengan cara menghitung setiap kata pada setiap dokumen.

pada code (topWordRemoverFactory) ini juga akan menghapus kata yang tidak penting seperti "dan","di","atau".

------

```
koneksi = sqlite3.connect('KBI.db')
cur_kbi = koneksi.execute("SELECT* from KATA")
    
def LinearSearch (kbi,kata):
    found=False
    posisi=0
    while posisi < len (kata) and not found :
        if kata[posisi]==kbi:
            found=True
        posisi=posisi+1
    return found
berhasil=[]
for kata in cur_kbi :
    ketemu=LinearSearch(kata[0],katadasar)
    if ketemu :
        kata = kata[0]
        berhasil.append(kata)
print('hasil KBI = ',berhasil)
katadasar = np.array(berhasil)
```

pada code ini digunakan untuk menseleksi kata yang tidak baku atau yang tidak ada dalam Kamus Besar Bahasa Indonesia (KBBI) 

dengan mengambil data kata yang ada di database KBI.db.

------

```
matrix = []
for row in isi :
    tamp_isi=[]
    for a in katadasar:
        tamp_isi.append(row.lower().count(a))
    matrix.append(tamp_isi)


with open ('data_matrix.csv', newline='', mode='w')as employee_file :
    employee_writer = csv.writer(employee_file, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)
    employee_writer.writerow(katadasar)
    for i in matrix :
        employee_writer.writerow(i)
```

pada code ini berfungsi untuk menghitung katadasar disetiap dkoumen/artikel dan akan menuliskannya pada file data_matrix.csv

------

```
df = list()
for d in range (len(matrix[0])):
    total = 0
    for i in range(len(matrix)):
        if matrix[i][d] !=0:
            total += 1
    df.append(total)

idf = list()
for i in df:
    tmp = 1 + log10(len(matrix)/(1+i))
    idf.append(tmp)

tf = matrix
tfidf = []
for baris in range(len(matrix)):
    tampungBaris = []
    for kolom in range(len(matrix[0])):
        tmp = tf[baris][kolom] * idf[kolom]
        tampungBaris.append(tmp)
    tfidf.append(tampungBaris)


with open('tf-idf.csv', newline='', mode='w') as employee_file:
    employee_writer = csv.writer(employee_file, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)
    employee_writer.writerow(katadasar)
    for i in tfidf:
        employee_writer.writerow(i)
```

tf-idf merupakan perhitungan antara tf dan idf

tf yang telah kita cari dengan cara menghitung katadasar dengan variabel 'matriks' yang bertujuan untuk menghitung berapa banyak kata disetiap dokumen/artiker

sedangkan idf bertujuan untuk menghitung ada berapa kata tersebut disetiap dokumen/artikelnya.

rumus tf-idf sendiri 'tmp = tf[baris][kolom] * idf[kolom]'

dan hasilnya akan ditulis pada file tf-idf.csv



