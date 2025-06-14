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

![image](https://github.com/user-attachments/assets/7a14bce6-f8c9-4a56-9781-82fc7c2fcb7b)


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
```xaml
      <TabItem Header="Cipők">
    <StackPanel Margin="10">
        <Border Background="White" CornerRadius="5" Padding="10" Margin="0,0,0,10"
                BorderThickness="1" BorderBrush="#FFDDDDDD">
            <StackPanel Orientation="Horizontal">
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Márka"/>
                    <TextBox x:Name="CipoMarka"/>
                </StackPanel>
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Méret"/>
                    <TextBox x:Name="CipoMeret"/>
                </StackPanel>
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Szín"/>
                    <TextBox x:Name="CipoSzin"/>
                </StackPanel>
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Ár"/>
                    <TextBox x:Name="CipoAr"/>
                </StackPanel>
                <Button Content="Hozzáadás" Click="AddCipo_Click" Margin="15 25 5 5"/>
            </StackPanel>
        </Border>
        <Border CornerRadius="5" Background="White" BorderThickness="1" BorderBrush="#FFDDDDDD">
            <ListView x:Name="CipoList" Height="500">
                <ListView.View>
                    <GridView>
                        <GridViewColumn Header="ID" DisplayMemberBinding="{Binding Id}" Width="80"/>
                        <GridViewColumn Header="Márka" DisplayMemberBinding="{Binding Marka}" Width="150"/>
                        <GridViewColumn Header="Méret" DisplayMemberBinding="{Binding Meret}" Width="80"/>
                        <GridViewColumn Header="Szín" DisplayMemberBinding="{Binding Szin}" Width="100"/>
                        <GridViewColumn Header="Ár" DisplayMemberBinding="{Binding Ar}" Width="100"/>
                    </GridView>
                </ListView.View>
            </ListView>
        </Border>
    </StackPanel>
</TabItem>
```

### TabItem – Vásárlók fül

- Ugyanaz a logika, mint a cipőknél:
  - `TextBox`-okban megadjuk: név, email, születési év, lakhely, telefonszám
  - `ListView` táblázatban megjelenítjük ezeket
  - Itt is `DisplayMemberBinding` kapcsolja az oszlopokat a `Felhasznalo` osztály mezőihez, például:
    ```xml
    <GridViewColumn Header="Lakhely" DisplayMemberBinding="{Binding Lakhely}" />
    ```
![image](https://github.com/user-attachments/assets/b2e969af-fded-4064-950d-a98d40a17001)

```xaml
<TabItem Header="Vásárlók">
    <StackPanel Margin="10">
        <Border Background="White" CornerRadius="5" Padding="10" Margin="0,0,0,10"
                BorderThickness="1" BorderBrush="#FFDDDDDD">
            <StackPanel Orientation="Horizontal">
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Név"/>
                    <TextBox x:Name="FelhaszNev"/>
                </StackPanel>
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Email"/>
                    <TextBox x:Name="FelhaszEmail"/>
                </StackPanel>
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Év"/>
                    <TextBox x:Name="FelhaszEv"/>
                </StackPanel>
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Lakhely"/>
                    <TextBox x:Name="FelhaszLakhely"/>
                </StackPanel>
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="Telefonszám"/>
                    <TextBox x:Name="FelhaszTel"/>
                </StackPanel>
                <Button Content="Hozzáadás" Click="AddFelhasznalo_Click" Margin="15 25 5 5"/>
            </StackPanel>
        </Border>
        <Border CornerRadius="5" Background="White" BorderThickness="1" BorderBrush="#FFDDDDDD">
            <ListView x:Name="FelhasznaloList" Height="500">
                <ListView.View>
                    <GridView>
                        <GridViewColumn Header="Név" DisplayMemberBinding="{Binding Nev}" Width="150"/>
                        <GridViewColumn Header="Email" DisplayMemberBinding="{Binding Email}" Width="180"/>
                        <GridViewColumn Header="Év" DisplayMemberBinding="{Binding SzuletesiEv}" Width="80"/>
                        <GridViewColumn Header="Lakhely" DisplayMemberBinding="{Binding Lakhely}" Width="150"/>
                        <GridViewColumn Header="Tel" DisplayMemberBinding="{Binding Telefonszam}" Width="150"/>
                    </GridView>
                </ListView.View>
            </ListView>
        </Border>
    </StackPanel>
</TabItem>
```

### TabItem – Összekötés fül

- **Grid**: Itt már komolyabb elrendezés van, két oszlop és három sor.
- **ListBox**: két külön lista, az egyikben cipőket, a másikban vásárlókat választunk ki.
  - A vásárlók listája a `FelhasznaloListBox`, és itt:
    - `DisplayMemberBinding`, csak string mezőt lehet megadni.
- **Button**: „Összekapcsolás” gomb, ami a kiválasztott elemekből kapcsolatot hoz létre.
- **ListView**: itt jelennek meg a kapcsolatok:
  ```xml
  <GridViewColumn Header="Felhasználó" DisplayMemberBinding="{Binding FelhasznaloNev}" />
  <GridViewColumn Header="Cipő" DisplayMemberBinding="{Binding CipoMarka}" />
  <GridViewColumn Header="Cipő ID" DisplayMemberBinding="{Binding CipoID}" />
  ```
![image](https://github.com/user-attachments/assets/70300911-622c-4803-bae7-cf1045bebc6c)

```xaml
<TabItem Header="Összekötés">
    <Grid Margin="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <StackPanel Grid.Column="0" Grid.Row="0" Orientation="Vertical">
            <TextBlock Text="Válassz cipőt" Margin="5 5 5 0"/>
            <Border CornerRadius="5" Background="White" 
                    BorderThickness="1" BorderBrush="#FFDDDDDD" Margin="5">
                <ListBox x:Name="CipoListBox" Height="150" Margin="5"/>
            </Border>
        </StackPanel>

        <StackPanel Grid.Column="1" Grid.Row="0" Orientation="Vertical">
            <TextBlock Text="Válassz vásárlót" Margin="5 5 5 0"/>
            <Border CornerRadius="5" Background="White" 
                    BorderThickness="1" BorderBrush="#FFDDDDDD" Margin="5">
                <ListBox x:Name="FelhasznaloListBox" Height="150" Margin="5" DisplayMemberPath="Nev"/>
            </Border>
        </StackPanel>

        <Button Content="Összekapcsolás" Grid.ColumnSpan="2" Grid.Row="1" 
                Click="Osszekotes_Click" Margin="5" HorizontalAlignment="Center" Width="150"/>

        <StackPanel Grid.ColumnSpan="2" Grid.Row="2" Orientation="Vertical">
            <TextBlock Text="Kapcsolatok" Margin="5 5 5 0"/>
            <Border CornerRadius="5" Background="White" 
                    BorderThickness="1" BorderBrush="#FFDDDDDD" Margin="5">
                <ListView x:Name="KapcsolatList" Height="300">
                    <ListView.View>
                        <GridView>
                            <GridViewColumn Header="Felhasználó" DisplayMemberBinding="{Binding FelhasznaloNev}" Width="250"/>
                            <GridViewColumn Header="Cipő" DisplayMemberBinding="{Binding CipoMarka}" Width="150"/>
                            <GridViewColumn Header="Cipő ID" DisplayMemberBinding="{Binding CipoID}" Width="100"/>
                        </GridView>
                    </ListView.View>
                </ListView>
            </Border>
        </StackPanel>
    </Grid>
</TabItem>
```
  
### TabItem – Kereső fül

- **Elrendezés**: Egy `Grid` két oszloppal:
  - Bal oldalon: felhasználók `TreeView`-ban
  - Jobb oldalon: kapcsolt cipők `ListView`-ban

#### TreeView – Vásárlók listája

- A `TreeView` egy fastruktúrájú lista, ami itt a felhasználók nevét jeleníti meg (`DisplayMemberPath="Nev"`).
- Az `SelectedItemChanged` esemény akkor fut le, ha kiválasztunk egy vásárlót.
- A vezérlő `Border`-ben van, ami keretet és háttérszínt ad.

#### ListView – Kapcsolt cipők

- A kiválasztott felhasználóhoz tartozó cipők `ListView`-ban jelennek meg.
- A `GridView` oszlopai: ID, Márka, Méret, Szín, Ár.
- Szintén `Border` veszi körül a vizuális egységesség érdekében.

![image](https://github.com/user-attachments/assets/a8b56a04-fb82-49db-80d9-2d456ab004a8)

```xaml
<TabItem Header="Kereső">
    <Grid Margin="10">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="250"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <Border Background="White" CornerRadius="5" Padding="5" Margin="5"
    BorderThickness="1" BorderBrush="#FFDDDDDD" Grid.Column="0">
            <TreeView x:Name="FelhasznaloTreeView" SelectedItemChanged="FelhasznaloTreeView_SelectedItemChanged" DisplayMemberPath="Nev"/>
        </Border>
        <StackPanel Grid.Column="1" Margin="10,0,0,0" Orientation="Vertical">
            <TextBlock Text="Kapcsolt cipők" Margin="5 0 0 5" FontWeight="Bold"/>
            <Border CornerRadius="5" Background="White" BorderThickness="1" BorderBrush="#FFDDDDDD">
                <ListView x:Name="KeresesCipoLista" Height="450" Margin="5">
                    <ListView.View>
                        <GridView>
                            <GridViewColumn Header="ID" DisplayMemberBinding="{Binding Id}" Width="80"/>
                            <GridViewColumn Header="Márka" DisplayMemberBinding="{Binding Marka}" Width="150"/>
                            <GridViewColumn Header="Méret" DisplayMemberBinding="{Binding Meret}" Width="80"/>
                            <GridViewColumn Header="Szín" DisplayMemberBinding="{Binding Szin}" Width="120"/>
                            <GridViewColumn Header="Ár" DisplayMemberBinding="{Binding Ar}" Width="100"/>
                        </GridView>
                    </ListView.View>
                </ListView>
            </Border>
        </StackPanel>
    </Grid>
</TabItem>
```

### DisplayMemberBinding="{Binding ...}" magyarázata

A `GridViewColumn` oszlopoknál gyakran szerepel a következő kifejezés:  
**`DisplayMemberBinding="{Binding Valami}"`**

Ez azt jelenti, hogy az adott oszlop tartalma egy objektum (pl. egy `Cipo` vagy `Felhasznalo`) `Valami` nevű tulajdonságát jeleníti meg.

Példa:
```xml
<GridViewColumn Header="Márka" DisplayMemberBinding="{Binding Marka}" />
```

---

## CS kód

### Osztályok

Ebben az alkalmazásban három egyszerű osztály szerepel: `Cipo`, `Felhasznalo` és `Kapcsolat`. Ezek az osztályok az adatok szerkezetét határozzák meg – azaz, hogy **milyen mezőkből áll egy cipő, egy vásárló vagy egy kapcsolat**.

A `get; set;` egy automatikus tárolási forma:
- **`get`**: kiolvasható az érték
- **`set`**: beállítható új érték
```cs
public class Cipo
{
    public int Id { get; set; }
    public string Marka { get; set; }
    public string Meret { get; set; }
    public string Szin { get; set; }
    public string Ar { get; set; }

    public override string ToString() => $"[{Id}] {Marka} - {Meret} - {Szin} - {Ar} Ft";
}

public class Felhasznalo
{
    public string Nev { get; set; }
    public string Email { get; set; }
    public string SzuletesiEv { get; set; }
    public string Lakhely { get; set; }
    public string Telefonszam { get; set; }
    public override string ToString() => Nev;
}

public class Kapcsolat
{
    public string FelhasznaloNev { get; set; }
    public string CipoMarka { get; set; }
    public string CipoID { get; set; }
    public override string ToString() => $"{FelhasznaloNev} - {CipoMarka} {CipoID}";
}
```

### Változók:

- **`cipok`**: lista (`List<Cipo>`) – ide kerülnek a programban tárolt cipők.
- **`felhasznalok`**: lista (`List<Felhasznalo>`) – a regisztrált vásárlókat tartalmazza.
- **`kapcsolatok`**: lista (`List<Kapcsolat>`) – a cipő-felhasználó kapcsolatok.
- **`kovetkezoCipoId`**: egész szám – az új cipőknek adandó azonosító, automatikusan növekszik.

```cs
private List<Cipo> cipok = new List<Cipo>();
private List<Felhasznalo> felhasznalok = new List<Felhasznalo>();
private List<Kapcsolat> kapcsolatok = new List<Kapcsolat>();
private int kovetkezoCipoId = 1;
```

### Konstruktor

```cs
public MainWindow()
{
    InitializeComponent();                // A felhasználói felület elemeit betölti
    BetoltCipok();                        // Betölti a mentett cipőket fájlból
    BetoltFelhasznalok();                // Betölti a vásárlók adatait
    BetoltKapcsolatok();                 // Betölti a korábbi összekötéseket
    FeltoltFelhasznaloTreeView();        // A vásárlókat beteszi a TreeView vezérlőbe
}
```

# AddCipo_Click

Ez a `AddCipo_Click` nevű függvény egy eseménykezelő, ami akkor fut le, amikor a felhasználó rákattint az „AddCipo” gombra. A függvény célja, hogy egy új cipő adatot vegyen fel a felhasználó által megadott adatok alapján, és ezt megjelenítse a felhasználói felületen.

```csharp
    if (!int.TryParse(CipoMeret.Text, out _) || !int.TryParse(CipoAr.Text, out _))
{
    MessageBox.Show("A méret és az ár csak szám lehet!");
    return;
}
```
Megpróbálja átalakítani a CipoMeret.Text és CipoAr.Text szöveges értékeket egész számokká.

Ha bármelyik nem konvertálható számként (például betűket tartalmaz), akkor megjelenít egy üzenetet: "A méret és az ár csak szám lehet!".

Ezután a return; miatt a függvény leáll, nem folytatódik tovább, így nem ad hozzá új cipőt.

```csharp
var cipo = new Cipo
{
    Id = kovetkezoCipoId++,
    Marka = CipoMarka.Text,
    Meret = CipoMeret.Text,
    Szin = CipoSzin.Text,
    Ar = CipoAr.Text
};
```
Létrehoz egy új Cipo nevű objektumot.

Az Id mezőjéhez beállítja az aktuális kovetkezoCipoId értéket, majd növeli azt eggyel (++).

A Marka, Meret, Szin, Ar tulajdonságokat a felhasználó által beírt szöveges mezők értékeivel tölti fel.

```csharp
cipok.Add(cipo);
```
Hozzáadja az új cipő objektumot a cipok nevű listához, ami valószínűleg a cipők adatait tárolja.

```csharp
CipoList.Items.Add(cipo);
CipoListBox.Items.Add(cipo);
```
Két különböző UI elemhez (talán két lista vagy listaelem kontroll) hozzáadja a létrehozott cipő objektumot, hogy megjelenjen a felületen.

```csharp
MentCipok();
```
Meghív egy másik függvényt, ami feltehetőleg elmenti az aktuális cipők listáját valamilyen tartós tárolóba (például fájlba vagy adatbázisba).

```csharp
private void AddCipo_Click(object sender, RoutedEventArgs e)
{
    if (!int.TryParse(CipoMeret.Text, out _) || !int.TryParse(CipoAr.Text, out _))
    {
        MessageBox.Show("A méret és az ár csak szám lehet!");
        return;
    }

    var cipo = new Cipo
    {
        Id = kovetkezoCipoId++,
        Marka = CipoMarka.Text,
        Meret = CipoMeret.Text,
        Szin = CipoSzin.Text,
        Ar = CipoAr.Text
    };

    cipok.Add(cipo);
    CipoList.Items.Add(cipo);
    CipoListBox.Items.Add(cipo);
    MentCipok();
}
```
---
# AddFelhasznalo_Click

Ez az AddFelhasznalo_Click nevű függvény egy eseménykezelő, ami akkor fut le, amikor a felhasználó rákattint az „AddFelhasznalo” gombra. A függvény célja, hogy egy új felhasználó adatot vegyen fel a felhasználó által megadott adatok alapján, és ezt megjelenítse a felhasználói felületen.

```csharp
if (!int.TryParse(FelhaszEv.Text, out _) || !long.TryParse(FelhaszTel.Text, out _))
{
    MessageBox.Show("A születési év és a telefonszám csak szám lehet!");
    return;
}
```
Megpróbálja átalakítani a FelhaszEv.Text szöveges értéket egész számként, és a FelhaszTel.Text értéket hosszú egész számmá (long).

Ha bármelyik nem konvertálható számként (például betűket tartalmaz), akkor megjelenít egy üzenetet: "A születési év és a telefonszám csak szám lehet!".

Ezután a return; miatt a függvény leáll, nem folytatódik tovább, így nem ad hozzá új felhasználót.

```csharp
var felh = new Felhasznalo
{
    Nev = FelhaszNev.Text,
    Email = FelhaszEmail.Text,
    SzuletesiEv = FelhaszEv.Text,
    Lakhely = FelhaszLakhely.Text,
    Telefonszam = FelhaszTel.Text
};
```
Létrehoz egy új Felhasznalo nevű objektumot.

A tulajdonságait a felhasználó által beírt szöveges mezők értékeivel tölti fel (Nev, Email, SzuletesiEv, Lakhely, Telefonszam).

```csharp
felhasznalok.Add(felh);
```
Hozzáadja az új felhasználó objektumot a felhasznalok nevű listához, ami valószínűleg a felhasználók adatait tárolja.

```csharp
FelhasznaloList.Items.Add(felh);
FelhasznaloListBox.Items.Add(felh);
```
Két különböző UI elemhez (például lista- vagy listaelem kontrollokhoz) hozzáadja a létrehozott felhasználó objektumot, hogy megjelenjen a felületen.

```csharp
MentFelhasznalok();
```
Meghív egy másik függvényt, ami feltehetőleg elmenti az aktuális felhasználók listáját valamilyen tartós tárolóba (például fájlba vagy adatbázisba).

```csharp
private void AddFelhasznalo_Click(object sender, RoutedEventArgs e)
{
    if (!int.TryParse(FelhaszEv.Text, out _) || !long.TryParse(FelhaszTel.Text, out _))
    {
        MessageBox.Show("A születési év és a telefonszám csak szám lehet!");
        return;
    }

    var felh = new Felhasznalo
    {
        Nev = FelhaszNev.Text,
        Email = FelhaszEmail.Text,
        SzuletesiEv = FelhaszEv.Text,
        Lakhely = FelhaszLakhely.Text,
        Telefonszam = FelhaszTel.Text
    };
    felhasznalok.Add(felh);
    FelhasznaloList.Items.Add(felh);
    FelhasznaloListBox.Items.Add(felh);
    MentFelhasznalok();
}
```
---

# Osszekotes_Click

Ez a Osszekotes_Click nevű függvény egy eseménykezelő, amely akkor fut le, amikor a felhasználó rákattint az „Osszekotes” gombra. A függvény célja, hogy összekapcsoljon egy kiválasztott cipőt és egy kiválasztott felhasználót, majd ezt az összekapcsolást elmentse és megjelenítse.

```csharp
if (CipoListBox.SelectedItem is Cipo c && FelhasznaloListBox.SelectedItem is Felhasznalo f)
```
Ellenőrzi, hogy a CipoListBox-ban (cipőket tartalmazó lista) van-e kiválasztott elem, és hogy az elem típusa Cipo.

Ugyanígy ellenőrzi, hogy a FelhasznaloListBox-ban (felhasználókat tartalmazó lista) van-e kiválasztott elem, és az elem típusa Felhasznalo.

Ha mindkét feltétel teljesül, a kiválasztott cipő objektumot a c változóba, a kiválasztott felhasználó objektumot pedig az f változóba menti.

```csharp
var kapcsolat = new Kapcsolat { FelhasznaloNev = f.Nev, CipoMarka = c.Marka, CipoID = c.Id.ToString() };

```
Létrehoz egy új Kapcsolat nevű objektumot, amely az összekapcsolást reprezentálja.

A kapcsolatba beállítja a felhasználó nevét (FelhasznaloNev) a kiválasztott f.Nev alapján.

Beállítja a cipő márkáját (CipoMarka) a kiválasztott cipő c.Marka értékével.

Beállítja a cipő azonosítóját (CipoID) a kiválasztott cipő c.Id értékének szöveges változataként (ToString()).

```csharp
kapcsolatok.Add(kapcsolat);

```
Hozzáadja az új kapcsolatot a kapcsolatok nevű listához, amely az összes felhasználó-cipő összekapcsolást tárolja.

```csharp
KapcsolatList.Items.Add(kapcsolat);

```
Hozzáadja az új kapcsolatot a KapcsolatList nevű felhasználói felület elemeihez, hogy megjelenjen a felületen.

```csharp
MentKapcsolatok();

```
Meghív egy függvényt, amely elmenti az aktuális kapcsolatokat valamilyen tartós tárolóba (például fájlba vagy adatbázisba).

```csharp
FeltoltFelhasznaloTreeView();

```
Meghív egy másik függvényt, amely feltölti a felhasználói fa nézetet (TreeView), hogy tükrözze az új kapcsolatokat.

```csharp
private void Osszekotes_Click(object sender, RoutedEventArgs e)
{
    if (CipoListBox.SelectedItem is Cipo c && FelhasznaloListBox.SelectedItem is Felhasznalo f)
    {
        var kapcsolat = new Kapcsolat { FelhasznaloNev = f.Nev, CipoMarka = c.Marka, CipoID = c.Id.ToString() };
        kapcsolatok.Add(kapcsolat);
        KapcsolatList.Items.Add(kapcsolat);
        MentKapcsolatok();

        FeltoltFelhasznaloTreeView();
    }
}
```
---
# BetoltCipok

Ez a BetoltCipok függvény betölti a cipők adatait egy „cipok.txt” nevű fájlból, és feltölti a program cipőlistáit, hogy az adatok megjelenjenek a felhasználói felületen.

```csharp
if (File.Exists("cipok.txt"))
{
    string[] sorok = File.ReadAllLines("cipok.txt");
    foreach (string sor in sorok)
    {
        string[] mezok = sor.Split(';');
        if (mezok.Length == 5)
        {
            
        }
    }
}
```
Ellenőrzi, hogy létezik-e a „cipok.txt” fájl az aktuális mappában.
Beolvassa a „cipok.txt” fájl összes sorát egy string[] tömbbe, ahol minden elem egy sor a fájlból.
Végigmegy minden egyes soron a fájlban.
A sorokat pontosvessző (;) jel mentén felbontja mezőkre, így létrejön egy tömb, ahol az egyes elemek a cipő adatai (például Id, Márka, Méret, Szín, Ár).
Ellenőrzi, hogy pontosan 5 mező van-e a sorban, mert csak így lehet értelmezni a cipő adatait.

```csharp
Cipo cipo = new Cipo
{
    Id = int.Parse(mezok[0]),
    Marka = mezok[1],
    Meret = mezok[2],
    Szin = mezok[3],
    Ar = mezok[4]
};
```
Létrehoz egy új Cipo objektumot a fájlból beolvasott mezők alapján.

Az első mezőt (Id) egész számmá konvertálja.

A többi mezőt szövegként állítja be.

```csharp
cipok.Add(cipo);
CipoList.Items.Add(cipo);
CipoListBox.Items.Add(cipo);
```
Hozzáadja az új cipőt a program cipok nevű listájához.

Megjeleníti az új cipőt a felhasználói felületen található két listaelemen (CipoList és CipoListBox).

```csharp
if (cipo.Id >= kovetkezoCipoId)
    kovetkezoCipoId = cipo.Id + 1;
```
Ellenőrzi, hogy a beolvasott cipő azonosítója nagyobb vagy egyenlő-e a következő szabad azonosítóval (kovetkezoCipoId).

Ha igen, akkor a következő elérhető azonosítót az aktuális cipő Id-jének egyessel növelt értékére állítja, hogy ne legyen ütközés az új cipők létrehozásakor.

```csharp
private void BetoltCipok()
{
    if (File.Exists("cipok.txt"))
    {
        string[] sorok = File.ReadAllLines("cipok.txt");
        foreach (string sor in sorok)
        {
            string[] mezok = sor.Split(';');
            if (mezok.Length == 5)
            {
                Cipo cipo = new Cipo
                {
                    Id = int.Parse(mezok[0]),
                    Marka = mezok[1],
                    Meret = mezok[2],
                    Szin = mezok[3],
                    Ar = mezok[4]
                };
                cipok.Add(cipo);
                CipoList.Items.Add(cipo);
                CipoListBox.Items.Add(cipo);

                if (cipo.Id >= kovetkezoCipoId)
                    kovetkezoCipoId = cipo.Id + 1;
            }
        }
    }
}
```
---
# MentCipok

Ez a MentCipok függvény elmenti a program cipőlistáját egy „cipok.txt” nevű fájlba úgy, hogy minden cipő adatait egy sorba írja, pontosvesszővel elválasztva.

```csharp
using var writer = new StreamWriter("cipok.txt");

```
Megnyit egy író (StreamWriter) objektumot a „cipok.txt” fájlhoz.

Ha a fájl már létezik, felülírja.

Az using kulcsszó gondoskodik róla, hogy az író a művelet végén automatikusan bezáródjon és a fájl mentésre kerüljön.

```csharp
foreach (var c in cipok)

```
Végigmegy a cipok listában tárolt összes cipő objektumon.

```csharp
writer.WriteLine($"{c.Id};{c.Marka};{c.Meret};{c.Szin};{c.Ar}");

```
Minden cipő adatait pontosvesszővel elválasztott formátumban kiírja egy új sorba a fájlban.

Az Id, Márka, Méret, Szín és Ár mezők kerülnek így mentésre.
```csharp
private void MentCipok()
{
    using var writer = new StreamWriter("cipok.txt");
    foreach (var c in cipok)
    {
        writer.WriteLine($"{c.Id};{c.Marka};{c.Meret};{c.Szin};{c.Ar}");
    }
}
```
---
# BetoltFelhasznalok

Ez a függvény betölti a felhasználók adatait a „felhasznalok.txt” fájlból, és feltölti a program felhasználói listáit, hogy az adatok megjelenjenek a felhasználói felületen.
- Ellenőrzi, hogy létezik-e a „felhasznalok.txt” fájl.
- Beolvassa a fájl összes sorát.
- Minden sort pontosvesszővel (;) bont fel, és ha 5 mező van, akkor létrehoz egy Felhasznalo objektumot a mezők alapján.
- Hozzáadja az új felhasználót a felhasznalok listához és megjeleníti a felületi listákban (FelhasznaloList, FelhasznaloListBox).

```csharp
private void BetoltFelhasznalok()
{
    if (File.Exists("felhasznalok.txt"))
    {
        string[] sorok = File.ReadAllLines("felhasznalok.txt");
        foreach (string sor in sorok)
        {
            string[] mezok = sor.Split(';');
            if (mezok.Length == 5)
            {
                Felhasznalo felh = new Felhasznalo
                {
                    Nev = mezok[0],
                    Email = mezok[1],
                    SzuletesiEv = mezok[2],
                    Lakhely = mezok[3],
                    Telefonszam = mezok[4]
                };
                felhasznalok.Add(felh);
                FelhasznaloList.Items.Add(felh);
                FelhasznaloListBox.Items.Add(felh);
            }
        }
    }
}
```
---
# MentFelhasznalok

Ez a függvény elmenti a felhasznalok listában tárolt összes felhasználó adatait a felhasznalok.txt fájlba, minden sorba egy felhasználót írva pontosvesszővel elválasztva.
- StreamWriter-t használ a fájlba íráshoz.
- Végigmegy minden Felhasznalo objektumon a listában.
- Kiírja a nevét, e-mail címét, születési évét, lakhelyét és telefonszámát.

```csharp
private void MentFelhasznalok()
{
    using var writer = new StreamWriter("felhasznalok.txt");
    foreach (var f in felhasznalok)
    {
        writer.WriteLine($"{f.Nev};{f.Email};{f.SzuletesiEv};{f.Lakhely};{f.Telefonszam}");
    }
}
```
---
# BetoltKapcsolatok

Ez a függvény betölti a kapcsolatok adatait a kapcsolatok.txt fájlból, és feltölti a program kapcsolatlistáját, hogy az adatok megjelenjenek a felületen.
- Ellenőrzi, hogy létezik-e a fájl.
- Beolvassa az összes sort.
- Minden sort pontosvessző (;) alapján mezőkre bont.
- Ha pontosan 3 mező van, létrehoz egy Kapcsolat objektumot a mezők alapján.
- Hozzáadja a listához (kapcsolatok) és megjeleníti a KapcsolatList elemben.

```csharp
private void BetoltKapcsolatok()
{
    if (File.Exists("kapcsolatok.txt"))
    {
        string[] sorok = File.ReadAllLines("kapcsolatok.txt");
        foreach (string sor in sorok)
        {
            string[] mezok = sor.Split(';');
            if (mezok.Length == 3)
            {
                Kapcsolat kapcsolat = new Kapcsolat
                {
                    FelhasznaloNev = mezok[0],
                    CipoMarka = mezok[1],
                    CipoID = mezok[2]
                };
                kapcsolatok.Add(kapcsolat);
                KapcsolatList.Items.Add(kapcsolat);
            }
        }
    }
}
```
---
# MentKapcsolatok

Ez a függvény elmenti a kapcsolatok listában szereplő adatokat a kapcsolatok.txt fájlba.
- StreamWriter-t használ az íráshoz.
- Minden Kapcsolat objektumot egy sorban rögzít, pontosvesszővel (;) elválasztva:
    - FelhasznaloNev
    - CipoMarka
    - CipoID

```csharp
private void MentKapcsolatok()
{
    using var writer = new StreamWriter("kapcsolatok.txt");
    foreach (var k in kapcsolatok)
    {
        writer.WriteLine($"{k.FelhasznaloNev};{k.CipoMarka};{k.CipoID}");
    }
}
```
---
# FeltoltFelhasznaloTreeView

Ez a függvény frissíti a FelhasznaloTreeView nevű grafikus vezérlőt (pl. TreeView), hogy megjelenítse a programban tárolt összes felhasználót.
- Először törli a FelhasznaloTreeView összes korábbi elemét.
- Ezután végigmegy a felhasznalok listán, és hozzáadja az összes felhasználót a TreeView-hoz.

```csharp
private void FeltoltFelhasznaloTreeView()
{
    FelhasznaloTreeView.Items.Clear();
    foreach (var felh in felhasznalok)
    {
        FelhasznaloTreeView.Items.Add(felh);
    }
}
```
---
# FelhasznaloTreeView_SelectedItemChanged

Ez az eseménykezelő akkor fut le, amikor a felhasználó kiválaszt egy elemet a FelhasznaloTreeView vezérlőben. A célja: megjeleníteni az adott felhasználóhoz kapcsolódó cipőket a KeresesCipoLista nevű listában.

```csharp
KeresesCipoLista.Items.Clear();
```
Minden alkalommal, amikor új felhasználót választanak ki a TreeView-ból, először törli a cipők listáját, hogy ne maradjanak benne a korábbi adatok.

```csharp
if (FelhasznaloTreeView.SelectedItem is Felhasznalo felh)
```
Ez a sor ellenőrzi, hogy a kiválasztott elem valóban egy Felhasznalo típusú objektum-e.
Ha igen, akkor elnevezi felh-nek, és vele dolgozik tovább.

```csharp
var kapcsoltCipok = kapcsolatok
    .Where(k => k.FelhasznaloNev == felh.Nev)
    .Join(cipok,
        k => k.CipoID,
        c => c.Id.ToString(),
        (k, c) => c);
```
Ez a LINQ-lekérdezés két dolgot csinál:
- Szűri a kapcsolatok listáját (kapcsolatok), hogy csak azok maradjanak, ahol a FelhasznaloNev megegyezik a kiválasztott felhasználó nevével
- Ezután összekapcsolja ezeket a kapcsolati elemeket a cipők listájával (cipok) úgy, hogy:
    - a kapcsolat CipoID mezője (string) megegyezzen a cipő Id mezőjével (egész szám, stringgé alakítva)
- Az eredmény: egy kapcsoltCipok nevű lista, amely tartalmazza a kiválasztott felhasználóhoz tartozó cipőket
  
```csharp
foreach (var cipo in kapcsoltCipok)
{
    KeresesCipoLista.Items.Add(cipo);
}
```
A lekérdezésből kapott összes cipőt hozzáadja a KeresesCipoLista felhasználói felületi elemhez, így a felhasználó láthatja az eredményt.

```csharp
private void FelhasznaloTreeView_SelectedItemChanged(object sender, RoutedPropertyChangedEventArgs<object> e)
{
    KeresesCipoLista.Items.Clear();

    if (FelhasznaloTreeView.SelectedItem is Felhasznalo felh)
    {
        var kapcsoltCipok = kapcsolatok
            .Where(k => k.FelhasznaloNev == felh.Nev)
            .Join(cipok,
                k => k.CipoID,
                c => c.Id.ToString(),
                (k, c) => c);
        foreach (var cipo in kapcsoltCipok)
        {
            KeresesCipoLista.Items.Add(cipo);
        }
    }
}
```

<details>
<summary>Nyiss le az xaml forrásért!</summary> 

```xaml
<Window x:Class="WpfApp3.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WpfApp3"
        mc:Ignorable="d"
        Title="Cipő Nyilvántartó" Height="720" Width="900"
        FontFamily="Segoe UI" WindowStartupLocation="CenterScreen">
    <Window.Resources>
        <Style TargetType="TabItem">
            <Setter Property="Padding" Value="10 5"/>
            <Setter Property="FontWeight" Value="SemiBold"/>
        </Style>

        <Style TargetType="TextBox">
            <Setter Property="Padding" Value="5"/>
            <Setter Property="Margin" Value="5 5 15 5"/>
            <Setter Property="VerticalContentAlignment" Value="Center"/>
            <Setter Property="MinWidth" Value="120"/>
        </Style>

        <Style TargetType="Button">
            <Setter Property="Padding" Value="10 5"/>
            <Setter Property="Margin" Value="5"/>
            <Setter Property="Background" Value="#FF0078D7"/>
            <Setter Property="Foreground" Value="White"/>
            <Setter Property="BorderThickness" Value="0"/>
            <Setter Property="MinWidth" Value="100"/>
        </Style>

        <Style TargetType="ListView">
            <Setter Property="BorderThickness" Value="0"/>
            <Setter Property="Margin" Value="5"/>
        </Style>

        <Style TargetType="GridViewColumnHeader">
            <Setter Property="Padding" Value="10 5"/>
            <Setter Property="HorizontalContentAlignment" Value="Left"/>
            <Setter Property="Background" Value="#FFEEEEEE"/>
            <Setter Property="FontWeight" Value="SemiBold"/>
        </Style>

        <Style TargetType="TextBlock">
            <Setter Property="Margin" Value="5 5 0 5"/>
            <Setter Property="VerticalAlignment" Value="Center"/>
            <Setter Property="FontWeight" Value="SemiBold"/>
        </Style>
    </Window.Resources>

    <Grid Background="#FFF5F5F5">
        <TabControl Background="White">
            <!-- Cipők fül -->
            <TabItem Header="Cipők">
                <StackPanel Margin="10">
                    <Border Background="White" CornerRadius="5" Padding="10" Margin="0,0,0,10"
                            BorderThickness="1" BorderBrush="#FFDDDDDD">
                        <StackPanel Orientation="Horizontal">
                            <StackPanel Orientation="Vertical">
                                <TextBlock Text="Márka"/>
                                <TextBox x:Name="CipoMarka"/>
                            </StackPanel>
                            <StackPanel Orientation="Vertical">
                                <TextBlock Text="Méret"/>
                                <TextBox x:Name="CipoMeret"/>
                            </StackPanel>
                            <StackPanel Orientation="Vertical">
                                <TextBlock Text="Szín"/>
                                <TextBox x:Name="CipoSzin"/>
                            </StackPanel>
                            <StackPanel Orientation="Vertical">
                                <TextBlock Text="Ár"/>
                                <TextBox x:Name="CipoAr"/>
                            </StackPanel>
                            <Button Content="Hozzáadás" Click="AddCipo_Click" Margin="15 25 5 5"/>
                        </StackPanel>
                    </Border>
                    <Border CornerRadius="5" Background="White" BorderThickness="1" BorderBrush="#FFDDDDDD">
                        <ListView x:Name="CipoList" Height="500">
                            <ListView.View>
                                <GridView>
                                    <GridViewColumn Header="ID" DisplayMemberBinding="{Binding Id}" Width="80"/>
                                    <GridViewColumn Header="Márka" DisplayMemberBinding="{Binding Marka}" Width="150"/>
                                    <GridViewColumn Header="Méret" DisplayMemberBinding="{Binding Meret}" Width="80"/>
                                    <GridViewColumn Header="Szín" DisplayMemberBinding="{Binding Szin}" Width="100"/>
                                    <GridViewColumn Header="Ár" DisplayMemberBinding="{Binding Ar}" Width="100"/>
                                </GridView>
                            </ListView.View>
                        </ListView>
                    </Border>
                </StackPanel>
            </TabItem>

            <!-- Felhasználók fül -->
            <TabItem Header="Vásárlók">
                <StackPanel Margin="10">
                    <Border Background="White" CornerRadius="5" Padding="10" Margin="0,0,0,10"
                            BorderThickness="1" BorderBrush="#FFDDDDDD">
                        <StackPanel Orientation="Horizontal">
                            <StackPanel Orientation="Vertical">
                                <TextBlock Text="Név"/>
                                <TextBox x:Name="FelhaszNev"/>
                            </StackPanel>
                            <StackPanel Orientation="Vertical">
                                <TextBlock Text="Email"/>
                                <TextBox x:Name="FelhaszEmail"/>
                            </StackPanel>
                            <StackPanel Orientation="Vertical">
                                <TextBlock Text="Év"/>
                                <TextBox x:Name="FelhaszEv"/>
                            </StackPanel>
                            <StackPanel Orientation="Vertical">
                                <TextBlock Text="Lakhely"/>
                                <TextBox x:Name="FelhaszLakhely"/>
                            </StackPanel>
                            <StackPanel Orientation="Vertical">
                                <TextBlock Text="Telefonszám"/>
                                <TextBox x:Name="FelhaszTel"/>
                            </StackPanel>
                            <Button Content="Hozzáadás" Click="AddFelhasznalo_Click" Margin="15 25 5 5"/>
                        </StackPanel>
                    </Border>
                    <Border CornerRadius="5" Background="White" BorderThickness="1" BorderBrush="#FFDDDDDD">
                        <ListView x:Name="FelhasznaloList" Height="500">
                            <ListView.View>
                                <GridView>
                                    <GridViewColumn Header="Név" DisplayMemberBinding="{Binding Nev}" Width="150"/>
                                    <GridViewColumn Header="Email" DisplayMemberBinding="{Binding Email}" Width="180"/>
                                    <GridViewColumn Header="Év" DisplayMemberBinding="{Binding SzuletesiEv}" Width="80"/>
                                    <GridViewColumn Header="Lakhely" DisplayMemberBinding="{Binding Lakhely}" Width="150"/>
                                    <GridViewColumn Header="Tel" DisplayMemberBinding="{Binding Telefonszam}" Width="150"/>
                                </GridView>
                            </ListView.View>
                        </ListView>
                    </Border>
                </StackPanel>
            </TabItem>

            <!-- Összekötés fül -->
            <TabItem Header="Összekötés">
                <Grid Margin="10">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="*"/>
                        <ColumnDefinition Width="*"/>
                    </Grid.ColumnDefinitions>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="*"/>
                    </Grid.RowDefinitions>

                    <StackPanel Grid.Column="0" Grid.Row="0" Orientation="Vertical">
                        <TextBlock Text="Válassz cipőt" Margin="5 5 5 0"/>
                        <Border CornerRadius="5" Background="White" 
                                BorderThickness="1" BorderBrush="#FFDDDDDD" Margin="5">
                            <ListBox x:Name="CipoListBox" Height="150" Margin="5"/>
                        </Border>
                    </StackPanel>

                    <StackPanel Grid.Column="1" Grid.Row="0" Orientation="Vertical">
                        <TextBlock Text="Válassz vásárlót" Margin="5 5 5 0"/>
                        <Border CornerRadius="5" Background="White" 
                                BorderThickness="1" BorderBrush="#FFDDDDDD" Margin="5">
                            <ListBox x:Name="FelhasznaloListBox" Height="150" Margin="5" DisplayMemberPath="Nev"/>
                        </Border>
                    </StackPanel>

                    <Button Content="Összekapcsolás" Grid.ColumnSpan="2" Grid.Row="1" 
                            Click="Osszekotes_Click" Margin="5" HorizontalAlignment="Center" Width="150"/>

                    <StackPanel Grid.ColumnSpan="2" Grid.Row="2" Orientation="Vertical">
                        <TextBlock Text="Kapcsolatok" Margin="5 5 5 0"/>
                        <Border CornerRadius="5" Background="White" 
                                BorderThickness="1" BorderBrush="#FFDDDDDD" Margin="5">
                            <ListView x:Name="KapcsolatList" Height="300">
                                <ListView.View>
                                    <GridView>
                                        <GridViewColumn Header="Felhasználó" DisplayMemberBinding="{Binding FelhasznaloNev}" Width="250"/>
                                        <GridViewColumn Header="Cipő" DisplayMemberBinding="{Binding CipoMarka}" Width="150"/>
                                        <GridViewColumn Header="Cipő ID" DisplayMemberBinding="{Binding CipoID}" Width="100"/>
                                    </GridView>
                                </ListView.View>
                            </ListView>
                        </Border>
                    </StackPanel>
                </Grid>
            </TabItem>

            <!-- Kereső fül -->
            <TabItem Header="Kereső">
                <Grid Margin="10">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="250"/>
                        <ColumnDefinition Width="*"/>
                    </Grid.ColumnDefinitions>
                    <Border Background="White" CornerRadius="5" Padding="5" Margin="5"
                BorderThickness="1" BorderBrush="#FFDDDDDD" Grid.Column="0">
                        <TreeView x:Name="FelhasznaloTreeView" SelectedItemChanged="FelhasznaloTreeView_SelectedItemChanged" DisplayMemberPath="Nev"/>
                    </Border>
                    <StackPanel Grid.Column="1" Margin="10,0,0,0" Orientation="Vertical">
                        <TextBlock Text="Kapcsolt cipők" Margin="5 0 0 5" FontWeight="Bold"/>
                        <Border CornerRadius="5" Background="White" BorderThickness="1" BorderBrush="#FFDDDDDD">
                            <ListView x:Name="KeresesCipoLista" Height="450" Margin="5">
                                <ListView.View>
                                    <GridView>
                                        <GridViewColumn Header="ID" DisplayMemberBinding="{Binding Id}" Width="80"/>
                                        <GridViewColumn Header="Márka" DisplayMemberBinding="{Binding Marka}" Width="150"/>
                                        <GridViewColumn Header="Méret" DisplayMemberBinding="{Binding Meret}" Width="80"/>
                                        <GridViewColumn Header="Szín" DisplayMemberBinding="{Binding Szin}" Width="120"/>
                                        <GridViewColumn Header="Ár" DisplayMemberBinding="{Binding Ar}" Width="100"/>
                                    </GridView>
                                </ListView.View>
                            </ListView>
                        </Border>
                    </StackPanel>
                </Grid>
            </TabItem>

        </TabControl>
    </Grid>
</Window> 

```
</details>

<details>
<summary>Nyiss le az xaml.cs forrásért!</summary> 

```C#
using System.IO;
using System.Linq;
using System.Windows;
using System.Windows.Controls;
using System.Collections.Generic;

namespace WpfApp3
{
    public class Cipo
    {
        public int Id { get; set; }
        public string Marka { get; set; }
        public string Meret { get; set; }
        public string Szin { get; set; }
        public string Ar { get; set; }

        public override string ToString() => $"[{Id}] {Marka} - {Meret} - {Szin} - {Ar} Ft";
    }

    public class Felhasznalo
    {
        public string Nev { get; set; }
        public string Email { get; set; }
        public string SzuletesiEv { get; set; }
        public string Lakhely { get; set; }
        public string Telefonszam { get; set; }
        public override string ToString() => Nev;
    }

    public class Kapcsolat
    {
        public string FelhasznaloNev { get; set; }
        public string CipoMarka { get; set; }
        public string CipoID { get; set; }
        public override string ToString() => $"{FelhasznaloNev} - {CipoMarka} {CipoID}";
    }
    public partial class MainWindow : Window
    {
        private List<Cipo> cipok = new List<Cipo>();
        private List<Felhasznalo> felhasznalok = new List<Felhasznalo>();
        private List<Kapcsolat> kapcsolatok = new List<Kapcsolat>();
        private int kovetkezoCipoId = 1;

        public MainWindow()
        {
            InitializeComponent();
            BetoltCipok();
            BetoltFelhasznalok();
            BetoltKapcsolatok();
            FeltoltFelhasznaloTreeView();
        }

        private void AddCipo_Click(object sender, RoutedEventArgs e)
        {
            if (!int.TryParse(CipoMeret.Text, out _) || !int.TryParse(CipoAr.Text, out _))
            {
                MessageBox.Show("A méret és az ár csak szám lehet!");
                return;
            }

            var cipo = new Cipo
            {
                Id = kovetkezoCipoId++,
                Marka = CipoMarka.Text,
                Meret = CipoMeret.Text,
                Szin = CipoSzin.Text,
                Ar = CipoAr.Text
            };

            cipok.Add(cipo);
            CipoList.Items.Add(cipo);
            CipoListBox.Items.Add(cipo);
            MentCipok();
        }

        private void AddFelhasznalo_Click(object sender, RoutedEventArgs e)
        {
            if (!int.TryParse(FelhaszEv.Text, out _) || !long.TryParse(FelhaszTel.Text, out _))
            {
                MessageBox.Show("A születési év és a telefonszám csak szám lehet!");
                return;
            }

            var felh = new Felhasznalo
            {
                Nev = FelhaszNev.Text,
                Email = FelhaszEmail.Text,
                SzuletesiEv = FelhaszEv.Text,
                Lakhely = FelhaszLakhely.Text,
                Telefonszam = FelhaszTel.Text
            };
            felhasznalok.Add(felh);
            FelhasznaloList.Items.Add(felh);
            FelhasznaloListBox.Items.Add(felh);
            MentFelhasznalok();
        }

        private void Osszekotes_Click(object sender, RoutedEventArgs e)
        {
            if (CipoListBox.SelectedItem is Cipo c && FelhasznaloListBox.SelectedItem is Felhasznalo f)
            {
                var kapcsolat = new Kapcsolat { FelhasznaloNev = f.Nev, CipoMarka = c.Marka, CipoID = c.Id.ToString() };
                kapcsolatok.Add(kapcsolat);
                KapcsolatList.Items.Add(kapcsolat);
                MentKapcsolatok();

                FeltoltFelhasznaloTreeView();
            }
        }

        private void BetoltCipok()
        {
            if (File.Exists("cipok.txt"))
            {
                string[] sorok = File.ReadAllLines("cipok.txt");
                foreach (string sor in sorok)
                {
                    string[] mezok = sor.Split(';');
                    if (mezok.Length == 5)
                    {
                        Cipo cipo = new Cipo
                        {
                            Id = int.Parse(mezok[0]),
                            Marka = mezok[1],
                            Meret = mezok[2],
                            Szin = mezok[3],
                            Ar = mezok[4]
                        };
                        cipok.Add(cipo);
                        CipoList.Items.Add(cipo);
                        CipoListBox.Items.Add(cipo);

                        if (cipo.Id >= kovetkezoCipoId)
                            kovetkezoCipoId = cipo.Id + 1;
                    }
                }
            }
        }

        private void MentCipok()
        {
            using var writer = new StreamWriter("cipok.txt");
            foreach (var c in cipok)
            {
                writer.WriteLine($"{c.Id};{c.Marka};{c.Meret};{c.Szin};{c.Ar}");
            }
        }

        private void BetoltFelhasznalok()
        {
            if (File.Exists("felhasznalok.txt"))
            {
                string[] sorok = File.ReadAllLines("felhasznalok.txt");
                foreach (string sor in sorok)
                {
                    string[] mezok = sor.Split(';');
                    if (mezok.Length == 5)
                    {
                        Felhasznalo felh = new Felhasznalo
                        {
                            Nev = mezok[0],
                            Email = mezok[1],
                            SzuletesiEv = mezok[2],
                            Lakhely = mezok[3],
                            Telefonszam = mezok[4]
                        };
                        felhasznalok.Add(felh);
                        FelhasznaloList.Items.Add(felh);
                        FelhasznaloListBox.Items.Add(felh);
                    }
                }
            }
        }

        private void MentFelhasznalok()
        {
            using var writer = new StreamWriter("felhasznalok.txt");
            foreach (var f in felhasznalok)
            {
                writer.WriteLine($"{f.Nev};{f.Email};{f.SzuletesiEv};{f.Lakhely};{f.Telefonszam}");
            }
        }
        private void BetoltKapcsolatok()
        {
            if (File.Exists("kapcsolatok.txt"))
            {
                string[] sorok = File.ReadAllLines("kapcsolatok.txt");
                foreach (string sor in sorok)
                {
                    string[] mezok = sor.Split(';');
                    if (mezok.Length == 3)
                    {
                        Kapcsolat kapcsolat = new Kapcsolat
                        {
                            FelhasznaloNev = mezok[0],
                            CipoMarka = mezok[1],
                            CipoID = mezok[2]
                        };
                        kapcsolatok.Add(kapcsolat);
                        KapcsolatList.Items.Add(kapcsolat);
                    }
                }
            }
        }

        private void MentKapcsolatok()
        {
            using var writer = new StreamWriter("kapcsolatok.txt");
            foreach (var k in kapcsolatok)
            {
                writer.WriteLine($"{k.FelhasznaloNev};{k.CipoMarka};{k.CipoID}");
            }
        }

        private void FeltoltFelhasznaloTreeView()
        {
            FelhasznaloTreeView.Items.Clear();
            foreach (var felh in felhasznalok)
            {
                FelhasznaloTreeView.Items.Add(felh);
            }
        }

        private void FelhasznaloTreeView_SelectedItemChanged(object sender, RoutedPropertyChangedEventArgs<object> e)
        {
            KeresesCipoLista.Items.Clear();

            if (FelhasznaloTreeView.SelectedItem is Felhasznalo felh)
            {
                var kapcsoltCipok = kapcsolatok
                    .Where(k => k.FelhasznaloNev == felh.Nev)
                    .Join(cipok,
                        k => k.CipoID,
                        c => c.Id.ToString(),
                        (k, c) => c);
                foreach (var cipo in kapcsoltCipok)
                {
                    KeresesCipoLista.Items.Add(cipo);
                }
            }
        }
    }
}

```
</details>
