# ZPG

## Standardní zobrazovací řetězec (realizace jednotlivých kroků řetězce, modelovací a zobrazovací transformace, Phongův osvětlovací model, řešení viditelnosti, identifikace těles, stručná charakteristika standardu OpenGL a jazyka GLSL)

### Zobrazovací řetězec
![alt text](image.png)
![alt text](image-24.png)

- **Vertex shader** - Vrací lokaci
    - Vezme data a pro každý vertex zavolá funkci
    - Na GPU, paralelně
    - Určuje, kde budou body
    - Probíhají v něm transformace
- **Clipping**
    - Ořízne všechno mimo zorné pole
    - Vytvoří nové vertexy na hraně view
- **Rasterizace**
 - Převede tělsa na fragmenty
    - Fragment - *Kandidát* na pixel. Každá pozice pixelu může mít více fragmentů, při vykreslování se pak vykreslí ten nejbližší (většinou)
- Pošle fragmenty do fragment shaderu
- **Fragment Shader** - Vrací barvu fragmentu
    - Interpoluje hloubku, barvu, texturu, normály...
- **Texture unit**
    - Fragment shader požádá o vzorek textury
    - TU převede UV souřadnice na skutečné souřadnice v textuře
        - UV mapping
            - Nearest
                - Který pixel je nejblíž
            - Linear, Bilinear, Trilinear... (1,2,3 vekotry)
                - Vezme nejbližší a ve směru x vektorů interpoluje s ostatními
    - V případě šáhnutí mimo texturu dopočítá
        - Repeat
        - Mirror repeat
        - Clamp to edge
    - Použije derivaci sousedních vrcholů
    - Výsledek se vrátí fragment shaderu
- **Framebuffer** - Doublebuffer - Do jednoho se zapisuje, zatímco druhý je zobrazený, pak se přehodí. Zamezuje se tak postupnému vykreslování obrazovky. (Někde se používá triplebuffer -> same thing tbh)
    - Color buffer - Každý buffer má 8 bitů (0-255).
        - Skládá se ze čtyř bufferů pro rgba (rgb ze tří)
        - Každý buffer určuje hodnotu jednotlivé barvy, popřípadě alfa složky
    - Depth buffer / Z-buffer
        - Každý fragment uchovává hloubku jak daleko je od kamery
        - Prochází fragmenty, uchovává ten nejbližší, najde-li bližší, zahodí ten přechozí a uloží si nový nejbližší
        - 0 je near plane
        - 1 je far plane
        - Z-fighting - pixely které jsou *v sobě* problikávají
    - Stencil buffer
        - Udržuje o fragmentech číslo (stencil value), které pak může porovnávat s nějakým pravidlem
        - Na projektu u každého fragemntu udržuje id drawable objectu

### Modelovací a zobrazovací transformace

### Phong
![alt text](image-3.png)
![alt text](image-26.png)
- **I** - Intezita světla
- **r** - Materiálová složka
- **a** - Ambientní
    - Konstatní neměná složka, simuluje odraz paprsků, i když na plochu nesvítí žádné světlo, není žádoucí mít čistě černou plochu
    - Přidává se jedna na konec bez ohledu na počet světel
- **d** - Difuzní
    - Barevná část.
    - Alfa - Úhel mezi normálou povrchu a směrem ke světlu
- **s** - Spekulární
    - Odražená část (Odlesk)
    - Fí - Úhel mezi odraženým vektorem světla a směrem pohledu kamery (oka)
    - h - Shininess. Určuje jak moc se bude odlesk *rozlévat*
        - Větší číslo -> Strmnější pád funkce cosinus -> Menší plocha odlesku
- **HM - Blinn**
    - Fí - Úhel mezi half vecotrem (poloviční mezi vektorem světla a vektorem ke kameře/oku) a normálovým vektorem



## Geometrické modelování (afinní a projektivní prostory, popis těles a možnosti jejich reprezentace, základní křivky používané v počítačové grafice, jejich vyjádření, vlastnosti a použití, Fergusonova kubika, Bézierova křivka)
### Afiní a projektivní prostory
- Afinní prostor (big nono)
    - ((P1 + T1) S1) + T2
        - Pro každý bod
        - Náročné
        - Nemůžeš si to vypočítat předem
- Projektivní prostor (big yes yes)
    - Přidáš homogení složku (další rozměr)
    - Lze všechno násobit
    
![alt text](image-25.png)

### Základní křivky


![alt text](image-23.png)