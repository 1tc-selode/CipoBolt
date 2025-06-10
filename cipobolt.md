# Cipő Nyilvántartó – WPF alkalmazás

Ez az alkalmazás egy **Windows Presentation Foundation (WPF)** alapú rendszer, amely cipők és vásárlók adatainak kezelésére szolgál. Lehetőség van új adatokat hozzáadni, menteni, betölteni, valamint kapcsolatot létrehozni cipők és vásárlók között.

---

## Felhasználói Felület

A felhasználói felület a következő főbb elemekből áll:

- **Window**: A fő ablak, amely meghatározza az alkalmazás méretét, címét, betűtípusát és hogy hol jelenjen meg a képernyőn. Itt történik az összes többi vezérlőelem elhelyezése.

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
