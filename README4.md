```mermaid
classDiagram
    class Kiralysag {
        - lovagok : Lovag[*]
        - kastelyok : Kastely[*]
        + tamadasSzimulacio(t: Tamadas, k: Kastely)
        + legkisebbErejuKastely() : Kastely
    }
    
    class Lovag {
        - nev : string
        - harciEro : int
        + Lovag(n: string, h: int)
        + harciEroCsokken(ertek: int)
        + meghal()
    }
    
    class Kastely {
        - nev : string
        - vedero : int
        - vedok : Lovag[*]
        + Kastely(n: string, v: int)
        + vederoCsokken(ertek: int)
        + osszedol()
        + egyuttesEro() : int
    }
    
    %% Tervminta: Stratégia (Strategy) 
    class Tamadas {
        <<interface>>
        + sujt(k: Kastely)
    }
    
    %% Tervminta: Egyke (Singleton)
    class Ostrom {
        <<singleton>>
        + sujt(k: Kastely)
    }
    class Kemtevekenyseg {
        <<singleton>>
        + sujt(k: Kastely)
    }
    class Tuzvesz {
        <<singleton>>
        + sujt(k: Kastely)
    }

    Kiralysag "1" *-- "*" Lovag : tartalmaz
    Kiralysag "1" *-- "*" Kastely : tartalmaz
    Kastely "0..1" o-- "*" Lovag : vedik
    
    Tamadas <|.. Ostrom : megvalósít
    Tamadas <|.. Kemtevekenyseg : megvalósít
    Tamadas <|.. Tuzvesz : megvalósít
