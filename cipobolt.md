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
```cs
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
```cs
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
    - `DisplayMemberPath="Nev"` → ez egyszerűbb alternatíva a `DisplayMemberBinding`-re, csak string mezőt lehet megadni.
- **Button**: „Összekapcsolás” gomb, ami a kiválasztott elemekből kapcsolatot hoz létre.
- **ListView**: itt jelennek meg a kapcsolatok:
  ```xml
  <GridViewColumn Header="Felhasználó" DisplayMemberBinding="{Binding FelhasznaloNev}" />
  <GridViewColumn Header="Cipő" DisplayMemberBinding="{Binding CipoMarka}" />
  <GridViewColumn Header="Cipő ID" DisplayMemberBinding="{Binding CipoID}" />
  ```
```cs
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

```cs
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
- Megpróbálja átalakítani a CipoMeret.Text és CipoAr.Text szöveges értékeket egész számokká
- Ha bármelyik nem konvertálható számként (például betűket tartalmaz), akkor megjelenít egy üzenetet: "A méret és az ár csak szám lehet!"
- Ezután a return; miatt a függvény leáll, nem folytatódik tovább, így nem ad hozzá új cipőt

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
- Létrehoz egy új Cipo nevű objektumot
- Az Id mezőjéhez beállítja az aktuális kovetkezoCipoId értéket, majd növeli azt eggyel (++)
- A Marka, Meret, Szin, Ar tulajdonságokat a felhasználó által beírt szöveges mezők értékeivel tölti fel

Létrehoz egy új Cipo nevű objektumot.

Az Id mezőjéhez beállítja az aktuális kovetkezoCipoId értéket, majd növeli azt eggyel (++).

A Marka, Meret, Szin, Ar tulajdonságokat a felhasználó által beírt szöveges mezők értékeivel tölti fel.

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





