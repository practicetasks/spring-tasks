# Квиз

Выберите, какие последовательности команд можно использовать, чтобы выполнить следующие действия. 

1. Перейти в корневую директорию.
2. Создать в ней две новые папки — `empty_folder` и `file_folder`.
3. Проверить, что директории появились — отобразить список объектов.
4. В директории `file_folder` создать файлы `super_file_1.txt` и `super_file_2.txt`.
5. Проверить, появились ли эти файлы.

**a)** 
```shell
cd /
touch empty_folder
touch file_folder
ls
cd file_folder/
mkdir super_file_1.txt super_file_2.txt
ls
```


**b)**
```shell
cd /
mkdir empty_folder 
mkdir file_folder
ls
cd file_folder/
touch super_file_1.txt super_file_2.txt
ls
```

**c)**
```shell
mkdir empty_folder
mkdir file_folder
ls
touch file_folder/super_file_1.txt 
touch file_folder/super_file_2.txt
ls
```

**d)**
```shell
cd /
mkdir empty_folder
mkdir file_folder
ls
touch file_folder/super_file_1.txt
touch file_folder/super_file_2.txt
ls file_folder/
```