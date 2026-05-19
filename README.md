```mermaid
classDiagram
    class Fold {
        - tulelok : Tulelo[*]
        - menedekek : Menedek[*]
        + csapasSzimulacio(c: Csapas)
        + legrosszabbMenedek(korlat: int) : Menedek
    }
    
    class Tulelo {
        - nev : string
        - eletero : int
        + Tulelo(n: string, e: int)
        + eleteroCsokken(ertek: int)
        + meghal()
    }
    
    class Menedek {
        - allapot : int
        - lakok : Tulelo[*]
        + Menedek(a: int)
        + allapotCsokken(ertek: int)
        + osszedol()
        + osszFizikaiAllapot() : int
    }
    
    %% Tervminta: Stratégia (Strategy) az éghajlatfüggő csapásokhoz
    class Csapas {
        <<interface>>
        + sujt(f: Fold)
    }
    
    %% Tervminta: Egyke (Singleton) a konkrét stratégiákhoz
    class Ehinseg {
        <<singleton>>
        + sujt(f: Fold)
    }
    class RadioaktivEso {
        <<singleton>>
        + sujt(f: Fold)
    }
    class MutansTamadas {
        <<singleton>>
        + sujt(f: Fold)
    }

    %% Kapcsolatok
    Fold "1" *-- "*" Tulelo : tartalmaz
    Fold "1" *-- "*" Menedek : tartalmaz
    Menedek "0..1" o-- "*" Tulelo : lakik
    
    Csapas <|.. Ehinseg : megvalósít
    Csapas <|.. RadioaktivEso : megvalósít
    Csapas <|.. MutansTamadas : megvalósít
