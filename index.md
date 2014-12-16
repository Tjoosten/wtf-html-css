---
layout: default
---

### Inhoudstafel

- [Verklaar een doctype](#doctype)
- [Kadermodel wiskunde](#box-model-math)
- [Rem eenheden en Mobile Safari](#rems-mobile-safari)
- [Zwevende elementen eerst](#floats-first)
- [Floats en clearing](#floats-clearing)
- [Floats end computed height](#floats-computed-height)
- [Floated are block level](#floats-block-level)
- [Vertical margins often collapse](#vertical-margins-collapse)
- [Stylen van tabel rijen](#styling-table-rows)
- [Firefox en `<input>` knoppen](#buttons-firefox)
- [Firefox inner outline on buttons](#buttons-firefox-outline)
- [Zet altijd een `type` op `<button>`s](#buttons-type)
- [Internet Explorer's selector limit](#ie-selector-limit)
- [Position explained](#position-explained)
- [Position and width](#position-width)
- [Fixed position and transforms](#position-transforms)


<a name="doctype"></a>
### Verklaar een doctype
Verklaar altijd een doctype. Ik raad aan de simple HTML5 doctype:

```html
<!DOCTYPE html>
```

[Overslaan van de doctype kan fouten veroorzaken](http://quirks.spec.whatwg.org) bv. misvormde tabellen, invoervelden, en de pagina zal ook verkeerd kunnen genereren.

<a name="box-model-math"></a>
### Kadermodel wiskunde
Elementen dat een vaste breedte hebben worden *breder* wanneer ze een `padding` en/of `border-width` hebben. Om deze problemen te voorkomen, kunt u [`box-sizing: border-box;` reset](http://www.paulirish.com/2012/box-sizing-border-box-ftw/) gebruiken.

<a name="rems-mobile-safari"></a>
### Rem eenheden en Mobile Safari
Terwijl Mobile Safari het gebruik van `rem`s eenheden ondersteund in alle eigenschappen, maar ze veroorzaken blijkbaar problemen als ze gebruikt worden in dimentionele media zoektermen waar ze ervoor zorgen dat de tekst de hele tijd van formaat verandert.

Voor nu, gebruik `em`s in plaats van `rem`s.

```css
html {
  font-size: 16px;
}

/* Veroorzaakt knipperende bug in Mobile Safari */
@media (min-width: 40rem) {
  html {
    font-size: 20px;
  }
}

/* Werkt perfect in Mobile Safari */
@media (min-width: 40em) {
  html {
    font-size: 20px;
  }
}
```

**Help!** *Als je een link hebt naar een Apple of Webkit-bug rapport, dan wil ik ze graag insluiten. Ik weet niet goed waar het te rapporteren aangezien het alleen van toepassing is op Mobiel en niet desktop, Safari.*


<a name="floats-first"></a>
### Zwevende elementen eerst
Zwevende elementen zouden altijd eerst moeten staan in de document order. Zwevende elementen zouden altijd iets moeten bevatten om er rond te verpakken. Andere kunnen ze een stap terug effect veroorzaken. In de plaats van op het einde te verschijnen.

```html
<div class="parent">
  <div class="float">Float</div>
  <div class="content">
    <!-- ... -->
  </div>
</div>
```


<a name="floats-clearing"></a>
### Floats and clearing
If you float it, you *probably* need to clear it. Any content that follows an element with a `float` will wrap around that element until cleared. To clear floats, use one of the following techniques.

Als het zweeft, zul u deze* waarschijnlijk* nodig hebben om het te wissen . Alle inhoud die een element volgt met een `float` zal ingewiikeld blijven rond dat element tot gewist. Om zwevende elementen te wissen , gebruikt u een van de volgende technieken .

Gebruik [de micro clearfix](http://nicolasgallagher.com/micro-clearfix-hack/) Om al je zwevende te wissen met een aparte class.

```css
.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}
```

Als alternatief geef ` overflow` , met ` auto` of ` hidden` , op de parent class.

```css
.parent {
  overflow: auto; /* clearfix */
}
.other-parent {
  overflow: hidden; /* clearfix */
}
```

Be aware that `overflow` can cause other unintended side effects, typically around positioned elements within the parent.
Wees ervan bewust dat `overflow` andere onbedoelde bijwerkingen kan bevatten, deze doen zich meestal voor rond meestal rond gepositioneerde elementen.

**Pro-Tip!** *Keep your future self and your coworkers happy by including a comment like `/* clearfix */` when clearing floats as the property can be used for other reasons.*


<a name="floats-computed-height"></a>
### Floats and computed height
Een hoofd element dat alleen zwevende elementen bevat, zal een berekende `heigt: 0;`. bevatten. Voeg een clearfix toe om browsers te forceren om een hoogte te berekenen.

<a name="floats-block-level"></a>
### Floated elements are block level
Elementen met een `float` zullen automatisch een `display: block;` bevatten. Verifieer geen float en display: block; als het niet nodig is. `float` zal dan `display` overschrijven

```css
.element {
  float: left;
  display: block; /* Niet perse nodig */
}
```

**Fun fact:** *Jaren geleden, moesten we `display: inline;` gebruiken voor de meeste zwevende elementen om [dubbele margin bug](http://www.positioniseverything.net/explorer/doubled-margin.html) te vermijden in IE6. Alhoewel deze dagen al lang geleden zijn.*


<a name="vertical-margins-collapse"></a>
### Vertically adjacent margins collapse
Top en bodem margins op aangrenzende elementen (de eene achter de andere) kunnen en willen ineen storten. maar zullen nooit zweven of absoluut gepositioneerde elementen worden.
[Lees dit MDN article](https://developer.mozilla.org/en-US/docs/Web/CSS/margin_collapsing) of de CSS2 specificatie's [collapsing margin section](http://www.w3.org/TR/CSS2/box.html#collapsing-margins) Om meer te weten te komen.

Horizontale aangrenzende margins zullen **nooit instorten**

<a name="styling-table-rows"></a>
### Stylen van tabel rijen
Table rijen, `<tr>`s, zullen geen `border` bevatten. Tenzij je `border-collapse: collapse;` gebruikt. op het hoofd element `<table>`. Meer informatie, als het `<tr>` attribuut en het sub attribuut `<td>` of `<th>` *dezelfde* `border-width` bevatten. De rijen zullen niet zien dat hun grens toegepast word. [Zie deze JS Bin als voorbeeld.](http://jsbin.com/yabek/2/)

<a name="buttons-firefox"></a>
### Firefox en `<input>` knoppen

Voor onbekende redenen, Voegt Firefox een `line-height` toe aan de submit and knoppen `<input>`s. Deze kan niet overscheven worden door aangepaste css code. Je hebt twee opties voor het omgaan met dit:

1. Blijf bij `<button>` elementen.
2. Gebruik `line-height` niet voor je knoppen.

Als u voor de eerste optie gaat (en ik raad deze optie aan omdak `<button>`s geweldig zijn), is hier wat je moet weten.

```html
<!-- Niet zo goed-->
<input type="submit" value="Save changes">
<input type="button" value="Cancel">

<!-- Super goed overal -->
<button type="submit">Save changes</button>
<button type="button">Cancel</button>
```

Als u voor de 2de optie gaat. Zet dan geen `line-height` en gebruik *alleen* `padding` om de knop tekst verticaal te plaatsen. [Bekijk deze JS Bin als voorbeeld](http://jsbin.com/yabek/4/) in Firefox om het orginele probleem en zijn workaround te bekijken.

**Goed nieuws!** *Het lijkt erop dat er een [oplossing](https://bugzilla.mozilla.org/show_bug.cgi?id=697451#c43)  komt in Firefox 30. Dat is goed nieuws voor de toekomst. Maak hou het in het achterhoofd dat het geen oplossing is voor oudere versies/*

<a name="buttons-firefox-outline"></a>
### Firefox inner outline on buttons

Firefox [Voegt inner outline toe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button#Notes) aan knoppen (`<input>`s en `<button>`s) on `:focus`. Blijkbaar is het voor de toegankelijkheid , maar de plaatsing lijkt nogal vreemd . Gebruik deze CSS om het te negeren :

```css
input::-moz-focus-inner,
button::-moz-focus-inner {
  padding: 0;
  border: 0;
}
```


U kunt deze correctie zelf in actie zien in dezelfde [JS Bin voorbeeld]( http://jsbin.com/yabek/4/ ) vermeld in de vorige paragraaf.

**Pro-Tip!** *Zorg ervoor dat u een aantal focus elementen hebt onder andere op de knoppen , links, en ingangen . Het verstrekken van een affordance voor de toegankelijkheid van het grootste belang , zowel voor professionele gebruikers die tab door de inhoud en mensen met een visuele handicap .*


<a name="buttons-type"></a>
### Zet altijd een `type` op `<button>`s

De standaard waarde is `submit`, met de bedoeling dat elke knop in een form gegevens kan bevestigen. Gebruik `type="button"` voor alles dat de gegevens niet hoeft te bevestigen.

```html
<button type="submit">Sla op</button>
<button type="button">Annuleer</button>
```

Voor acties dat een `<button>` nodig hebben. en niet een form staan. Gebruik de `type="button"`.

```html
<button class="dismiss" type="button">x</button>
```

**Fun fact:** *Blijkbaar ondersteunt IE7 de `value` attributen niet zo goed. op `<button>`s/ In plaats van het lezen van de inhoud van het attribuut, trekt IE7 het uit de innerHTML (de inhoud tussen de opening en sluiting van de `<button>` labels). Echter, zie ik dit niet als grote zorg om de twee volgende redenen: IE7 gebruikers aantallen is sterk naar beneden gegaan, en het lijkt nogal ongewoon om zowel een `value` en de innerHTML op een `<button> in te stellen.*

<a name="ie-selector-limit"></a>
### Internet Explorer's selector limit
Internet Explorer 9 and below have a max of 4,096 selectors per stylesheet. There is also a limit of 31 combined stylesheets and `<style></style>` includes per page. Anything after this limit is ignored by the browser. Either split your CSS up, or start refactoring. I'd suggest the latter.

As a helpful side note, here's how browsers count selectors:

```css
/* één selector */
.element { }

/* Twee selectors */
.element,
.other-element { }

/* Drie selectors */
input[type="text"],
.form-control,
.form-group > input { }
```


<a name="position-explained"></a>
### Position explained
Elementen met ee, `position: fixed;` zijn geplaatst in the borwser viewport, Elementen met `position: absolute;` zijn geplaatst bij de dichts bijzijnde hoofd class met een positie die anders is dan `static` (e.g. `relative`, of `fixed`).

<a name="position-width"></a>
### Position and width
Don't set `width: 100%;` on an element that has `position: [absolute|fixed];`, `left`, and `right`. The use of `width: 100%;` is the same as the combined use of `left: 0;` and `right: 0;`. Use one or the other, but not both.


<a name="position-transforms"></a>
### Fixed position and transforms
Browsers break `position: fixed;` when an element's parent has a `transform` set. Using transforms creates a new containing block, effectively forcing the parent to have `position: relative;` and the fixed element to behave as `position: absolute;`.

[Bekijk de demo](http://jsbin.com/yabek/1/) en lees [Eric Meyer's post on the matter](http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/).
