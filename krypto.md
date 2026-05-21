# Základy kryptografie (blokové a proudové šifry včetně jejich režimů, symetrická a asymetrická kryptografie a jejich využití, hashovací funkce, infrastruktura veřejného klíče – základní pilíře, certifikační autorita, certifikát veřejného klíče
## Základy
- Kryptografie – obor zabývající se ochranou informací.
- Šifrování – převod čitelné zprávy na nečitelnou pomocí algoritmu a klíče.
- Dešifrování – opačný proces, získání původní zprávy.
- Plaintext – otevřený (čitelný) text.
- Ciphertext – zašifrovaný text.
- Klíč – údaj řídící šifrování a dešifrování.
- **CIA**
    - Confidentiality (utajení)
        - Přístup má jenom autorizovaný subjekt
    - Integrity (Integrita)
        - Modifikovat smí jenom autorziovaný objekt
    - Availability (dostupnost)
        - Dostpunost autorizovaným objektům
- **AAA🚗**
    - Authentication
        - Ověření identity uživatele autorizační autoritou
    - Authorization
        - Přidělení přístupových práv uživateli, který prošel autentizací
    - Accounting
        - Spyware. Zbírání informací o uživateli
- **Kerckhoffův princip** - Bezpečnost systému nesmí záviset na utajení algoritmu, ale pouze na utajení klíče
## Symetrická kryptografie
- Jeden klíč
- Odesílatel pomocí klíče zašifruje
- Příjemce stejným klíčem odšifruje
- Problém může nastat u předávání klíče
    - Možnosti:
        - Osobní předání
        - Předem domluvený klíč
        - Výměna klíče asymetricky (Například https)

## Asymetrická kryptografie
- Dva klíče
    - Veřejný
    - Soukromý
- Odesílatel zveřejní veřejný klíč
    - Kdokoliv k němu může mít přístup
    - Slouží k zašifrování
- Zašifrovaná data lze rozšifrovat pouze privátním klíčem
- Zpráva + Veřejný klíč -> Šifra
- Šifra + Privátní klíč -> Zpráva
- Generace klíče
    - Rozklad velkých čísel
    - Diskrétní logaritmus
    - Eliptické křivky
- Podepisuje se soukromým klíčem
- Ověřuje se veřejným klíčem
- RSA
    - Dvě prvočísla
        - p = 61
        - q = 53
    - Vynásobí se
        - n = 61 * 53 = 3233
    - Spočítá se Eulerova funkce
        - n2 = (p - 1)(q - 1)
        - n2 = 60 * 52 = 3120
    - Vybere se veřejný exponent
        - 1 < e < n2
        - nesmí mít společného dělitele s n2
        - e = 17
        - veřejný klíč je (n,e) = (3233,17)
    - Spočítá se soukromý exponent
        - d * e ≡ 1 (mod n2)
            - d je inverzní k e
        - d = 2753
    - Šifrování
        - m = message (např. 123)
        - c = šifra
            - c = m<sup>e</sup> mod n
            - c = 123<sup>17</sup> mod 3233
            - c = 855
    - Dešifrování
        - m = c<sup>d</sup> mod n
        - m = 855<sup>2753</sup> mod 3233
        - m = 123

| Vlastnost   | Symetrická | Asymetrická     |
| ----------- | ---------- | --------------- |
| Počet klíčů | 1          | 2               |
| Rychlost    | rychlá     | pomalá          |
| Náročnost   | nízká      | vysoká          |
| Použití     | velká data | malé objemy dat |
| Příklady    | AES, RC4   | RSA, ECC        |

- V praxi se používá asymetrické šifrování na výměnu klíče pro symetrické šifrování, pak se šifruje symetricky (zmizí problém s únikem symetrického klíče)
- Server udělá veřejný a privátní klíč
    - Zveřejní veřejný
- Klient udělá symetrický klíč
    - Veřejným klíčem serveru zašifruje symetrický klíč
- Server rozšifruje a je jediný kdo zná symetrický klíč
- Posílají si data zašifrované symetricky

## Blokové šifry
- Používá pevně velké bloky dat (většinou 64/128 bitů)
- Šifruje každý blok zvlášť
- Pořadí bloků zůstává stejné
- Převádí bloky na jiné pomoci klíče
### AES - Advanced Encryption standard
- 128 / 192 / 256 bitů klíč
- Nejrozšířenější moderní bloková šifra
- Nahrazuje DES a 3DES
- Operace v několika kolech:
    - Substituce bajtů (SubBytes)
    - Posun řádků (ShiftRows)
    - Míchání sloupců (MixColumns)
    - Přidání podklíče (AddRoundKey)
- Vysoká bezpečnost a rychlost
- Použití: HTTPS, VPN, Wi-Fi (WPA2/WPA3)

### DES
- 64bitové bloky
- 56bitový klíč (dnes už slabý)
- Feistelova síť (opakované substituce a permutace)
- Dnes považován za nebezpečný (lze prolomit brute force)
- Historicky velmi rozšířený
### 3DES
- DES aplikovaný třikrát (EDE: Encrypt–Decrypt–Encrypt)
- Efektivní velikost klíče ~112/168 bitů
- Výrazně bezpečnější než DES
- Velmi pomalý
- Dnes zastaralý, nahrazen AES
### ECB - Electronic CodeBook
- Každý blok se šifruje nezávisle
- Stejný vstup → stejný výstup
- Výhody:
    - jednoduchý
    - paralelizovatelný
- Nevýhody:
    - prozrazuje vzory v datech
    - není bezpečný pro většinu použití
### CBC
- Každý blok je XORován s předchozím ciphertextem
- První blok používá IV (initialization vector)
- Výhody:
    - skrývá vzory v datech
    - výrazně bezpečnější než ECB
- Nevýhody:
    - nelze paralelizovat šifrování
    - chyba v bloku ovlivní další blok

## Proudové šifry
- Šifrují se po bitech nebo bajtech
- Generuje pseudonáhodný proud bajtů
- Data se kombinují s plaintextem pomocí XOR
- Výhody:
    - rychlé, nenáročné
- Nevýhody:
    - snadno prolomitelné
### RC4
## Hashovací funkce
- Jednocestná hashovací funkce
 - Zpráva ma libovolnou délku
 - Výsledek má pevnou délku
 - Funkci lze snadno realizovat
 - Funkce je jednocestná
 - Funkce je odolná proti kolizím
- Použití
    - Uložení hesel (Unlike Town of Salem)
    - Kontrola integrity dat bez klíče
    - HMAC (Hash message authentication code)
        - Společně se zprávou se pošle HMAC = hash_funkce(message, key)
    - Digitální podpisy
- Odolnost proti kolizím
    - Je prakticky nemožné najít libovolné dvě různé zprávy se stejným hashem
        - Vstupů je nekonečně mnoho, takže matematicky existovat budou
    - Odolnosti 
        - Weak resistance: z hashe nejde zpětně najít zprávu
        - Strong resistance: nejde najít ani dvě různé zprávy se stejným hashem
## Infrastruktura veřejného klíče
- **PKI - public key infrastructure**
- Soustava pravidel, technologií a autorit
- Zajišťují důvěryhodnou distribuci veřejných klíčů.
- Pilíře
    - Bezpečnostní politika
        - Pravidla provozu a zabezpečení.
    - Procedury
        - Postupy generování, distribuce a správy klíčů.
    - Produkty
        - Hardware a software pro práci s klíči.
    - Autority
        - Organizace zajišťující důvěryhodnost systému.
- Součást PKI
    - **Certifikační autorita**
        - Vydává certifikáty.
        - Potvrzuje identitu subjektů.
        - Podepisuje veřejné klíče.
        - Vede seznam zneplatněných certifikátů (CRL).
    - Registrační autorita
        - Ověřuje identitu žadatele.
        - Přijímá žádosti o certifikát.
        - Předává informace CA.
    - Adresářová služba
        - Uchovává platné klíče a seznam zneplatněných certifikátů

## Certifikát
- **Elektronický dokument spojující identitu subjektu s veřejným klíčem.**
- Poskytuje záruku, že identita spojená s vlastníkem daného veřejného klíče není podvržená
- Obsah certifikátu
    - Veřejný klíč
    - Jméno vlastníka
    - Vydavatel (CA)
    - Sériové číslo
    - Platnost
    - Digitální podpis CA
- Důvody odvolání certifikátu
    - Ztráta soukromého klíče.
    - Kompromitace klíče.
    - Vypršení platnosti.
- X.509 - Standrad, používaný skoro všude
    - Windows, Linux, Https, maily, VPN...