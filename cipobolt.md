# 🥾 Cipő Nyilvántartó – WPF alkalmazás

Ez az alkalmazás egy **Windows Presentation Foundation (WPF)** alapú rendszer, amely cipők és vásárlók adatainak kezelésére szolgál. Lehetőség van új adatokat hozzáadni, menteni, betölteni, valamint kapcsolatot létrehozni cipők és vásárlók között.

---

## 🎨 XAML (Felhasználói felület)

A felhasználói felületet **XAML (Extensible Application Markup Language)** segítségével írjuk le. Itt történik az elemek elrendezése, stílusozása és az interakciók (gombok, szövegdobozok stb.) meghatározása.

### 🧱 Alapbeállítások

A `Window` elem tartalmazza az ablak beállításait:
- `Title="Cipő Nyilvántartó"` → az ablak címe.
- `Height`, `Width` → az ablak méretei.
- `FontFamily="Segoe UI"` → az alapértelmezett betűtípus.
- `WindowStartupLocation="CenterScreen"` → indításkor középre igazítja az ablakot.

### 🎨 Stílusok (Style)

A `Window.Resources` szekcióban előre beállított stílusokat definiálunk:
- `TabItem`, `TextBox`, `Button`, `ListView`, `TextBlock`, stb. → ezekhez szabványos margókat, betűtípust, színeket rendelünk.

Ezek segítik az egységes megjelenést.

### 🗂 TabControl – Fülek

A `TabControl` vezérlő több „fület” jelenít meg:

1. **Cipők** – új cipő hozzáadása, megtekintése
2. **Vásárlók** – új vásárló rögzítése
3. **Összekötés** – cipő és vásárló összerendelése
4. **Kereső** – vásárlóhoz kapcsolt cipők listázása

### 📦 Vezérlők és működés

- `StackPanel`, `Grid` → elrendezésre szolgáló konténerek
- `TextBox`, `TextBlock`, `ComboBox`, `ListView`, `ListBox` → felhasználói interakciókhoz
- `Button` → műveletek indítása (pl. Hozzáadás, Összekapcsolás)

---

## 🧠 C# Kódlogika – MainWindow.xaml.cs

Az alkalmazás eseményeit és adatait itt kezeljük.

### 🥾 AddCipo_Click

Cipő hozzáadását végzi:
- Ellenőrzi, hogy méret és ár szám.
- Új `Cipo` objektumot hoz létre.
- Hozzáadja a listához és a vizuális elemekhez (`ListView`, `ListBox`).
- Mentést végez `cipok.txt` fájlba.

### 👤 AddFelhasznalo_Click

Vásárló felvitel:
- Ellenőrzi, hogy születési év és telefonszám szám.
- Új `Felhasznalo` példány készül.
- Hozzáadás listához és mentés `felhasznalok.txt` fájlba.

### 🔗 Osszekotes_Click

- Ha ki van választva egy cipő és egy vásárló, összeköti őket.
- Új `Kapcsolat` objektum jön létre, majd menti `kapcsolatok.txt` fájlba.

### 💾 BetoltCipok / BetoltFelhasznalok / BetoltKapcsolatok

- Beolvassák a korábban elmentett adatokat szövegfájlokból.
- Az adatok feldolgozás után megjelennek a felületen.

### 💽 MentCipok / MentFelhasznalok / MentKapcsolatok

- Fájlba mentik a lista elemeit.
- A sorok mezőit pontosvessző választja el (`;`), hogy könnyen visszaolvashatók legyenek.

### 🔍 KeresesFelhasznaloComboBox_SelectionChanged

- Vásárló kiválasztásakor listázza azokat a cipőket, amelyeket az adott vásárlóhoz rendeltünk.
- A `kapcsolatok` listát szűri és a kapcsolódó `Cipo` objektumokat adja vissza.

---

## 🧾 Adattípusok

### `Cipo` osztály

```csharp
public class Cipo
{
    public int Id { get; set; }
    public string Marka { get; set; }
    public string Meret { get; set; }
    public string Szin { get; set; }
    public string Ar { get; set; }

    public override string ToString() => $"[{Id}] {Marka} - {Meret} - {Szin} - {Ar} Ft";
}
