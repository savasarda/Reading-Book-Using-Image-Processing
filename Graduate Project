from PyQt5 import QtWidgets, QtSql, QtCore
from PyQt5.QtWidgets import QWidget, QLabel, QPushButton, QVBoxLayout, QRadioButton
import sqlite3
import time
import random
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
import os
import sys
import pytesseract
import cv2 as cv
from gtts import gTTS
from typing import Tuple, Union
import numpy as np
import math


class Pencere(QtWidgets.QWidget):

    def __init__(self):

        super().__init__()
        self.baglanti_olustur()
        self.init_ui()

    def baglanti_olustur(self):

        self.baglanti = sqlite3.connect("Bilgiler.db", timeout=10)
        self.cursor = self.baglanti.cursor()
        self.cursor.execute(
            "Create Table If not exists üyeler (kullanıcı_adı TEXT,parola TEXT,Ad TEXT,Soyad TEXT, Mail TEXT)")
        self.baglanti.commit()

    def init_ui(self):

        self.kullanici_adi = QtWidgets.QLineEdit()
        self.parola = QtWidgets.QLineEdit()
        self.parola.setEchoMode(QtWidgets.QLineEdit.Password)
        self.unuttum = QtWidgets.QPushButton("Şifremi Unuttum!")
        self.giris = QtWidgets.QPushButton("Giriş")
        self.kaydol = QtWidgets.QPushButton("Kaydol")
        self.kullanici = QtWidgets.QLabel("Kullanıcı Adı :")
        self.sifre = QtWidgets.QLabel("Şifre            :")
        self.hatirlat = QtWidgets.QCheckBox("Hatırla")
        self.setWindowTitle("Kullanıcı Girişi")
        self.yazi_alani = QtWidgets.QLabel("")

        h_box = QtWidgets.QHBoxLayout()
        h_box.addStretch()
        h_box.addWidget(self.kullanici)
        h_box.addWidget(self.kullanici_adi)
        h_box.addStretch()

        h_box2 = QtWidgets.QHBoxLayout()
        h_box2.addStretch()
        h_box2.addWidget(self.sifre)
        h_box2.addWidget(self.parola)
        h_box2.addStretch()

        h_box5 = QtWidgets.QHBoxLayout()
        h_box5.addStretch()
        h_box5.addWidget(self.yazi_alani)
        h_box5.addStretch()

        h_box3 = QtWidgets.QHBoxLayout()
        h_box3.addStretch()
        h_box3.addWidget(self.hatirlat)
        h_box3.addWidget(self.unuttum)
        h_box3.addStretch()

        h_box4 = QtWidgets.QHBoxLayout()
        h_box4.addStretch()
        h_box4.addWidget(self.giris)
        h_box4.addWidget(self.kaydol)

        v_box = QtWidgets.QVBoxLayout()
        v_box.addLayout(h_box)
        v_box.addLayout(h_box2)
        v_box.addLayout(h_box5)
        v_box.addLayout(h_box3)
        v_box.addStretch()
        v_box.addLayout(h_box4)

        self.kullanici.setFixedSize(60, 20)
        self.kullanici_adi.setFixedSize(120, 20)
        self.sifre.setFixedSize(60, 20)
        self.parola.setFixedSize(120, 20)

        self.setLayout(v_box)

        self.setGeometry(800, 300, 300, 300)
        self.setFixedSize(300, 300)

        self.show()

        self.unuttum.clicked.connect(self.yenile)
        self.giris.clicked.connect(self.sign)
        self.kaydol.clicked.connect(self.kayit)

    def yenile(self):

        pencere2.show()

    def kayit(self):

        pencere3.show()

    def sign(self):

        ad = self.kullanici_adi.text()
        parol = self.parola.text()

        self.cursor.execute("Select * From üyeler Where kullanıcı_adı=? and parola=?", (ad, parol))

        data = self.cursor.fetchall()

        if len(data) == 0:

            self.yazi_alani.setText("Kullanıcı Adı veya Parola Hatalı...")

        else:

            self.yazi_alani.setText("Hoş Geldiniz.")
            self.close()
            self.pencere4 = Language()


class Pencere2(QtWidgets.QWidget):

    def __init__(self):

        super().__init__()
        self.gorsel()

    def gorsel(self):

        self.email = QtWidgets.QLabel("E-Mail :")
        self.mail = QtWidgets.QLineEdit()
        self.gonder = QtWidgets.QPushButton("Gönder")
        self.yazi_alani = QtWidgets.QLabel("")

        h_box = QtWidgets.QHBoxLayout()
        h_box.addStretch()
        h_box.addWidget(self.email)
        h_box.addWidget(self.mail)
        h_box.addStretch()

        h_box3 = QtWidgets.QHBoxLayout()
        h_box3.addStretch()
        h_box3.addWidget(self.yazi_alani)
        h_box3.addStretch()

        h_box2 = QtWidgets.QHBoxLayout()
        h_box2.addStretch()
        h_box2.addWidget(self.gonder)

        v_box = QtWidgets.QVBoxLayout()
        v_box.addStretch()
        v_box.addLayout(h_box)
        v_box.addLayout(h_box3)
        v_box.addLayout(h_box2)
        v_box.addStretch()

        self.setLayout(v_box)

        self.setWindowTitle("Şifre Yenileme")

        self.gonder.clicked.connect(self.yolla)

    def yolla(self):

        self.sifre = str(random.randint(1000, 9999))
        self.mai = self.mail.text()
        mesaj = MIMEMultipart()
        mesaj["From"] = "ardasavas6@gmail.com"
        mesaj["To"] = self.mai
        mesaj["Subject"] = "Şifre Yenileme"
        yazı = "Mail Şifresi : "
        mesaj_gövdesi = MIMEText(yazı + self.sifre, "plain")

        mesaj.attach(mesaj_gövdesi)

        try:
            mail = smtplib.SMTP("smtp.gmail.com", 587)
            mail.ehlo()
            mail.starttls()
            mail.login("ardasavas6@gmail.com", "ardasavasfb98")
            mail.sendmail(mesaj["From"], mesaj["To"], mesaj.as_string())
            self.yazi_alani.setText("Şifre Maile Gönderildi.")
            mail.close()

        except:
            self.yazi_alani.setText("Mail Gönderilemedi.")

        self.baglanti = sqlite3.connect("Bilgiler.db", timeout=10)
        self.cursor = self.baglanti.cursor()
        self.cursor.execute("Update üyeler set parola=? where Mail=?", (self.sifre, self.mai))
        self.baglanti.commit()


class Pencere3(QtWidgets.QWidget):

    def __init__(self):
        super().__init__()
        self.gorsel()

    def gorsel(self):

        self.ad = QtWidgets.QLabel("İsim             :")
        self.soyad = QtWidgets.QLabel("Soyisim        :")
        self.kullanici_adi = QtWidgets.QLabel("Kullanıcı Adı :")
        self.email = QtWidgets.QLabel("E-Mail          :")
        self.sifre = QtWidgets.QLabel("Şifre            :")
        self.isim = QtWidgets.QLineEdit()
        self.soy = QtWidgets.QLineEdit()
        self.kullanici = QtWidgets.QLineEdit()
        self.mail = QtWidgets.QLineEdit()
        self.parola = QtWidgets.QLineEdit()
        self.kaydol = QtWidgets.QPushButton("Kaydet!")
        self.yazi_alani = QtWidgets.QLabel("")

        h_box = QtWidgets.QHBoxLayout()
        h_box.addStretch()
        h_box.addWidget(self.ad)
        h_box.addWidget(self.isim)
        h_box.addStretch()

        h_box2 = QtWidgets.QHBoxLayout()
        h_box2.addStretch()
        h_box2.addWidget(self.soyad)
        h_box2.addWidget(self.soy)
        h_box2.addStretch()

        h_box3 = QtWidgets.QHBoxLayout()
        h_box3.addStretch()
        h_box3.addWidget(self.kullanici_adi)
        h_box3.addWidget(self.kullanici)
        h_box3.addStretch()

        h_box4 = QtWidgets.QHBoxLayout()
        h_box4.addStretch()
        h_box4.addWidget(self.email)
        h_box4.addWidget(self.mail)
        h_box4.addStretch()

        h_box5 = QtWidgets.QHBoxLayout()
        h_box5.addStretch()
        h_box5.addWidget(self.sifre)
        h_box5.addWidget(self.parola)
        h_box5.addStretch()

        h_box7 = QtWidgets.QHBoxLayout()
        h_box7.addStretch()
        h_box7.addWidget(self.yazi_alani)
        h_box7.addStretch()

        h_box6 = QtWidgets.QHBoxLayout()
        h_box6.addStretch()
        h_box6.addWidget(self.kaydol)

        v_box = QtWidgets.QVBoxLayout()
        v_box.addLayout(h_box)
        v_box.addLayout(h_box2)
        v_box.addLayout(h_box3)
        v_box.addLayout(h_box4)
        v_box.addLayout(h_box5)
        v_box.addStretch()
        v_box.addLayout(h_box7)
        v_box.addStretch()
        v_box.addStretch()
        v_box.addLayout(h_box6)

        self.setLayout(v_box)

        self.ad.setFixedSize(65, 20)
        self.isim.setFixedSize(120, 20)
        self.soyad.setFixedSize(65, 20)
        self.soy.setFixedSize(120, 20)
        self.kullanici_adi.setFixedSize(65, 20)
        self.kullanici.setFixedSize(120, 20)
        self.email.setFixedSize(65, 20)
        self.mail.setFixedSize(120, 20)
        self.sifre.setFixedSize(65, 20)
        self.parola.setFixedSize(120, 20)

        self.setWindowTitle("           KAYDOL            ")
        self.setFixedSize(300, 300)

        self.kaydol.clicked.connect(self.yeni)

    def yeni(self):

        ad = self.isim.text()
        soyad = self.soy.text()
        kullan = self.kullanici.text()
        parol = self.parola.text()
        mai = self.mail.text()

        self.im = sqlite3.connect("Bilgiler.db", timeout=10)
        self.curs = self.im.cursor()
        self.curs.execute("Select * From üyeler where kullanıcı_adı=?", (kullan,))
        date = self.curs.fetchall()

        if len(date) == 0:
            self.yazi_alani.setText("Bilgileriniz Kaydedildi.")
            self.curs.execute("Insert Into üyeler Values (?,?,?,?,?)", (kullan, parol, ad, soyad, mai))
            self.im.commit()

        else:

            self.yazi_alani.setText("Kullanıcı Adı Alınmış.")

class Language(QWidget):  #LANGUAGE
    def __init__(self):
        super().__init__()
        self.init_ui()
    def init_ui(self):
        self.yazi_alani = QLabel("Select a Language")
        self.buton1 = QRadioButton("Turkish")
        self.buton2 = QRadioButton("English")
        self.buton3 = QRadioButton("German")
        self.buton4 = QRadioButton("French")
        self.sonuc = QLabel("")
        self.buton = QPushButton("Select!")

        v_box = QVBoxLayout()
        v_box.addWidget(self.yazi_alani)
        v_box.addStretch()
        v_box.addWidget(self.buton1)
        v_box.addWidget(self.buton2)
        v_box.addWidget(self.buton3)
        v_box.addWidget(self.buton4)
        v_box.addStretch()
        v_box.addWidget(self.sonuc)
        v_box.addStretch()
        v_box.addWidget(self.buton)

        self.setLayout(v_box)

        self.buton.clicked.connect(self.click)
        self.setGeometry(800, 300, 300, 300)
        self.setFixedSize(300, 300)
        self.show()

    def click(self):
        tr = self.buton1.isChecked()
        en = self.buton2.isChecked()
        de = self.buton3.isChecked()
        fr = self.buton4.isChecked()
        global qwe
        if (tr == True):
            print("Turkish language is selected")
            qwe = "tr"
            self.close()
            proje_eee()

        elif (en == True):
            print("English language is selected")
            qwe = "en"
            self.close()
            proje_eee()

        elif (de == True):
            print("German language is selected")
            qwe = "de"
            self.close()
            proje_eee()

        elif (fr == True):
            print("French language is selected")
            qwe = "fr"
            self.close()
            proje_eee()

class proje_eee():
    def __init__(self):

        super().__init__()
        self.run()


    def rescaleframe(self,frame,scale):   #Çerçeve büyüklüğü ayarlama
        height = int(frame.shape[0] * scale)
        widht = int(frame.shape[1] * scale)
        dimensions = (widht,height)
        return cv.resize(frame,dimensions,interpolation=cv.INTER_AREA)

    def rotate(self,
            image: np.ndarray, angle: float, background: Union[int, Tuple[int, int, int]]
    ) -> np.ndarray:
        old_width, old_height = image.shape[:2]
        angle_radian = math.radians(angle)
        width = abs(np.sin(angle_radian) * old_height) + abs(np.cos(angle_radian) * old_width)
        height = abs(np.sin(angle_radian) * old_width) + abs(np.cos(angle_radian) * old_height)

        image_center = tuple(np.array(image.shape[1::-1]) / 2)
        rot_mat = cv.getRotationMatrix2D(image_center, angle, 1.0)
        rot_mat[1, 2] += (width - old_width) / 2
        rot_mat[0, 2] += (height - old_height) / 2
        return cv.warpAffine(image, rot_mat, (int(round(height)), int(round(width))), borderValue=background)

    def run(self):
        # # --------------------LANGUAGE--------------------------
        # dil = int(input("Please select a language.!:\n"
        #                 "1.Turkish\n"
        #                 "2.English\n"
        #                 "3.German\n"
        #                 "4.French\n"))
        # if dil == 1:
        #     self.language = "tr"
        # elif dil == 2:
        #     self.language = "en"
        # elif dil == 3:
        #     self.language = "de"
        # elif dil == 4:
        #     self.language = "fr"

        pytesseract.pytesseract.tesseract_cmd = "C:\\Program Files\\Tesseract-OCR\\tesseract.exe"
        url = "https://192.168.0.13:8080/video"
        #url=0
        #url = "http://10.30.46.83:8080/video"

        capture = cv.VideoCapture(url)
        while True:
            isTrue,frame =capture.read()
            #cv.imshow("Video",frame)
            resized = self.rescaleframe(frame,scale=0.5)
            rotated_frame = self.rotate(resized, -90, None)
            cv.imshow("Resized",rotated_frame)


            if cv.waitKey(20) & 0XFF == ord("q"):  #pencereyi kapatır.
                sys.exit()
            elif cv.waitKey(20) & 0XFF == ord("a"):
                rotated = self.rotate(resized, -90, (0, 0, 0))   #rotated yaparsan 38.satıra frame yerine rotated yaz!!!
                cv.imwrite("capture.jpg", rotated)   #Anlık yakalar.
                break

        capture.release()
        cv.destroyAllWindows()
#-----------------------------------------------------
        captured_image = cv.imread("capture.jpg")
        capture_image_resized = self.rescaleframe(captured_image,scale=1)
        cv.imshow("captured_image",capture_image_resized)


        while True:
            if cv.waitKey(20) & 0XFF == ord("s"):  # pencereyi kapatır.
                cv.destroyAllWindows()
                break

        gray = cv.cvtColor(capture_image_resized,cv.COLOR_BGR2GRAY)
        cv.imshow("captured_image",capture_image_resized)
        cv.imshow("Gray",gray)

        while True:
            if cv.waitKey(20) & 0XFF == ord("d"):  # pencereyi kapatır.
                cv.destroyAllWindows()
                break
        ##########################################
        kernel = np.ones((2,2),np.uint8)
        eroded = cv.erode(gray,kernel,iterations = 1)
        cv.imshow("captured_image",capture_image_resized)
        cv.imshow("Gray",gray)
        cv.imshow("Thicken Letters",eroded)
        cv.imwrite("Thicken Letters.jpg",eroded)

        while True:
            if cv.waitKey(20) & 0XFF == ord("f"):  # pencereyi kapatır.
                cv.destroyAllWindows()
                break

        ##################################
        rgb_planes = cv.split(eroded)

        result_planes = []
        result_norm_planes = []

        for plane in rgb_planes:
            dilated_img = cv.dilate(plane, np.ones((7,7), np.uint8))
            bg_img = cv.medianBlur(dilated_img, 21)
            diff_img = 255 - cv.absdiff(plane, bg_img)
            norm_img = cv.normalize(diff_img,None, alpha=0,\
                                     beta=255, norm_type=cv.NORM_MINMAX,\
                                     dtype=cv.CV_8UC1)
            result_planes.append(diff_img)
            result_norm_planes.append(norm_img)

        result = cv.merge(result_planes)
        result_norm = cv.merge(result_norm_planes)

        cv.imwrite('shadows_out.png', result)
        cv.imwrite('shadows_out_norm.png', result_norm)

        cv.imshow('shadows_out', result)
        cv.imshow('shadows_out_norm', result_norm)

        while True:
            if cv.waitKey(20) & 0XFF == ord("g"):  # pencereyi kapatır.
                cv.destroyAllWindows()
                break



        self.text = pytesseract.image_to_string(("shadows_out_norm.png"),lang="tur")
        print(self.text)

        with open("text.txt", "w", encoding="utf-8") as file:
            file.write(self.text)

        output= gTTS (text=self.text,lang = qwe ,slow=False)  #hız ayarını dene
        output.save("output.mp3")
        os.system("start output.mp3")
        os.system("text.txt")

if __name__ == '__main__':
    uygulama = QtWidgets.QApplication(sys.argv)

    pencere = Pencere()
    pencere2 = Pencere2()
    pencere3 = Pencere3()
    sys.exit(uygulama.exec_())


