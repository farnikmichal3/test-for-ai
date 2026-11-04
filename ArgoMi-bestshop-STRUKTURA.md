# Stromová struktura projektu ArgoMi-bestshop

Navržená struktura pro modulární monolit na frameworku Nette.

```text
argomi-bestshop/
├── .editorconfig                # Konfigurace editoru (konce řádků, odsazení)
├── .gitattributes               # Nastavení Gitu pro zachování konců řádků
├── .gitignore                   # Ignorované soubory (temp, vendor, local.neon)
├── phpstan.neon                 # Konfigurace PHPStan (level: max)
├── composer.json                # Závislosti PHP, autoloading, definice skriptů
├── composer.lock
├── package.json                 # Závislosti Node.js, build skripty (esbuild, sass)
├── package-lock.json
├── README.md                    # Dokumentace, jak projekt rychle rozjet
│
├── app/                         # Hlavní adresář aplikace
│   ├── Bootstrap.php            # Inicializace Nette kontejneru a prostředí
│   │
│   ├── Shared/                  # Sdílené jádro (zcela nezávislé na modulech)
│   │   ├── Domain/              # Hodnotové objekty (Money, Quantity, ULID)
│   │   └── Infrastructure/      # EventDispatcher, BaseRepository, Clock
│   │
│   ├── Modules/                 # Zapouzdřené doménové a byznys moduly
│   │   ├── Catalog/             # Modul: Catalog (Vlna 1)
│   │   │   ├── Api/             # Veřejné API modulu (CatalogFacade, DTOs)
│   │   │   ├── Domain/          # Entity a pravidla (Product, Variant, Category)
│   │   │   ├── Application/     # Command/Query Handlery, interní služby
│   │   │   ├── Infrastructure/  # Repozitáře nad Explorerem, mapování
│   │   │   └── DI/              # CatalogExtension.php (registrace do DI)
│   │   │
│   │   ├── Pricing/             # Modul: Pricing (Ceny variant po měnách)
│   │   ├── Inventory/           # Modul: Inventory (Sklad, rezervace)
│   │   ├── Cart/                # Modul: Cart (Košík jako doménový objekt)
│   │   ├── Checkout/            # Modul: Checkout (Průchod pokladnou)
│   │   ├── Orders/              # Modul: Orders (Objednávky, stavový automat)
│   │   ├── Tax/                 # Modul: Tax (Daňové třídy a sazby)
│   │   ├── Customer/            # Modul: Customer (Zákazníci, registrace)
│   │   ├── Shipping/            # Modul: Shipping (Způsoby dopravy)
│   │   ├── Payments/            # Modul: Payments (Platební brány a metody)
│   │   ├── Translation/         # Modul: Translation (Správa textů v DB)
│   │   └── Mailing/             # Modul: Mailing (Šablony e-mailů, fronta)
│   │
│   └── UI/                      # Uživatelské rozhraní (Nette Presentery)
│       ├── Front/               # Veřejná část e-shopu
│       │   ├── Home/            # Domovská stránka
│       │   ├── Catalog/         # Výpisy kategorií, detaily produktů
│       │   ├── Cart/            # Stránka košíku
│       │   ├── Checkout/            # Pokladna
│       │   ├── @layout.latte    # Hlavní šablona veřejného webu
│       │   └── FrontPresenter.php # Společný předek pro front-end
│       │
│       └── Admin/               # Administrační rozhraní
│           ├── Dashboard/       # Úvodní přehledy
│           ├── Catalog/         # Správa produktů a kategorií
│           ├── Orders/          # Správa objednávek
│           ├── Translation/     # Úprava textů a překladů z administrace
│           ├── @layout.latte    # Hlavní šablona administrace
│           └── AdminPresenter.php # Společný předek (hlídání ACL a session)
│
├── assets/                      # Zdrojové soubory pro front-end build
│   ├── js/                      # app.js (inicializace Naja, Bootstrap)
│   └── scss/                    # app.scss (Bootstrap, custom styly)
│
├── bin/                         # Spustitelné skripty
│   └── console                  # Vstupní bod pro CLI (Symfony/Contributte console)
│
├── config/                      # Konfigurační soubory Nette
│   ├── common.neon              # Společná konfigurace (mapování presenterů, moduly)
│   ├── services.neon            # Globální registrace služeb
│   └── local.neon               # Lokální nastavení (přihlášení k DB) – ignorováno Gitem
│
├── db/                          # Migrace a databázové skripty (Nextras Migrations)
│   ├── structures/              # 001_wave1_structure.sql (DDL)
│   ├── basic-data/              # 002_basic_data.sql (Číselníky, jazyky)
│   └── dummy-data/              # 003_demo_products.sql (Testovací data)
│
├── log/                         # Chybové logy Tracy (ignorováno Gitem)
├── temp/                        # Dočasné soubory Nette (cache, šablony) (ignorováno Gitem)
├── tests/                       # Testovací suita projektu
│   ├── Architecture/            # Architektonické testy (hranice modulů přes phpat/deptrac)
│   ├── Integration/             # Integrační testy repozitářů nad DB
│   └── Unit/                    # Jednotkové testy (Shared komponenty)
│
└── www/                         # Document Root webového serveru
    ├── assets/                  # Vygenerovaný produkční JS a CSS build (ignorováno Gitem)
    ├── index.php                # Vstupní bod aplikace
    └── .htaccess                # Směrování (mod_rewrite) na index.php
```
