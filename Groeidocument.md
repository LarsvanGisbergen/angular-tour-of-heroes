# Groeidocument Internet Technologie
**Lars van Gisbergen**




## The hero editor

Om van een hero een object met variabelen te maken zetten we in de HeroesComponent een hero die de Hero interface implementeert. Deze interface bevat de variabelen: "id" en "name". 
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

Om onderdelen van elkaar te scheiden kunnen losse componenten worden gegenereerd. Deze componenten krijgen hun eigen css,html en typescript files. De componenten kunnen onderling informatie en data delen. Ook kunnen ze geactiveerd worden wanneer gewenst. Als voorbeeld kan een detailed component worden gebruikt vanuit de heroes component, dat ziet er als volgt uit in de heroes.component.html:
```javascript
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
``` 
Wanneer er een hero uit de lijst wordt geselecteerd wordt de html vanuit hero-detail getoond. de hero-detail html bevat:
```javascript
<div *ngIf = "hero">
    <h2>{{hero.name | uppercase}} Details</h2>
    <div><span>id: </span>{{hero.id}}</div>
    <div>
    <label for="hero-name">Hero name: </label>
    <input id="hero-name" [(ngModel)]="hero.name" placeholder="name">
    </div>
  </div>
``` 
Wanneer er een hero gekozen is zal het id en de naam van de hero getoond worden. 

## Add services
Services worden gebruikt om informatie te delen tussen klassen die verder niks van elkaar weten. 
Het genereren van een service geeft de volgende typescript:
```javascript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class HeroService {

  constructor() { }
}

```
We vullen deze code aan om uiteindelijk heroes op te kunnen halen. Een simpele get functie krijgt dit al voor elkaar omdat we eerder een lijst met mock heroes hebben gemaakt die we hier kunnen importen en gebruiken als volgt:
```javascript
  getHeroes(): Hero[]{
    return HEROES;
  }
```
Deze functie kan nu worden gebruikt vanuit de heroes.component.ts door de HeroService mee te geven als parameter in de constructor. Binnen de klasse wordt opnieuw een "getHeroes()" functie gedifinieerd die verwijst naar de functionaliteit in de HeroService als volgt:

```javascript
getHeroes(): void{
    this.heroes = this.HeroService.getHeroes();
  }
```
Omdat we willen dat de lijst bij het inladen van de pagina wordt opgehaald wordt de functie aangeroepen in de "ngOnInit()" functie van de klasse. 

In een echte applicatie wordt er gewerkt met Observables omdat je niet kan garanderen dat je meteen data ontvangt vanuit bijvoorbeeld een server. 
We vervangen de getHeroes() functionaliteit met code die wel rekening houdt met de Observable data:

```javascript
  getHeroes(): Observable<Hero[]> {
    const heroes = of(HEROES);
    return heroes;
  }
```
Omdat er nu een Observable type data wordt opgehaald moet dit in de heroes.component.ts ook worden vastgesteld. Er moet nu worden gesubscribed naar een functie zodat de data op elk moment kan worden ingelezen en gerefreshed. Het simuleert als het ware data die constant kan worden uitgelezen vanuit een server. De nieuwe getHeroes() functie ziet er als volgt uit:

```javascript
  getHeroes(): void {
    this.heroService.getHeroes()
        .subscribe(heroes => this.heroes = heroes);
  }
```
## Add navigation

## Get data from a server
