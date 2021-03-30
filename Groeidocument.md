# Groeidocument Internet Technologie
**Lars van Gisbergen**




## The hero editor

Om een van een hero een object met variabelen te maken zetten we in de HeroesComponent een hero die de Hero interface implementeert. Deze interface bevat de variabelen: "id" en "name". 
```javascript
 export class HeroesComponent implements OnInit {

  hero: Hero = {
    id: 1,
    name: 'Lars'
  };

  constructor() {}

  ngOnInit(): void {
    
  }

}
```

Om de hero die nu attributen heeft ook echt te laten zien op de webpagina wordt de volgende code toegevoegd aan heroes.component.html:

```javascript
<h3>{{hero.name}} Details</h3>
<div><span>id: </span>{{hero.id}}</div>
<div><span>name: </span>{{hero.name}}</div>

 ```
Hierdoor wordt de naam en id getoond. 

Om live de naam van de hero aan te kunnen passen moet er een two-way binding opgezet worden. Hierdoor worden aanpassingen op het scherm getoond en input vanuit het scherm naar de code gestuurd. Dit gaat als volgt:

```javascript
<div>
    <label for="name">Hero name: </label>
    <input id="name" [(ngModel)]="hero.name" placeholder="name">
  </div>
 ``` 
 Een label staat nu op het scherm waarin de gebruiker gemakkelijk de naam van de hero aan kan passen. Deze naam wordt live geupdate. 

## Display a list
Om aan de slag te kunnen met een lijst van heroes zullen er mock heroes moeten worden aangemaakt om de lijst mee op te vullen. Hiervoor wordt de volgende array gebruikt:
```javascript
import { Hero } from './hero';

export const HEROES: Hero[] = [
  { id: 11, name: 'Dr Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
 ```
 De heroes in de lijst maken allemaal gebruik van de eerder gedeclareerde Hero interface. Deze interface moet hier dus ook worden geimporteerd voor deze kan worden gebruikt. 

 Nu de lijst aangemaakt en opgevuld is kan hij worden gebruikt als volgt:
 ```javascript
   <h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```
De *ngFor zorgt ervoor dat alle items in de lijst doorlopen en op het scherm getoond worden. Stond deze term er niet bij dan zou alleen de ge-hardcode hero: "Lars" getoond worden. 

De styling van alle onderdelen in een html file kan worden gedaan met css. De css van een bijbehorende html file bepaald de layout en opmaak van een onderdeel die wordt aangegeven in de html file. In mijn code is een goed voorbeeld de klasse: "heroes". In de css file wordt de opmaak van deze klasse uitgevoerd en heeft zo alleen effect op de "My Heroes" lijst.

Wanneer er een hero uit de lijst wordt aangeklikt vanuit de gebruiker gebeurt er niks. Er zit namelijk nog geen functionaliteit voor de klik in de code. Deze is gemakkelijk toe te voegen door het implementeren van een "onSelect()" functie. Die ziet er als volgt uit:
```javascript
 selectedHero?: Hero;
  onSelect(hero: Hero): void {
    this.selectedHero = hero;
  }
```
De functie wordt gekoppeld aan de onderdelen in de lijst doormiddel van het toevoegen van de volgende code in de html file:
```javascript 
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
```
Hier wordt de bijbehorende functie uitgevoerd indien er een "Click" is geregistreerd op een hero. 

Om gekozen onderdelen in een lijst te highlighten kan er een onderdeel uit de css file worden ingeschakeld. In dit geval bevat de css het onderdeel "selected". Deze kan worden gebruikt voor een gekozen onderdeel van de lijst door het volgende code fragment toe te voegen aan de lijst parameters in de html:
```javascript
[class.selected]="hero === selectedHero" 
```
Wanneer er nu een selectedHero vaststaat zal deze als "selected" worden gezien vanuit de css en een eigen opmaak krijgen. 

## Creat a feature component


## Add services

## Add navigation

## Get data from a server
