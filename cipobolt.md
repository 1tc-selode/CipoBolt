# Cipő Nyilvántartó – WPF alkalmazás

Ez az alkalmazás egy **Windows Presentation Foundation (WPF)** alapú rendszer, amely cipők és vásárlók adatainak kezelésére szolgál. Lehetőség van új adatokat hozzáadni, menteni, betölteni, valamint kapcsolatot létrehozni cipők és vásárlók között.

---

## Felhasználói Felület

A felhasználói felület a következő főbb elemekből áll:

- **Window.Resources**: Ebben a szakaszban előre beállított stílusokat (Style) definiálunk, amelyek egységes megjelenést adnak bizonyos elemeknek (például gomboknak, szövegdobozoknak stb.). Ezek a stílusok globálisan érvényesek az egész ablakon belül.

- **Grid**: Az ablak fő tartalmi konténere, amelyben egyetlen `TabControl` található. A `Grid` olyan elrendezési elem, amely sorok és oszlopok mentén rendezi el a belső vezérlőket.

- **TabControl**: Olyan vezérlőelem, amely lehetővé teszi, hogy több különböző "lapot" vagy "fület" jelenítsünk meg egymás mellett. Itt négy `TabItem`-et használunk:
  - **Cipők** fül: cipők hozzáadása és listázása
  - **Vásárlók** fül: felhasználók hozzáadása és megjelenítése
  - **Összekötés** fül: cipők és vásárlók összekapcsolása
  - **Kereső** fül: vásárló kiválasztása után a hozzá tartozó cipők listázása

### TabItem – Cipők fül

- **StackPanel**: Függőleges elrendezésű konténer, amiben két fő rész található:
  1. **Adatbeviteli rész**: egy `Border` keretbe helyezett `StackPanel`, amelyben vízszintesen egymás mellett helyezkednek el az adatok bevitelére szolgáló mezők:
     - **TextBlock**: rövid címke, amely megnevezi a mezőt (pl. "Márka")
     - **TextBox**: a felhasználó itt adja meg az adatokat (pl. CipoMarka)
  2. **ListView**: egy táblázatszerű lista, amelyben a már hozzáadott cipők jelennek meg. A `GridView` segítségével oszlopokat használunk.

    - A `GridViewColumn` elemek rendelkeznek egy `Header` (fejléc) és egy `DisplayMemberBinding` tulajdonsággal.
    - **`DisplayMemberBinding="{Binding Valami}"`** azt jelenti, hogy az adott oszlop az adatforrás (pl. `Cipo`) egy bizonyos tulajdonságát jeleníti meg.  
      Például:
      ```xml
      <GridViewColumn Header="Márka" DisplayMemberBinding="{Binding Marka}" />
      ```
      → Ez azt jelenti, hogy a `Cipo` objektum `Marka` nevű mezőjét fogja megjeleníteni ebben az oszlopban.

### TabItem – Vásárlók fül

- Ugyanaz a logika, mint a cipőknél:
  - `TextBox`-okban megadjuk: név, email, születési év, lakhely, telefonszám
  - `ListView` táblázatban megjelenítjük ezeket
  - Itt is `DisplayMemberBinding` kapcsolja az oszlopokat a `Felhasznalo` osztály mezőihez, például:
    ```xml
    <GridViewColumn Header="Lakhely" DisplayMemberBinding="{Binding Lakhely}" />
    ```

### TabItem – Összekötés fül

- **Grid**: Itt már komolyabb elrendezés van, két oszlop és három sor.
- **ListBox**: két külön lista, az egyikben cipőket, a másikban vásárlókat választunk ki.
  - A vásárlók listája a `FelhasznaloListBox`, és itt:
    - `DisplayMemberPath="Nev"` → ez egyszerűbb alternatíva a `DisplayMemberBinding`-re, csak string mezőt lehet megadni.
- **Button**: „Összekapcsolás” gomb, ami a kiválasztott elemekből kapcsolatot hoz létre.
- **ListView**: itt jelennek meg a kapcsolatok:
  ```xml
  <GridViewColumn Header="Felhasználó" DisplayMemberBinding="{Binding FelhasznaloNev}" />
  <GridViewColumn Header="Cipő" DisplayMemberBinding="{Binding CipoMarka}" />
  <GridViewColumn Header="Cipő ID" DisplayMemberBinding="{Binding CipoID}" />
  ```
  
### TabItem – Kereső fül

- **ComboBox**: legördülő lista, amelyből kiválaszthatunk egy vásárlót. Az itt kiválasztott felhasználó alapján jelennek meg a hozzá kapcsolódó cipők.
  - A `SelectionChanged="KeresesFelhasznaloComboBox_SelectionChanged"` tulajdonság azt jelzi, hogy ha a felhasználó változik, akkor egy C# metódus fut le, ami betölti a kapcsolódó cipőket.

- **ListView**: itt jelennek meg a kiválasztott vásárlóhoz kapcsolt cipők.
  - Minden sor egy `Cipo` objektumot reprezentál.
  - A `GridViewColumn` elemek `DisplayMemberBinding="{Binding ...}"` attribútummal rendelkeznek. Ez az adatforráshoz való kötést jelenti.
  
    Például:
    ```xml
    <GridViewColumn Header="Márka" DisplayMemberBinding="{Binding Marka}" />
    ```
    Ez azt jelenti, hogy az oszlop a `Cipo` típusú objektum `Marka` mezőjét jeleníti meg.

- Az oszlopok sorrendje és tartalma:
  - **ID**: a cipő azonosítója
  - **Márka**: a cipő márkája
  - **Méret**: a cipő mérete
  - **Szín**: a cipő színe
  - **Ár**: a cipő ára

---

### Egyéb vezérlők

- **Button**: műveleti gombok, például „Hozzáadás” vagy „Összekapcsolás”. Ezekhez a háttérben eseménykezelők kapcsolódnak (`Click="..."`), amelyek C# metódusokat hívnak meg.

- **Border**: olyan vizuális konténer, amelynek segítségével keretet, hátteret, lekerekített sarkokat és szegélyeket tudunk adni más vezérlőelemek köré. A megjelenést javítja, valamint logikailag is elválaszthatjuk vele a különböző részeket.

---

### DisplayMemberBinding="{Binding ...}" magyarázata

A `GridViewColumn` oszlopoknál gyakran szerepel a következő kifejezés:  
**`DisplayMemberBinding="{Binding Valami}"`**

Ez azt jelenti, hogy az adott oszlop tartalma egy objektum (pl. egy `Cipo` vagy `Felhasznalo`) `Valami` nevű tulajdonságát jeleníti meg.

Példa:
```xml
<GridViewColumn Header="Márka" DisplayMemberBinding="{Binding Marka}" />
