```mermaid
classDiagram
    class Fold {
        - tulelok : Tulelo[*]
        - menedekek : Menedek[*]
        + csapasSzimulacio(c: Csapas)
        + legrosszabbMenedek(korlat: int) : Menedek
    }
    note for Fold "legrosszabbMenedek(korlat):<br>legrosszabb = null<br>minAllapot = MAX_INT<br>foreach m in menedekek:<br>  if m.osszFizikaiAllapot() > korlat<br>  and m.allapot < minAllapot:<br>    minAllapot = m.allapot<br>    legrosszabb = m<br>return legrosszabb<br><br>csapasSzimulacio(c):<br>  c.sujt(this)"
    
    class Tulelo {
        - nev : string
        - eletero : int
        + Tulelo(n: string, e: int)
        + eleteroCsokken(ertek: int)
        + meghal()
    }
    note for Tulelo "eleteroCsokken(ertek):<br>  eletero -= ertek<br>  if eletero <= 0:<br>    meghal()<br><br>meghal():<br>  eletero = 0<br>  // kikerül a<br>  // menedékből"
    
    class Menedek {
        - allapot : int
        - lakok : Tulelo[*]
        + Menedek(a: int)
        + allapotCsokken(ertek: int)
        + osszedol()
        + osszFizikaiAllapot() : int
    }
    note for Menedek "allapotCsokken(ertek):<br>  allapot -= ertek<br>  if allapot <= 0:<br>    osszedol()<br><br>osszedol():<br>  foreach t in lakok:<br>    t.meghal()<br>  lakok.clear()<br><br>osszFizikaiAllapot():<br>  sum = 0<br>  foreach t in lakok:<br>    sum += t.eletero<br>  return sum"
    
    class Csapas {
        <<interface>>
        + sujt(f: Fold)
    }
    
    class Ehinseg {
        <<singleton>>
        + sujt(f: Fold)
    }
    note for Ehinseg "sujt(f):<br>  foreach t in f.tulelok:<br>    t.eleteroCsokken(2)"

    class RadioaktivEso {
        <<singleton>>
        + sujt(f: Fold)
    }
    note for RadioaktivEso "sujt(f):<br>  foreach m in f.menedekek:<br>    m.allapotCsokken(1)<br>  foreach t in f.tulelok:<br>    t.eleteroCsokken(1)"

    class MutansTamadas {
        <<singleton>>
        + sujt(f: Fold)
    }
    note for MutansTamadas "sujt(f):<br>  foreach t in f.tulelok:<br>    t.eleteroCsokken(3)"

    Fold "1" *-- "*" Tulelo : tartalmaz
    Fold "1" *-- "*" Menedek : tartalmaz
    Menedek "0..1" o-- "*" Tulelo : lakik
    
    Csapas <|.. Ehinseg : megvalósít
    Csapas <|.. RadioaktivEso : megvalósít
    Csapas <|.. MutansTamadas : megvalósít
