# Rapide Laravel template

Laravel template voor Rapide, om gemakkelijk nieuwe projecten op te zetten.

## Stappen om mee te beginnen

1. Fork deze repository met de naam van het project (gebruik '-' als spatie/delimiter)
2. Verander alle instanties van {project-name} in het project naar je projectnaam
3. Controleer de CircleCI config qua instellingen
4. Update de omschrijving in deze readme en de productie url
5. Voeg een staging en production branch toe aan de repository
6. Voeg 'Rapide Team' toe met 'maintain' rol aan de repository
7. Initialiseer het project in CircleCI met de config.yml in de development branch
8. Importeer environment variabelen van een ander Rapide project in de CircleCI instellingen van het project
9. Zet het project in het Kubernetes cluster op
10. Verwijder deze en bovenstaande tekst om een nette readme over te houden

# {project-name}

{ Omschrijving van het project }

##### URL's: 
- Staging: {project-name}.k8s.rapide.software
- Productie: {productie url}

## Vereisten
- PHP 8.5
- Composer 2
- NPM >= 24
- MySQL/MariaDB

## Setup
Stappen om het project lokaal op te zetten:

1. git clone
2. composer install
3. npm install
4. copy .env.example into .env
5. set up config in .env (url, database, etc)
6. php artisan key:generate
7. npm run build
8. php artisan migrate
9. php artisan db:seed

## Architectuur
- Laravel 13, met InertiaJS en Vue3.
- Frontend met Reka UI components.

