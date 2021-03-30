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

Nu gaan we een berichten service toevoegen die bij het ophalen van de heroes list een conformatie geeft. Deze nieuwe service wordt in de bestaande hero service gezet zodat elke instantie van de hero service automatisch ook een message service heeft. De functionaliteit wordt toegevoegd aan de getHeroes functie in de hero service als volgt:

```javascript
getHeroes(): Observable<Hero[]> {
    const heroes = of(HEROES);
    this.messageService.add('HeroService: fetched heroes');
    return heroes;
  }
```
De berichten service kan ook worden ingezet om bijvoorbeeld het id van een gekozen hero in de berichten cache te zetten. dit gaat als volgt:

```javascript
 onSelect(hero: Hero): void {
    this.selectedHero = hero;
    this.messageService.add(`HeroesComponent: Selected hero id=${hero.id}`)
  }

```



## Add navigation

Om gemakkelijk tussen componenten te switchen moet er een duidelijke navigatie zijn. Hiervoor wordt routing gebruikt. Er kan zo in de code duidelijk worden gemaakt hoe de webapp tussen componenten navigeerd. 

Met behulp van routing kan een specifiek component worden gekoppeld aan een path:

```javascript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HeroesComponent } from './heroes/heroes.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
Er kan nu met bijvoorbeeld de volgende url worden genavigeerd naar het onderdeel HeroesComponent die in de voorafgaande onderdelen is ontwikkeld: "http://localhost:4200/heroes".

Een gebruiker een url laten invoeren om te navigeren is niet ideaal, vandaar dat we een dashboard gaan opzetten met de componenten erin verwerkt. De dashboard typescript code ziet er als volgt uit:

```javascript
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: ['./dashboard.component.css']
})
export class DashboardComponent implements OnInit {

  constructor(private heroService: HeroService) { }

  ngOnInit(): void {
    this.getHeroes();
  }

  heroes : Hero[] = [];
 
  getHeroes(): void {
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes.slice(1, 5));
  }
}

```
De getHeroes() functie kan nu wel in de init worden gezet omdat er een callback wordt aangemaakt. Hierdoor zal de actuele data altijd worden getoond, ook nadat de init functie is beindigd. 

Om het dashboard als startpagina te kunnen tonen moet de app-routing.module worden aangepast:

```javascript
const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'heroes', component: HeroesComponent },
  { path: 'dashboard', component: DashboardComponent}
];
```
De webpagina weet nu dat het dashboard als default pagina staat. Ook krijgt het dashboard haar eigen url: "http://localhost:4200/dashboard". Wat vanaf nu standaard zal worden voor elke component. 

Deze manier van url verbinden met componenten werkt ook voor de componenten in onze heroes lijst namelijk de detailed views, door in de lijst routerlinks te zetten kan elk item in de lijst worden gekoppeld aan zijn eigen pagina, dat gaat als volgt:

```javascript
<h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{{hero.id}}">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </a>
  </li>
</ul>

```
Om ook de daadwerkelijk bijbehorende data van de gekozen hero te tonen moet de hero met de aangegeven id worden opgehaald via HeroService. Deze service bevat namelijk de lijst met alle heroes. 
(Ik had problemen tijdens het uitwerken vandaar dat ik de hero met id = 11 heb gehardcode zodat ik de opdracht af kon ronden). De code voor het koppelen aan de details pagina werkt als volgt:

```javascript 
  @Input() hero? : Hero;

  constructor(
    private route: ActivatedRoute,
    private heroService: HeroService,
    private location: Location
  ) {}

  ngOnInit(): void {
    this.getHero();
  }
  
  getHero(): void {
    const id = 11;
    this.heroService.getHero(id)
      .subscribe(hero => this.hero = hero);
  }

  goBack(){
    this.location.back();
  }
 
```
Hierboven is ook te zien dat er een goBack() functie in de component zit verwerkt. Deze maakt gebruik van de Location module om te bepalen vanuit welke pagina de gebruiker komt en redirect de gebruiker daar terug naartoe. 



## Get data from a server
