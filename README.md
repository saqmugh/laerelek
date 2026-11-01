# Lærelek – fra kode til App Store (uten Mac)

Appen bygges i skyen av Codemagic og lastes automatisk opp til TestFlight.
Bundle-ID: `com.saqibmughal.laerelek`

## 1. Registrer Bundle-ID (Apple Developer, 2 min)
1. Gå til developer.apple.com › Account › **Certificates, Identifiers & Profiles** › **Identifiers** › **+**.
2. Velg **App IDs** › **App** › Continue.
3. Description: `Laerelek`. Bundle ID: **Explicit** › `com.saqibmughal.laerelek`.
4. Ikke huk av noen ekstra funksjoner. Continue › Register.

## 2. Opprett appen i App Store Connect (3 min)
1. appstoreconnect.apple.com › **Apper** › **+** › **Ny app**.
2. Plattform: iOS. Navn: `Lærelek` (er navnet tatt, prøv `Lærelek – norsk og matte`).
3. Primærspråk: Norsk (bokmål). Bundle-ID: den du nettopp laget. SKU: `laerelek-001`. Brukertilgang: Full.

## 3. Lag en API-nøkkel til Codemagic (3 min)
1. App Store Connect › **Brukere og tilgang** › **Integrasjoner** › **App Store Connect API** › **Teamnøkler**.
   (Første gang må du trykke «Be om tilgang» og godta vilkårene.)
2. **Generer API-nøkkel**. Navn: `Codemagic`. Tilgang: **App Manager**.
3. Last ned `.p8`-filen. **Den kan bare lastes ned én gang** – ta vare på den.
4. Noter **Issuer ID** (øverst på siden) og **Key ID** (på raden til nøkkelen).

## 4. Legg koden på GitHub (5 min)
1. Lag en gratis konto på github.com.
2. **New repository** › navn `laerelek` › **Public** › Create.
3. Trykk «uploading an existing file». Pakk ut zip-filen og dra **alt innholdet** i mappen inn. Commit changes.
4. Personvernsiden: **Settings** › **Pages** › Source: *Deploy from a branch* › Branch: `main` og mappe `/docs` › Save.
   Etter et par minutter ligger den på `https://BRUKERNAVN.github.io/laerelek/`. Det er personvern-lenken til App Store.

Repoet er offentlig fordi GitHub Pages er gratis da. Det ligger ingen passord eller nøkler i koden.

## 5. Sett opp Codemagic (10 min)
1. Lag konto på codemagic.io med **Sign up with GitHub**.
2. **Team settings** › **Team integrations** › **Developer Portal** › **Manage keys** › **Add key**:
   - App Store Connect API key name: `Laerelek API-nokkel` (skriv nøyaktig slik)
   - Issuer ID og Key ID fra steg 3, og last opp `.p8`-filen.
3. **Add application** › GitHub › velg `laerelek` › prosjekttype **Other** / bruk `codemagic.yaml`.
4. I appen: **Environment variables** › legg til:
   - Variable name: `CERTIFICATE_PRIVATE_KEY`
   - Value: hele innholdet i filen `laerelek-sertifikatnokkel.txt` (fra og med `-----BEGIN` til og med `-----END ... KEY-----`)
   - Group: `laerelek` · huk av **Secret** › Add.
5. **Start new build** › workflow **Lærelek iOS** › Start. Tar ca. 15 minutter.

Når bygget er grønt, ligger appen i App Store Connect under **TestFlight** etter 10–30 minutter.

## 6. Test på iPad/iPhone
Installer appen **TestFlight** fra App Store, legg deg selv til som intern tester i App Store Connect › TestFlight, og installer Lærelek.

## 7. Send til App Store
I App Store Connect › Lærelek › versjon 1.0:
- **Skjermbilder**: bruk bildene i mappen `skjermbilder` (iPhone 6,9" og iPad 13").
- **Beskrivelse, stikkord, støtte-URL** (støtte-URL kan være personvernsiden).
- **Appinformasjon**: Kategori *Utdanning*. Under alder: velg **Laget for barn** › aldersgruppe *6–8 år* (eller *9–11*).
- **Personvern i appen**: Personvernerklæring-URL = GitHub Pages-lenken. Datainnsamling: **«Nei, vi samler ikke inn data»**.
- **Pris**: Gratis.
- **Bygg**: velg bygget fra TestFlight › **Send til vurdering**.

## Senere oppdateringer
Endre `www/index.html`, last opp den nye filen på GitHub, og trykk **Start new build** i Codemagic. Byggnummeret øker automatisk. Ved ny versjon i App Store, endre `MARKETING_VERSION` i `ios/App/App.xcodeproj/project.pbxproj` (f.eks. 1.0 → 1.1).
