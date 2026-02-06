# KWIACIARNIA - APLIKACJA WINDOWS FORMS

mail do wysłania prac: wykonanezadania100@gmail.com

OPIS PROJEKTU:
Aplikacja do obsługi kwiaciarni umożliwiająca przeglądanie oferty bukietów,
dodawanie ich do koszyka oraz składanie zamówień.


# WYMAGANIA WSTĘPNE



1. FOLDER OBRAZKÓW:
   - Utwórz folder "images" w katalogu bin\Debug
   - Dodaj zdjęcia: rumianek.jpg, roza.jpg, tulipan.jpg

2. PODŁĄCZENIE ZDARZEŃ:
   Dla każdej kontrolki podłącz odpowiednie zdarzenie:
   
   a) listBukiety -> SelectedIndexChanged -> listBukiety_SelectedIndexChanged
   b) btnDodaj    -> Click               -> btnDodaj_Click_1
   c) btnUsun     -> Click               -> btnUsun_Click_1
   d) btnZamow    -> Click               -> btnZamow_Click

   Jak podłączyć zdarzenie:
   - Kliknij na kontrolkę (np. listBukiety)
   - W oknie Properties kliknij ikonę błyskawicy (Events)
   - Znajdź odpowiednie zdarzenie (np. SelectedIndexChanged)
   - Wybierz z listy lub wpisz nazwę metody

<img width="1388" height="578" alt="obraz" src="https://github.com/user-attachments/assets/e171ce65-c352-46df-83e8-d0022c84688b" />


<img width="814" height="460" alt="image" src="https://github.com/user-attachments/assets/c581d3a0-8c8e-4455-9e62-ef0da7f35d13" />


<img width="741" height="503" alt="image" src="https://github.com/user-attachments/assets/2e98c1e3-e515-4c7f-b83a-24c9260c77e0" />


# KOD ŹRÓDŁOWY
```

using System;
using System.Collections.Generic;
using System.Drawing;
using System.IO;
using System.Linq;
using System.Windows.Forms;

namespace kw
{
    public partial class Form1 : Form
    {
        private List<Bukiet> oferta = new List<Bukiet>();
        private List<PozycjaKoszyka> koszyk = new List<PozycjaKoszyka>();

        public Form1()
        {
            InitializeComponent();
            string folder = @"C:\Users....!TU_ŚCIEŻKA!...\Debug\images\";

            oferta.Add(new Bukiet
            {
                Nazwa = "Rumianki",
                Cena = 39.99m,
                Opis = "Delikatny bukiet świeżych rumianków",
                SciezkaObrazka = folder + "rumianek.jpg"
            });
            oferta.Add(new Bukiet
            {
                Nazwa = "Róże",
                Cena = 89.99m,
                Opis = "Elegancki bukiet różowych róż",
                SciezkaObrazka = folder + "roza.jpg"
            });
            oferta.Add(new Bukiet
            {
                Nazwa = "Tulipany",
                Cena = 49.99m,
                Opis = "Kolorowy bukiet wiosennych tulipanów",
                SciezkaObrazka = folder + "tulipan.jpg"
            });

            OdswiezListeBukietow();
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }

        private void OdswiezListeBukietow()
        {
            listBukiety.Items.Clear();
            foreach (var b in oferta)
                listBukiety.Items.Add(b);
        }

        private void listBukiety_SelectedIndexChanged(object sender, EventArgs e)
        {
            var bukiet = listBukiety.SelectedItem as Bukiet;
            if (bukiet == null) return;

            lblNazwa.Text = bukiet.Nazwa;
            lblCena.Text = bukiet.Cena.ToString("C");
            lblOpis.Text = bukiet.Opis;

            // Zwolnij stary obrazek
            if (picBukiet.Image != null)
            {
                picBukiet.Image.Dispose();
                picBukiet.Image = null;
            }

            // Załaduj nowy obrazek do pamięci
            if (File.Exists(bukiet.SciezkaObrazka))
            {
                byte[] bytes = File.ReadAllBytes(bukiet.SciezkaObrazka);
                MemoryStream ms = new MemoryStream(bytes);
                picBukiet.Image = Image.FromStream(ms);
            }
        }

        private void OdswiezKoszyk()
        {
            listKoszyk.Items.Clear();
            foreach (var p in koszyk)
                listKoszyk.Items.Add(p);

            decimal suma = koszyk.Sum(p => p.Wartosc);
            lblSuma.Text = "Suma: " + suma.ToString("C");
        }

        private void btnDodaj_Click_1(object sender, EventArgs e)
        {
            var bukiet = listBukiety.SelectedItem as Bukiet;
            if (bukiet == null)
            {
                MessageBox.Show("Wybierz bukiet.");
                return;
            }
            int ilosc = (int)numIlosc.Value;
            koszyk.Add(new PozycjaKoszyka(bukiet, ilosc));
            OdswiezKoszyk();
        }

        private void btnUsun_Click_1(object sender, EventArgs e)
        {
            if (listKoszyk.SelectedIndex < 0)
            {
                MessageBox.Show("Zaznacz element do usunięcia.");
                return;
            }
            koszyk.RemoveAt(listKoszyk.SelectedIndex);
            OdswiezKoszyk();
        }

        private void btnZamow_Click(object sender, EventArgs e)
        {
            if (koszyk.Count == 0)
            {
                MessageBox.Show("Koszyk jest pusty.");
                return;
            }

            string zamowienie = "ZAMÓWIENIE\n\n";
            foreach (var p in koszyk)
                zamowienie += p.ToString() + "\n";

            zamowienie += "\n" + lblSuma.Text;

            MessageBox.Show(zamowienie, "Potwierdzenie zamówienia");
        }
    }

    public class Bukiet
    {
        public string Nazwa { get; set; }
        public decimal Cena { get; set; }
        public string Opis { get; set; }
        public string SciezkaObrazka { get; set; }

        public override string ToString()
        {
            return Nazwa + " (" + Cena.ToString("C") + ")";
        }
    }

    public class PozycjaKoszyka
    {
        public Bukiet Bukiet { get; set; }
        public int Ilosc { get; set; }

        public decimal Wartosc
        {
            get { return Bukiet.Cena * Ilosc; }
        }

        public PozycjaKoszyka(Bukiet b, int ilosc)
        {
            Bukiet = b;
            Ilosc = ilosc;
        }

        public override string ToString()
        {
            return Bukiet.Nazwa + " x" + Ilosc + " = " + Wartosc.ToString("C");
        }
    }
}


```
# ZADANIA DO WYKONANIA


# ZADANIE 1: DODAJ 3 NOWE BUKIETY
--------------------------------------------------------------------------------

Dodaj 3 nowe bukiety do oferty w konstruktorze Form1().

Wzór:
    oferta.Add(new Bukiet
    {
        Nazwa = "...",
        Cena = ...m,
        Opis = "...",
        SciezkaObrazka = folder + "....jpg"
    });

Przykładowe bukiety do dodania:
- Słoneczniki
- Goździki
- Lilie



# ZADANIE 2: DODAJ ZDJĘCIA I OPISY
--------------------------------------------------------------------------------

Dla każdego nowego bukietu:

1. Znajdź zdjęcie kwiatu w internecie
2. Zapisz je w folderze bin\Debug\images\ z odpowiednią nazwą (np. slonecznik.jpg)
3. Dodaj opis bukietu (pole Opis)

Przykład opisu: "Piękny bukiet słoneczników rozjaśni każde wnętrze"




ZADANIE EXTRA: OBLICZANIE VAT
--------------------------------------------------------------------------------

Dodaj obliczanie podatku VAT (23%) do sumy zamówienia.

Kroki:
1. Na początku klasy dodaj stałą:
   private const decimal STAWKA_VAT = 0.23m;

2. W metodzie
   ```
   OdswiezKoszyk() zmień obliczanie sumy:
   decimal sumaNetto = koszyk.Sum(p => p.Wartosc);
   decimal vat = sumaNetto * STAWKA_VAT;
   decimal sumaBrutto = sumaNetto + vat;
   lblSuma.Text = "Netto: " + sumaNetto.ToString("C") + 
                  " + VAT: " + vat.ToString("C") + 
                  " = " + sumaBrutto.ToString("C");
   ```

# KONTROLKI NA FORMULARZU


KONTROLKA          | NAZWA              | OPIS
-------------------|--------------------|----------------------------------
ListBox            | listBukiety        | Lista dostępnych bukietów
ListBox            | listKoszyk         | Zawartość koszyka
Label              | lblNazwa           | Nazwa wybranego bukietu
Label              | lblCena            | Cena wybranego bukietu
Label              | lblOpis            | Opis wybranego bukietu
Label              | lblSuma            | Suma zamówienia
PictureBox         | picBukiet          | Zdjęcie bukietu
NumericUpDown      | numIlosc           | Ilość sztuk
Button             | btnDodaj           | Dodaj do koszyka
Button             | btnUsun            | Usuń z koszyka
Button             | btnZamow           | Złóż zamówienie


