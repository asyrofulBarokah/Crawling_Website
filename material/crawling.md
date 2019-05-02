##### Crawling

```
src = "https://www.jualmotorbekas.com/"

page = requests.get(src)

soup = BeautifulSoup(page.content, 'html.parser')

bagan = soup.findAll(class_='item-header')

koneksi = sqlite3.connect('DB_motor.db')
koneksi.execute(''' CREATE TABLE if not exists jualMotorBekas
            (judul TEXT NOT NULL,
             isi TEXT NOT NULL);''')

for i in range(len(bagan)):
    link = bagan[i].find('a')['href']
    page = requests.get(link)
    soup = BeautifulSoup(page.content, 'html.parser')
    judul = soup.find(class_='single-listing').getText()
    isi = soup.find(class_='single-main')
    paragraf = isi.findAll('p')
    p = ''
    for s in paragraf:
        p+=s.getText() +' '
        

​```
koneksi.execute('INSERT INTO jualMotorBekas values (?,?)', (judul, p));
​```

koneksi.commit()
tampil = koneksi.execute("SELECT * FROM jualMotorBekas")
with open ('data_crawler.csv', newline='', mode='w')as employee_file :
    employee_writer = csv.writer(employee_file, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)

​```
for i in tampil:
    employee_writer.writerow(i)
​```

tampil = koneksi.execute("SELECT * FROM jualMotorBekas")
isi = []
for row in tampil:
    isi.append(row[1])
```



###### penjelasan code crawling data :

```
src = "https://www.jualmotorbekas.com/"

page = requests.get(src)

soup = BeautifulSoup(page.content, 'html.parser')

bagan = soup.findAll(class_='item-header')
```

Code diatas adalah code yang menyambungkan dengan website www.jualmotorbekas.com  class yang akan di ambil datanya adalah class 'item-header'.

saya mengambil contoh web dari jual motor bekas.

------

```
koneksi = sqlite3.connect('DB_motor.db')
koneksi.execute(''' CREATE TABLE if not exists jualMotorBekas
            (judul TEXT NOT NULL,
             isi TEXT NOT NULL);''')

```

Code diatas merupakan code yang berfungsi untuk membuat database yang bernama 'DB_motor.db' dan dibuat juga table yang bernama 'jualMotorBekas' dengan 2 kolom yang bernama 'judul' dan 'isi'.

------

```
for i in range(len(bagan)):
    link = bagan[i].find('a')['href']
    page = requests.get(link)
    soup = BeautifulSoup(page.content, 'html.parser')
    judul = soup.find(class_='single-listing').getText()
    isi = soup.find(class_='single-main')
    paragraf = isi.findAll('p')
    p = ''
    for s in paragraf:
        p+=s.getText() +' '
        
koneksi.execute('INSERT INTO jualMotorBekas values (?,?)', (judul, p));
```

Code diatas memiliki fungsi jika "bagan = soup.findAll(class_='item-header')" diklik akan menuju ke alamat setiap motor bekas yang dijual. 

mengambil data dari class 'single-listing' yang berisi judul/header motor yang dijual. dan akan mengambil data dari class 'single-main' yang berisi isi/deskripsi dari motor yang dijual.

paragraf berfungsi untuk mengambil data sesuai dengan bentuk paragraf.

dan akan diinsertkan/dimasukkan data yang di ambil ke dalam table jualMotorBekas berdasarkan kolom (judul,p).

------

```
koneksi.commit()
tampil = koneksi.execute("SELECT * FROM jualMotorBekas")
with open ('data_crawler.csv', newline='', mode='w')as employee_file :
    employee_writer = csv.writer(employee_file, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)

​```
for i in tampil:
    employee_writer.writerow(i)
​```
```

dari data yang telah diambil akan dimasukkan/ditulis kedalam bentuk csv dengan nama data_crawler.csv.