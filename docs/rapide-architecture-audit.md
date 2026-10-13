# Rapide Laravel architecture audit

Auditdatum: 13 oktober 2026. Doel: aanvullende agentregels voor nieuwe Rapide-projecten, niet het standaardiseren of wijzigen van de onderzochte applicaties.

## Methode en grenzen

De audit gebruikt afzonderlijke, alleen gelezen Git-checkouts van de GitHub-defaultbranches. De inventaris omvat alle bijgehouden applicatiemappen, manifesten, lockfiles, routes, tests, migrations, toolingconfiguratie en operationele bestanden. Representatieve implementaties zijn gelezen; tellingen zijn een inventaris, geen kwaliteits- of coveragepercentage.

| Bron | Branch | Geaudit commit |
| --- | --- | --- |
| [Moto2Go](https://github.com/rapideinternet/Moto2Go/tree/ee92c3b4d279419b144be1a73a842130af2ec3b4) | `development` | `ee92c3b4d279419b144be1a73a842130af2ec3b4` |
| [Blackbirds Widgets Platform](https://github.com/rapideinternet/blackbirds-widgets-platform/tree/761b13398c4e902939b9585ec1234e2be22cf1c6) | `development` | `761b13398c4e902939b9585ec1234e2be22cf1c6` |
| Deze boilerplate | `small_changes` | `a0bb41f` bij aanvang |

Hieronder zijn `M:` en `B:` repositoryrelatieve paden op bovenstaande commits; `T:` verwijst naar deze boilerplate. Dit maakt ook niet afzonderlijk gelinkte voorbeelden terugvindbaar. Afwezig betekent afwezig in deze snapshot, niet dat een patroon nooit is gebruikt. Gitgeschiedenis is beperkt tot de opgehaalde laatste 30 commits per branch.

Er zijn geen dependencies geïnstalleerd, applicaties gestart, databases benaderd of tests uitgevoerd. `composer show --direct` meldt hier dat dependencies niet geïnstalleerd zijn; `composer show --locked --direct` en de referentielockfiles bepalen de vastgelegde versies. Runtimegedrag, daadwerkelijke productieconfiguratie, GitHub-branchbescherming, CI-resultaten, geheime waarden en de volledige historie zijn niet geverifieerd. Dit is geen penetratietest of volledige security-audit.

Boost `search-docs` is niet beschikbaar in deze sessie. Voor versiegevoelige aanbevelingen zijn de officiële [Laravel 13 Eloquent-documentatie](https://laravel.com/docs/13.x/eloquent), [validatiedocumentatie](https://laravel.com/docs/13.x/validation), [queuedocumentatie](https://laravel.com/docs/13.x/queues) en [Inertia 3 formulieren](https://inertiajs.com/docs/v3/the-basics/forms) geraadpleegd. Frameworkdocumentatie bevestigt mogelijkheden; zij bewijst geen bestaande Rapide-conventie.

### Versiebasis

| Onderdeel | Moto2Go lockfile / manifest | Blackbirds lockfile / manifest | Boilerplate lockfile / manifest |
| --- | --- | --- | --- |
| PHP-eis | `^8.2.0` | `^8.5` | `^8.3`; AGENTS.md en container/CI richten zich op PHP 8.5 |
| Laravel | `12.47.0` | `12.58.0` | `13.35.0` |
| Frontend | Livewire `4.4.7`, Mix `6.0.49`, Blade | Vue `3.5.34`, Vite `3.2.11` | Vue 3, TypeScript 5, Vite 8 volgens package.json |
| Inertia | Niet aanwezig | Server `2.0.24`, Vue-adapter `1.3.0` | Server `3.5.1`, Vue-adapter `^3.0.0` |
| Routes in frontend | Blade/JS | Servergestuurde URL-props | Wayfinder `0.1.21` en Vite-plugin |
| Tests | PHPUnit `11.5.47` | PHPUnit `11.5.55` | PHPUnit `12.5.38` |
| PHP-formattering | PHP-CS-Fixer als dev dependency | Geen afzonderlijke formatter gevonden | Pint `1.32.1`, Laravel-preset |

De PHP-eis van T en de Inertia-majorversies van B zijn expliciete verschillen tussen configuratieonderdelen; dit rapport verklaart ze niet zonder runtimeonderzoek. Nieuwe regels volgen de boilerplate en Boost, en kopiëren geen versiegebonden syntax uit M of B.

## 1. Applicatiestructuur en architectuur

**Moto2Go-aanpak.** `app/Models` bevat 54 PHP-files, `Http/Controllers` 109, `Repositories` 89, `Services` 73 en `Livewire` 113. Repositories combineren Prettus-contracten, Eloquent CRUD en gespecialiseerde zoekqueries. Services gebruiken zowel vlakke classes als `Interfaces`/`Implementation`-mappen. Lease-request/response-DTOs zijn lokaal bij de integratie ondergebracht. Verder: 17 enums, 15 policies, 4 exception-files inclusief Handler, 6 events, 5 listeners, 11 jobs, 13 observers, Helpers en Traits. Geen eigen Actions-, Queries- of Support-map aangetroffen.

**Blackbirds-aanpak.** `app/Clients` bevat 270 PHP-files, Repositories 109, Builders 59, Persistors 60, Validators 96, Transformers 199 en Services 83. Models (106 PHP-files) zijn verdeeld over Eloquent, Interfaces, DTO en Enums. Controllers (122) en policies (75) volgen Backend/Frontend/Api en organisatie/website/widget-grenzen. Verder: 13 job-files inclusief BaseJob, 5 exception-files inclusief Handler, Helpers en Traits. Geen applicatie-Events-, Listeners-, Actions-, Queries- of Support-map aangetroffen. `BaseBuilder` breidt Eloquent uit; dit is een queryabstractie, maar geen aparte Query Objects-laag.

**Overeenkomsten.** Eloquent blijft de persistencebasis. Beide hebben betekenisvolle integratieservices, dependency injection en domeingerelateerde submappen, naast aanzienlijke repositorylagen. Geen gedeelde, bewezen Action-standaard of vaste `handle`/`execute`-conventie.

**Verschillen.** M leunt op een repositorypackage en heeft een event/observerstructuur; B bouwt een eigen infrastructuur rond lezen, schrijven, validatie en serialisatie. M heeft compacte readonly lease-DTOs; B gebruikt ook mutable DTOs en interfaces voor eenvoudige waarden.

**Technical-debt-signalen.** `M:app/Repositories/Eloquent/UserRepository.php` bevat wrappers rond `User::find`, `update` en `destroy`, naast nuttige joins en filters. `B:app/Builders/BaseBuilder.php` voegt schema-inspectie en soft-deletefiltering toe; `B:app/Persistors/BasePersistor.php` start transacties rond individuele saves en vertaalt exceptions naar booleans. Dat vraagt extra onderhoud en kan de werkelijke foutoorzaak of transactiegrens verhullen. Niet iedere bestaande abstractie is daarom een geschikt templateonderdeel.

**Aanbevolen standaard en motivatie — A1.** Start with Laravel conventions and introduce abstractions only when the domain or infrastructure complexity justifies them. Eenvoudige endpointgebonden CRUD mag rechtstreeks via Eloquent. Extractie moet aantoonbaar een use case verduidelijken, substantieel hergebruik bieden of een externe grens isoleren. Geen vooraf aangemaakte domain-, repository-, DTO-, service- of querylagen.

| Patroon | Wel gebruiken | Niet als standaard gebruiken | Onderbouwing |
| --- | --- | --- | --- |
| Controller | HTTP-coördinatie en eenvoudige lokale CRUD | Lange berekeningen of integratieprotocollen | M:app/Http/Controllers/Api/AdController.php; B:app/Http/Controllers/Backend/OrganisationController.php tonen omvangrijke coördinatie |
| Action | Eén herkenbare, meerstaps business use case die duidelijker of herbruikbaar wordt | Eén class per model of CRUD-call | Expliciete opdracht; M:app/Services/MessageVerificationService.php toont een kandidaat-use-case, geen bewezen Action-conventie |
| Service | Gedeelde domeinberekening of externe integratie | Generieke doorgeeflaag per model | M:app/Services/Lease/Motor2GoLeaseCalculationService.php, app/Services/Location/GeocodingProviderResolver.php; B:app/Clients/WeFact en app/Services/RateLimit |
| Repository/adapter | Werkelijke infrastructuurgrens, verwisselbare bron of afwijkend opslagprotocol | Eloquent CRUD opnieuw verpakken | Beide repositorylagen plus M:UserRepository; integratieclients tonen de echte grens |
| DTO/data object | Complexe externe payload, normalisatie of een nuttig getypeerd contract | Triviale gevalideerde formulierarrays | M:app/Services/Lease/DTO; B:app/Clients/Akkuro/Requests en app/Models/DTO |
| Query Object | Substantiële herbruikbare query die niet helder in scopes past | Iedere `where` of modelquery | M:app/Repositories/Eloquent/AdRepository.php en B:app/Builders; geen bewezen aparte Queries-laag |
| Event/listener | Meerdere onafhankelijke reacties op een domeingebeurtenis | Eén simpele lokale aanroep | M:app/Providers/EventServiceProvider.php; geen eigen eventlaag in B; selectiecriterium uit opdracht |
| Job | Trage verwerking, retries of werk buiten het request | Snelle synchrone validatie/berekening | M:app/Jobs/RecalculateAdFinancingPrice.php; B:app/Jobs/Api/Widget/Lead |
| Policy | Resource-, eigendoms- en organisatiegebonden toegangsregels | Lege policy per model | Beide app/Policies-mappen; B:app/Policies/Api/Widget/InterestListV2Policy.php |
| Exception | Betekenisvolle fout waarvan callers/rendering behandeling moeten kennen | Generieke catch-all die fouten als succes teruggeeft | M:app/Exceptions/NewsletterSubscriptionException.php; B:app/Exceptions/AkkuroException.php en tests/Feature/Scenarios/AkkuroExceptionHandlingTest.php |
| Domain boundary | Submappen voor werkelijk samenhangende features | Verplichte DDD/moduleboom of modelinterfaces | B:Organisation/Website/Widget en M:Lease/Location tonen context, geen universele layout |
| Transaction | Samenhangende writes die gezamenlijk moeten slagen | Iedere save, externe request of volledige controller | M:app/Services/Hexon en app/Services/MessageVerificationService.php; B:app/Persistors/BasePersistor.php en app/Services/Storage/Implementation/StorageService.php tonen verschillende grenzen |

Bronvoorbeelden: [M UserRepository](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/app/Repositories/Eloquent/UserRepository.php), [B BaseBuilder](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/app/Builders/BaseBuilder.php), [B BasePersistor](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/app/Persistors/BasePersistor.php).

## 2. HTTP-laag

**Moto2Go-aanpak.** Routes zijn verdeeld over web/admin/dashboard/mobile/auth/api. Controllers gebruiken repositories, services en soms Eloquent direct. 33 Form Requests bevatten onder andere login-, advertentie- en landingpagevalidatie, met zowel arrays als stringregels. `LandingPageValidation::authorize()` controleert administratorrechten. De API gebruikt `/v1` voor veel endpoints, Sanctum-middleware en Fractal via `ApiController`; niet ieder endpoint heeft een versieprefix. Middleware en exceptionafhandeling staan in klassieke Kernel/Handler-classes.

**Blackbirds-aanpak.** 120 routefiles worden gedeeltelijk recursief geladen met `FileHelper::rglob`. Backend/Frontend/Api-controllers gebruiken Requests, Persistors, Validators en Transformers. 68 requestfiles bevatten ook formulier-, filter- en widget-state; `BaseRequest::rules()` is leeg. In representatieve schrijfflows voeren Persistors/Validators de validatie uit. Gate-abilities worden in controllers gecontroleerd en in UI-props verwerkt. De API gebruikt eigen JSON-responses, API-key/host-middleware en geen uniforme `/v1`-prefix. Ook hier klassieke Kernel/Handler-classes.

**Overeenkomsten.** Benoemde routes, constructor/method injection, validatieklassen en autorisatie zijn aanwezig. Geen `app/Http/Resources`-laag aangetroffen. Routes en controllers volgen meerdere contexten.

**Verschillen.** M gebruikt native Form Requests duidelijker voor HTTP-validatie; B verplaatst veel daarvan naar een eigen validator/persistorprotocol. M gebruikt Fractal, B eigen Transformers. API-authenticatie en versionering verschillen.

**Technical-debt-signalen.** `M:Http/Requests/StoreAdvertisingRequest.php` is leeg en retourneert `false` voor authorization; dit bewijst geen bruikbare schrijfconventie. `B:Backend/OrganisationController::store()` geeft `$request->all()` door en gebruikt voor een verboden actie een lege ValidationException. Dat vermengt input-, business- en toegangsproblemen. Recursief laden hoeft niet in een kleine template. De oude bootstrapstructuur is ondersteunde bestaande structuur, geen reden om die terug te brengen in T.

**Aanbevolen standaard en motivatie — H1.** Gebruik voor nieuwe schrijfformulieren operationele Form Requests met validatie en expliciete authorization. Kleine, endpointgebonden inlinevalidatie blijft toegestaan. Geef alleen bedoelde gevalideerde velden door; gebruik geen generieke BaseRequest met lege regels als schrijfgrens. Houd resources, autorisatie, businessoperatie en response uit elkaar zonder verplichte extra lagen. Gebruik native fouten voor validatie en toegang, en echte domeinexceptions waar nodig. Laat naamgeving aansluiten op resource/feature en bestaande routes; voeg alleen routefiles toe bij een heldere grens. Boost bepaalt API Resources en API-versionering; geen extra responseframework of afwijkende versioneringsregel. T registreert middleware/exceptions in `bootstrap/app.php`.

Bronvoorbeelden: [M LandingPageValidation](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/app/Http/Requests/Admin/LandingPageValidation.php), [B BaseRequest](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/app/Http/Requests/BaseRequest.php), [B OrganisationController](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/app/Http/Controllers/Backend/OrganisationController.php).

## 3. Database

**Moto2Go-aanpak.** 115 migrations, 40 factoryfiles en 39 seederfiles. Nieuwe Hexon-migrations gebruiken anonymous migrations, constrained foreign keys, expliciete deleteacties, composite uniques/indexes en betekenisvolle nullable relaties. Models gebruiken meestal fillable, `$casts`, relaties en soms SoftDeletes; lokale scopes en repositoryqueries bestaan naast elkaar. Recente services gebruiken `DB::transaction`; MessageVerification gebruikt een row lock. `docs/search-terms.md` motiveert waarom een gewone index geen `%term%`-zoekopdracht helpt.

**Blackbirds-aanpak.** 126 migrations, geen factoryfiles en één lege DatabaseSeeder. Setup verloopt ook via domeincommands zoals `wms`, tenant/user creation en synccommands. Migrations mengen named en anonymous classes; relationele foreign keys, indexes en uniques zijn aanwezig. Eloquent-models implementeren interfaces en gebruiken veelal `guarded = ['id']`, casts, Carbonate/Timestamps-traits en custom Builders. `Organisation::websites()` filtert zelf `deleted_at`; BaseBuilder inspecteert soft deletes. DatabaseService omwikkelt Laravel-transacties; Persistors beheren ook save-transacties.

**Overeenkomsten.** Relationele MySQL-configuratie, Eloquent-relaties, timestamps, nullable velden en schema-invarianten. Queries gebruiken eager loading, pagination en waar nodig joins/aggregates. Beide hebben transactiegebruik, maar geen gedeelde transactiegranulariteit.

**Verschillen.** M heeft native factory/seederinfrastructuur en overwegend allowlisting; B handgemaakte fixtures, synccommands en een eigen model/queryprotocol. Enumverschil: M gebruikt MyCLabs (`AdStatus`); B heeft native backed enums (`OrganisationStatusEnum`) naast helpers/constants. Beide modellen houden vaak `$casts` aan; T gebruikt al `casts()`.

**Technical-debt-signalen.** Frameworkgedrag opnieuw uitvoeren via schema-inspectie en handmatige soft-deletefilters maakt queries lastiger te voorspellen. Transacties rond individuele saves beschermen niet automatisch een volledige meerstaps operatie. M verstuurt notificaties binnen de confirm-transactie: een externe side effect kan niet door een database-rollback worden teruggedraaid. B:StorageService::copy heeft een vroeg `return null` na `beginTransaction()` zonder expliciete rollback in die tak. Dit zijn redenen om transactie-eigenaarschap expliciet te maken, geen bewezen productiestoringen.

**Aanbevolen standaard en motivatie — D1.** Volg Eloquent en de actuele T-casts; gebruik native PHP-enums voor echte gesloten waardesets. Introduceer geen MyCLabs-compatibiliteitslaag of verplichte modelinterfaces. Maak foreign-keydeletegedrag, nullability en uniques onderdeel van de domeinbetekenis; indexeer voor concrete querypatronen. Behoud gebruikelijke timestamps, tenzij de tabel aantoonbaar een andere lifecycle heeft; gebruik soft deletes alleen bij herstel/retentiebehoefte en leg definitief verwijderen vast. Laad benodigde relaties vooraf, begrens lijsten en gebruik scopes voor herhaalde filters. Complexere SQL mag wanneer zij een concrete behoefte oplost; bind waarden en allowlist dynamische identifiers. Eén businessoperatie beheert de transactie; externe side effects volgen pas na succesvolle commit of krijgen expliciete herstelafhandeling. Factories leveren testdata; seeders scheiden noodzakelijke referentiedata van demo-/omgevingsdata en maken geen productieaccounts met vaste credentials.

Bronvoorbeelden: [M Hexon-migration](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/database/migrations/2026_09_14_090000_create_hexon_mismatch_tables.php), [M zoekdocumentatie](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/docs/search-terms.md), [B Organisation-model](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/app/Models/Eloquent/Organisation.php).

## 4. Frontend

**Moto2Go-aanpak.** Blade-views, Livewire-components, Bootstrap/SCSS, Alpine en losse JavaScript via Mix. Geen Inertia/Vue/TypeScript/Wayfinder-pipeline aangetroffen. De PHP Livewire-componenten dragen een deel van de formulier- en UI-logica.

**Blackbirds-aanpak.** Inertia/Vue 3, Vite, JavaScript en SCSS. `resources/js/Pages`, `Layouts` en `Components` zijn hoofdlettergevoelig georganiseerd, met Backend/Frontend-submappen. `app.js` resolveert Pages en registreert een modalplugin en globale formatteermixins. Pages gebruiken `script setup`, `useForm`, foutprops en `processing`; URLs/methodes komen vaak via serverprops zoals `form` en `can`. Geen TypeScript-check, Wayfinder, Pinia/Vuex of eigen composables/stores-map aangetroffen.

**Overeenkomsten.** Server bepaalt routes, inputregels en toegangsrechten; UI hergebruikt onderdelen. Geen gedeelde frontendstack of state-libraryconventie.

**Verschillen.** M is Blade/Livewire, B Inertia/Vue zonder types; T is Inertia 3/Vue/TypeScript met lowercase `pages`, `layouts`, `components`, `composables` en gegenereerde Wayfinder-routes. B geeft routecontracten handmatig via DTO/Transformer-props mee; T genereert ze.

**Technical-debt-signalen.** B combineert server-Inertia 2 met adapter 1 en een Vite 3-toolchain. Dit is een compatibiliteits-/upgradeonderzoekspunt, geen vastgesteld defect. Globale mixins en brede Object-props maken contracten impliciet. `v-html` in widgetbeschrijvingen vraagt een aantoonbare sanitizationgrens; paginationlabels zijn een andere, servergegenereerde bron en niet automatisch hetzelfde risico.

**Aanbevolen standaard en motivatie — F1.** Kies B's Inertia/Vue-richting, aangepast aan T's al gekozen stack en padnamen; importeer geen oude client/configuratie. Types beschrijven props en gedeelde payloadcontracten. Pages coördineren schermen, layouts de shell, components hergebruik en composables gedeeld stateful gedrag. Houd state lokaal of bij formulier/page-props; voeg geen store toe zonder daadwerkelijk paginabreed lifecycleprobleem. Gebruik bestaande UI-components. Server is eigenaar van businessregels; frontendvalidatie ondersteunt alleen interactie. Boost bepaalt Inertia en Wayfinder; Rapide voegt geen parallel handgemaakt routeprotocol toe.

**Expliciete Rapide-keuze.** De gebruiker sluit Livewire uit voor Rapide-projecten. Installeer of gebruik Livewire niet; bouw interactieve applicatie-UI met Vue en Inertia. De Livewire-vermeldingen bij Moto2Go beschrijven uitsluitend de geaudite bron en zijn geen aanbeveling.

Bronvoorbeelden: [M Mix-configuratie](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/webpack.mix.js), [B app.js](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/resources/js/app.js), [B User/Edit.vue](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/resources/js/Pages/Backend/User/Edit.vue).

## 5. Testing

**Moto2Go-aanpak.** PHPUnit, Feature- en Unit-suites, `test_...`-methodes. Featuretests omvatten auth, landingpagevalidatie, bereikbaarheid van advertenties, message verification, financiering, zoekqueries en commands. `RefreshDatabase`, factories, Notification/Http-fakes en Mockery bestaan. Sommige tests bouwen SQLite-schema's handmatig. Unit/UserTest test een lokale mutator zonder appbootstrap; enkele andere Unit-tests gebruiken applicatievoorzieningen.

**Blackbirds-aanpak.** PHPUnit, Feature/Unit en business-/integratietests rond Scenarios, Refinance, Altum, BrainBay en WeFact. `RefreshDatabase`, eigen fixtures, mocks en gerichte WeFact-loggingtests. WeFact-featuretests en Altum-stabilitytests kunnen live APIs aanroepen en skippen zonder configuratie. Sommige Altum-fixtures wijzigen de appomgeving naar `local`, waardoor een middlewarebypass kan gelden. Geen native factories aangetroffen.

**Overeenkomsten.** Beide testen zichtbare businessuitkomsten en failure modes, gebruiken PHPUnit 11 en hebben meer specifieke Featuretests dan de CI-job uitvoert. Geen Pest-afhankelijkheid; B's Composer-allow-plugin-entry alleen bewijst geen Pest-gebruik.

**Verschillen.** M heeft bredere auth-/formdekking en factorygebruik; B legt nadruk op externe berekeningen en live contractstabiliteit. Geen voldoende bewijs voor een gedeelde, complete autorisatiematrix of tenantisolatiedekking. PHPUnit-configuratie verwijst in beide naar een afzonderlijke testomgeving, maar forceert niet overal SQLite; T kiest SQLite in-memory expliciet.

**Technical-debt-signalen.** Alleen Unit draaien mist belangrijke HTTP-/database-regressies. Een live test die zonder key skipt is geen bewijs van contractgezondheid. Handgemaakte schema's kunnen afwijken van migrations. Een testomgeving wijzigen naar local kan beveiligingschecks overslaan; zulke flowtests bewijzen geen productieautorisatie.

**Aanbevolen standaard en motivatie — T1.** Behoud T's PHPUnit. Featuretests dragen HTTP-, database- en Laravel-integratiegedrag; Unit is voor zelfstandige deterministische logica. Cover per gewijzigde beslissing de happy path en relevante authorization-, validation-, business-rule- en failuregevallen, inclusief state/side effects. Gebruik factory-states voor betekenisvolle scenario's en een geïsoleerde database met de projectmigrations; SQLite blijft T's snelle basis, met productie-enginechecks waar enginegedrag relevant is. Fake externe grenzen; neem live contracttests alleen expliciet opt-in op, los van de deterministische mergegate. Geen coveragepercentage of test per frameworkdeclaratie. De testverplichting, generators en runnersyntax blijven eigendom van Boost.

Bronvoorbeelden: [M MessageVerificationTest](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/tests/Feature/Frontend/MessageVerificationTest.php), [B AltumClientStabilityTest](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/tests/Feature/Altum/AltumClientStabilityTest.php), [B fixtures](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/tests/Feature/Altum/Support/CreatesAltumWidgetFlowFixtures.php).

## 6. Codekwaliteit

**Moto2Go-aanpak.** PHP-CS-Fixer is een dev dependency; geen bijbehorende rootconfig/scriptgate aangetroffen. Composer heeft lifecycle-scripts, npm Mix-dev/buildscripts en commitlint/Commitizen-configuratie. Geen Pint, PHPStan/Larastan, Rector, ESLint of TypeScript-check gevonden. CircleCI draait PHPUnit met `--testsuite Unit`, installeert met `--ignore-platform-reqs` en bouwt/releases daarna.

**Blackbirds-aanpak.** Composer-lifecycle en npm `dev`/`build`; geen centrale checkscript of formatter/lintergate. `qodana.yaml` bestaat met PHP 8.1, tegenover PHP `^8.5` in composer.json; geen actieve Qodana-gate aangetoond. Geen Pint, PHPStan/Larastan, Rector, ESLint of TypeScript-check gevonden. CircleCI draait eveneens alleen Unit met platformrequirements genegeerd. Geen bijgehouden pre-commit-hooks gevonden in beide snapshots.

**Overeenkomsten.** Lockfiles en CI bestaan, maar geen complete lokale/CI-qualitygate aangetoond. Packageaanwezigheid of een configfile alleen bewijst niet dat een check wordt uitgevoerd.

**Verschillen.** M heeft committooling en een PHP-CS-Fixer-package; B een Qodana-configuratie. T heeft al Pint, PHPUnit, ESLint, Prettier en `vue-tsc`.

**Technical-debt-signalen.** Testjobnaam `unit` verhult het ontbreken van Feature-uitvoering. Platformrequirements negeren kan runtime-incompatibiliteit maskeren. LTS/latest-buildimages en `npm install` verminderen reproduceerbaarheid; dependency-audits zijn geen zichtbare gate. B's Qodana-PHP-versie wijkt af van de applicatie-eis.

**Aanbevolen standaard en motivatie — Q1.** Gebruik T's bestaande `composer ci:check` als centrale lokale gate. Deze voert npm lint-, format- en typechecks uit, gevolgd door `composer test` (config clear, Pint-check en tests). Vul de mergegate aan met `npm run build` en `composer audit`; daarmee controleert zij ook bundling en PHP-advisories. Behoud de bestaande configs als bron voor stijl. Geen nieuw `composer check`-alias: scripts wijzigen valt buiten de gevraagde Markdown-output. PHPStan/Larastan zijn geen huidige baseline en vereisen eerst een concrete behoefte en dependencygoedkeuring. Rector blijft optioneel voor een gerichte migratie, geen runtime-architectuur of verplichte gate.

**Nog niet geïmplementeerd.** T's CircleCI draait nog alleen Unit en roept `composer ci:check` niet aan. De rules leggen de gewenste gate vast; scripts/CI zijn niet aangepast en de gate is niet uitgevoerd door ontbrekende dependencies. T's Wayfinder-outputs zijn genegeerd: genereer ze vóór typechecking in een schone CI-checkout met de bestaande Vite-plugin of geverifieerde Artisan-command. Frontendbuild heeft daarom ook een werkende PHP/appbootstrap nodig. CI dient lockfile-installaties, passende PHP/extensions en niet-mutaterende checks te gebruiken.

Bronvoorbeelden: [M CI](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/.circleci/config.yml#L97), [B CI](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/.circleci/config.yml#L104), T:composer.json, package.json, pint.json, eslint.config.js, .prettierrc, tsconfig.json en vite.config.ts.

## 7. Security

**Moto2Go-aanpak.** Sessies/Breeze-auth, Sanctum voor APIs, accounttype-middleware, adminchecks, policies en Form Request-authorization. Backendtokencommands maken/revoken tokens met specifieke abilities. MessageVerification gebruikt gehashte tokens, tijdelijke signed URLs en rate limits per IP/email/context. De API-limiter is gedefinieerd, maar de algemene api-middlewaregroep bevat geen `throttle:api`; definitie alleen bewijst geen handhaving. CSRF-excepties omvatten payment/webhook- en Apple-loginroutes. FileService kiest een map op clientextensie; dat is op zichzelf geen inhoudsvalidatie.

**Blackbirds-aanpak.** Eigen auth/JWT/2FA-services, tenantmiddleware, API-key/hostchecks en uitgebreide contextpolicies/gates. ApiKeyMiddleware laat lokaal of bij een bestaande tenanthost door; sommige flowfixtures maken zich local. Dat vraagt aparte productiecontexttests. CSRF-excepties zijn leeg. Uploads gaan via FileValidator/StorageService. OutboundThrottleService bewaakt externe quotas via cachelocks. WeFactClient logt diagnostiek met redaction, terwijl ApiKeyMiddleware een key logt en een LeadForwarder volledige data in een foutpad logt.

**Overeenkomsten.** Auth, authorization, upload/storagevoorzieningen, Sentry/logging en gevoelige domeindata. Beide bevatten `.env.example`/`.env.ci`; M ook `.env.testing`. Testcredentials/configuratie zijn niet automatisch productiegeheimen. Geen conclusie over daadwerkelijk gelekte productiecredentials of deploymentsecrets zonder nader onderzoek.

**Verschillen.** M gebruikt Sanctum; B een eigen API-key/JWT-/tenantmodel. M heeft gerichte throttling en bredere CSRF-uitzonderingen; B heeft expliciet een outbound quota-service. Mass assignment: M meestal fillable, B veelal slechts id guarded. B heeft Vue `v-html`; M Blade-output vereist beoordeling per rendercontext.

**Technical-debt-signalen.** De genoemde key/payloadlogging is concreet ongeschikt als nieuwe standaard. UI-`can`-props, clientvalidatie en hostheaders zijn geen vervanging voor serverauthorization. Brede writable modellen vragen strikte inputselectie. Aanwezige policies met lege methodes in M tonen dat mapaanwezigheid geen dekking bewijst. CSRF-uitzonderingen en HTML-rendering vragen een geverifieerde authenticity-/sanitizationgrens; deze audit bewijst niet dat die ontbreekt in iedere flow.

**Aanbevolen standaard en motivatie — S1.** Behoud T's Fortify/sessieauth; introduceer geen eigen auth/JWT/tokensysteem door referentiecode te kopiëren. Kies API-auth pas bij een echte API-clientbehoefte, met scope/expiry/revocation en serverownershipchecks. Test cross-user/cross-tenant toegang wanneer die context bestaat, ook buiten lokale bypasses. Log allowlisted diagnostiek zonder tokens, credentials of complete persoonsgegevens; gebruik synthetische fixtures. Valideer uploadinhoud/type/grootte, gebruik storagegegenereerde namen en private opslag voor gevoelige bestanden. Behoud CSRF en contextescaping; begrens `v-html` tot expliciet gesaniteerde content. Pas daadwerkelijk aangesloten throttles toe bij login/recovery, publieke formulieren, dure endpoints en relevante outbound quotas. Laat secrets via configuratie lopen en houd ze uit code/props/logs. T bevat ClamAV-integratie als dependency; dat bewijst geen scanpolicy en maakt antivirus niet universeel verplicht.

Bronvoorbeelden: [M MessageVerificationService](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/app/Services/MessageVerificationService.php), [B ApiKeyMiddleware](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/app/Http/Middleware/ApiKeyMiddleware.php), [B WeFactClient](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/app/Clients/WeFact/WeFactClient.php).

## 8. Operations

**Moto2Go-aanpak.** Queueconfig ondersteunt Laravel-drivers met default sync; RecalculateAdFinancingPrice definieert tries, timeout en backoff. Kernel schedules gebruiken veel `withoutOverlapping()`. Kubernetes bevat app/cron/migrationmanifesten en exec-healthchecks. Cronmanifest draait iedere tien minuten `schedule:run`, terwijl de app ook everyMinute-taken definieert: de daadwerkelijke timing behoeft verificatie. Provisioning cached config/views/routes. Sommige services lezen `env()` rechtstreeks; Redis is configureerbaar, niet bewezen productie-default. Sentry rapporteert via Handler.

**Blackbirds-aanpak.** RabbitMQ-package/connection, BaseJob en jobs voor mail, PDFs en leadforwarding. Kernel plant minute/daily/monthly/yearly-commands, waarbij `minute` iedere tien minuten draait; geen overlapbescherming in die scheduledeclaraties. Docker/Compose, `infra`, Kubernetes-provisioning en CircleCI-deploys zijn aanwezig. De bijgehouden `.kubernetes/deploy`-map bevat migration/version-configmanifesten; een app-healthprobe is daar niet aangetroffen. Cache/queues default file/sync in config, Redis optioneel. Sentry en externe clients met timeouts/diagnostiek bestaan.

**Overeenkomsten.** Laravel queues/scheduler, Docker/Kubernetes/CircleCI en configuratiecaching; productie-environment bepaalt daadwerkelijke drivers. Geen basis voor verplichte Redis, Horizon, Octane of Laravel Cloud. Beide hebben oude Kernel/Handlerstructuur.

**Verschillen.** M heeft zichtbaar gedetailleerde scheduler-locks en appprobes; B RabbitMQ en leads/PDF-jobs. T heeft RabbitMQ en Sentry al als dependencies, maar workers/cron staan in CI-parameters standaard uit, en `bootstrap/app.php` registreert `/up`.

**Technical-debt-signalen.** Sync-default maakt dispatch niet vanzelf asynchroon. `after_commit` staat in bekeken referentieconnections false; code mag niet aannemen dat jobs pas na commit draaien. Rechtstreekse `env()`-lezingen conflicteren met configcaching. Een DB-transactie maakt mail, storage of externe facturatie niet atomair. Probe-/Sentry-configuratie bewijst geen werkende monitoring of alerting.

**Aanbevolen standaard en motivatie — O1.** Configureer een echte worker voordat een feature op achtergrondverwerking vertrouwt. Definieer retry/timeout/failuregedrag en idempotentie voor side effects; dispatch commitafhankelijke jobs na commit. Bescherm schedules tegen overlap waar dubbele uitvoering schade doet; stem cronfrequentie op schedules af. Gebruik gedeelde locks bij meerdere nodes wanneer één uitvoering vereist is. Kies cache/queue-infrastructuur op de behoefte, met duidelijke cachekeys/TTL/invalidation en tenantgrenzen. Gebruik config in appcode. Controleer bij deployments migrations, benodigde workerrestarts, scheduler en health; behandel `/up` als applicatiehealth en niet als bewijs dat alle externe diensten werken. Operationsregels staan in backend/security/quality; geen extra operationsbestand.

Bronvoorbeelden: [M scheduler](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/app/Console/Kernel.php), [M cron](https://github.com/rapideinternet/Moto2Go/blob/ee92c3b4d279419b144be1a73a842130af2ec3b4/.kubernetes/deploy/cronjob.yaml), [B queueconfig](https://github.com/rapideinternet/blackbirds-widgets-platform/blob/761b13398c4e902939b9585ec1234e2be22cf1c6/config/queue.php).

## 9. Git en workflow

**Moto2Go-aanpak.** Defaultbranch development; releaseconfig richt zich op staging/production met semantic-release. Recente geschiedenis toont `tickets/M2G-376`, `small_textchange`, `fix:`-subjects en mergecommits; commitlint/Commitizen-config aanwezig. Geen bijgehouden PR-template of hook aangetroffen.

**Blackbirds-aanpak.** Defaultbranch development; staging/production-releaseconfig en CircleCI. Geschiedenis toont onder andere `origin/BWI-308`, `wefact_check`, `fix(wefact): ...`, `feat: ...` en mergecommits. Geen bijgehouden PR-template of hook aangetroffen; `npx semantic-release` in CI is geen bewijs van lokaal vastgepinde release-tooling.

**Overeenkomsten.** Conventional Commit-achtige subjects en environmentreleasebranches; PR-mergecommits zijn zichtbaar. Beide CI-configs koppelen releases aan de beperkte Unit-job.

**Verschillen.** Branchbenamingen zijn niet uniform en commits voldoen niet overal aan een strikte machinegate. Tooling verschilt. Repositorysettings, vereiste reviews, squash/rebase/mergebeleid en beschermingsregels zijn onbekend.

**Technical-debt-signalen.** Uit enkele branchnamen geen organisatiebrede Git-flow afleiden. Een zichtbare mergecommit legt geen verplichte mergestrategie vast. Een pipelineafhankelijkheid van Unit alleen is onvoldoende voor nieuwe HTTP-features.

**Aanbevolen standaard en motivatie — G1.** Gebruik korte featurebranches op de projectbasebranch; neem een ticket op wanneer aanwezig, zonder verplichte naamstructuur. Conventional Commits volgen de expliciete Rapide-instructie in dit verzoek; releasecompatibiliteit ondersteunt die keuze. PRs beschrijven probleem, gedrag, verificatie en relevante risico's. Vereis de gekozen qualitygate voor merge en release; claim niet dat branchbescherming dit al afdwingt. Behoud staging/production alleen waar deployment die branches gebruikt. Schrijf geen verplichte squash/rebase, reviewaantal of uitgebreide Git-flow voor.

Bronnen: recente Gitlog op de gepinde snapshots, beide package.json-releaseconfiguraties, `.circleci/config.yml` en `M:commitlint.config.js`.

## 10. Documentatie en AI

**Moto2Go-aanpak.** README is grotendeels de Laravel-skeletontekst. `docs/search-terms.md` documenteert een concrete business-/querykeuze en de indexmotivatie. Geen AGENTS.md, `.ai`, `.agents` of projectskills aangetroffen in de geaudite snapshot.

**Blackbirds-aanpak.** README beschrijft lokale/Docker-setup, tenant/organisatie/widgetcommands; `scenarios.md` en Altum-fixturedocumentatie vullen specifieke domein-/testcontext aan. Geen AGENTS.md, `.ai`, `.agents` of projectskills aangetroffen.

**Overeenkomsten.** Specifieke documentatie biedt nuttiger bewijs dan de algemene README. Geen bestaande gezamenlijke AI-ruleconventie uit de referenties af te leiden.

**Verschillen.** M heeft een gericht document onder docs; B setup- en scenarioinformatie verspreid over root/tests. T heeft uitgebreide Boost AGENTS.md, `.ai/skills` en aanvankelijk geen `.ai/rules`.

**Technical-debt-signalen.** Geen gedocumenteerde gedeelde architectuurnorm in de referenties. T's laravel-architecture-skill noemt onder andere een Action-package, Octane, PostgreSQL en generieke seeder/factory-/soft-deletestandaarden die niet door beide bronnen of T worden gedragen; deze audit neemt die niet over. De infer-conventions-skill documenteert alleen de bestaande meerderheid en vraagt expliciete activatie; dit verzoek kiest juist onderbouwde verbeteringen en gebruikt die workflow daarom niet.

**Aanbevolen standaard en motivatie — AI1.** Gebruik `.ai/rules/index.md` als expliciete pad/concernmapping; raadpleeg alle toepasselijke files, ook bij security-/operationsimpact buiten een primaire glob. Laat expliciet project-specifieke afspraken in docs voorgaan op generieke Rapide-conventies binnen hun beschreven scope. Onderzoek bestaande structuur voordat iets wordt toegevoegd. Boost blijft eigenaar van frameworksyntax, generators, packageversies, PHPUnit/Pint-instructies, Inertia en Wayfinder. Geen gekopieerde Boost-ruleboom of tweede skillset.

## Besluit en traceerbaarheid van de rules

| Rulebestand | Basis in deze audit | Bewust weggelaten |
| --- | --- | --- |
| index.md | AI1, alle concerns en de expliciete opdracht | Automatische globmatching claimen of niet-bestaande Boost-rulefiles linken |
| architecture.md | A1, H1, D1 | Verplichte Actions/Repositories/Services/DTOs, packages, moduleboom en methodenamen |
| backend.md | H1, O1 en de HTTP-grens uit S1 | Fractal/custom responseframework, generieke BaseRequest/Validator/Persistor, oude Kernels |
| database.md | D1, querybewijs en transactieproblemen | SoftDeletes/UUIDs op ieder model, index op iedere kolom, interfaces per model |
| frontend.md | F1 en de actuele T-bestanden | Mix/Livewire als templatebaseline, oude Inertia-config, mixins/storeverplichting, handmatige route-DTOs |
| testing.md | T1 en concrete tekortkomingen uit Q1/S1 | 100%-coverage, frameworkdeclaratietests, ongecontroleerde live APIs en local-authbypasses |
| security.md | S1, mass assignment en concrete logging/auth/uploadgevallen | Domein-PII, eigen JWT als standaard, universeel verplichte antiviruspackage |
| quality.md | Q1, O1 en bestaande T-scripts | Niet-bestaand composer check, verplichte nieuwe analyzers/Rector/Qodana |
| git-workflow.md | G1 en de expliciete Conventional Commits-instructie | Gefingeerde branchbescherming, vaste branchprefix, verplichte mergevorm |

Deze rules zijn **gekozen normen voor nieuwe projecten**, geen claim dat M/B er volledig aan voldoen. Actions, Query Objects, adapters en meervoudige eventreacties zijn voorwaardelijke opties uit de opdracht, geen toegevoegde baselagen. Security- en qualitynormen zijn rechtstreeks gekoppeld aan geobserveerde risico's of bestaande templatevoorzieningen.

Eindcontrole: alle acht domeinfiles staan in de index; globs dekken app, routes, database, resources, tests, config/bootstrap, CI en deployment. Security en quality zijn aanvullend per concern. Transactiebeslissingen hebben één eigenaar in database.md; authorization/mass assignment één in security.md. Frameworkinstructies blijven in AGENTS.md/Boost. Alleen dit rapport en de negen gevraagde `.ai/rules/*.md`-bestanden zijn toegevoegd. Er is geen applicatielogica, dependency, script of CI-config gewijzigd. Implementatie en uitvoering van de uitgebreidere CI-gate zijn toekomstige werkzaamheden, buiten deze output.
