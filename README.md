# KD Lab pagedjs workshop

## What is pagedjs / Was ist pagedjs

### CSS Extension / CSS Erweiterung

Pagedjs is facilitation the [CSS Generated Content for Paged Media Module Draft](https://www.w3.org/TR/css-gcpm-3/), which hasn't been fully implemented by any browser to date.
This allows us to use CSS rules for printed media, which the browser would otherwise ignore. Without these designing print documents is quite constrained.

Pagedjs setzt unter anderem den [CSS Generated Content for Paged Media Module Draft](https://www.w3.org/TR/css-gcpm-3/) um, der bisher noch von keinem Browser (komplett) unterstützt wird.
Dadurch können wir hilfreiche CSS Regeln benutzen, die der Browser normalerweise ignorieren würde. Ohne diese Regeln ist die Gestaltung von Print Dokumenten noch sehr begrenzt.

### Print Preview / Druckvorschau

To implement those print css rules pagedjs is simulating a print preview, which chunks our website into multiple pages and applies our custom styles.
This also allows us to use the inspect dev tools, to debug our code and try out code directly in the browser.  

Um diese und weitere Regeln umzusetzen simuliert pagedjs eine Druckvorschau, bei der unsere Webseite
gechunkt wird und in Doppelseiten ausgeworfen wird. Dazu können wir auch DevTools benutzen
um direkt im Browser Änderungen durchzuführen.

## setup

We will use the pagedjs polyfill. You can either download the script locally or use a cdn link. In these folders we use it locally.

Wir benutzen heute den vorkonfigurierten pagedjs polyfill. Wir können pagedjs entweder lokal oder über einen cdn link. Hier benutzen wir pagedjs in einer lokalen Datei.
[paged documentation](https://pagedjs.org/documentation/2-getting-started-with-paged.js/#using-paged.js-as-a-polyfill-in-web-browsers)

```HTML
<script src="./paged.polyfill.js" defer></script>
```

The polyfill automatically renders the HTML into a print preview. This can be stopped by adding `PagedConfig` as object with `{ auto: false }` to the [global window object](https://developer.mozilla.org/en-US/docs/Glossary/Global_object).

Der polyfill zeigt das HTML automatisch, nachdem er geladen wurde, in der Druckvorschau. Dies kann verhindert werden wenn wir `PagedConfig` mit `{ auto: false }` zum [global window object](https://developer.mozilla.org/en-US/docs/Glossary/Global_object) hinzufügen.

```HTML
<script> window.PagedConfig = { auto: false };</script>
```

To render the HTML in a print preview we can add a button:

Um das HTML als Printvorschau zu rendern fügen wir ein Button zu unserem HTML hinzu:

```HTML
<button onclick="window.PagedPolyfill.preview()" class="no-print"> preview</button>
```

For the preview to function correctly, we need to add the `interface.css` from the [official gitlab](https://gitlab.coko.foundation/pagedjs/interface-polyfill).

Wir brauchen auch die `interface.css` vom [offiziellen gitlab](https://gitlab.coko.foundation/pagedjs/interface-polyfill) um die Druckbögen korrekt zu sehen

```HTML
<link rel="stylesheet" href="./interface.css" />
```

Now the HTML should look like this.

Jetzt müsste dein HTML etwa so aussehen.

```HTML
<!DOCTYPE html>
<html lang="en">
    <head>
        <script>
            window.PagedConfig = { auto: false };
        </script>
        <script src="./paged.polyfill.js" defer></script>
        <link rel="stylesheet" href="./interface.css" />
    </head>
    <body>
        <button onclick="window.PagedPolyfill.preview()" class="no-print"> preview</button>
    </body>
</html>
```

## @page rules

@page css selector can be used to configure our document. Similarly to document options and master pages in InDesign.

Mit dem @page-CCS-Selektor können wir unser Dokument grundlegend einstellen. Diese funktioniert in etwa
wie die Dokumenteinstellungen und Masterseiten in InDesign.

### size

The size rule defines the format of our document. We can either use DIN formats or use custom sizes in mm for example. For print there exists mm and pt as unit in CSS.
_The size rule can only be defined once in an @page selector!_

In der size-Regel wird das Papierformat festgelegt. Dabei können wir gängige Formate oder mm Angaben nutzen. Für Print können wir im CSS mm und pt als Einheiten benutzen.
_Die size rule kann nur einmal im @page selektor festgelegt werden!_

```CSS
@page {
    size: 200mm 200mm;
    size: A4 landscape; /*landscape if we don't want the default portrait mode -- landscape falls wir nicht portrait wollen*/
}
```

### margin

The margin rule is used to define the margins of the page in the document. These margins can later be filled with [generated content](https://pagedjs.org/en/documentation/7-generated-content-in-margin-boxes/) via pagedjs.

Mit margin geben wir unseren Satzspiegel an. Die margins werden von Pagedjs später in unterschiedliche Elemente aufgeteilt, die mit [Inhalt gefüllt werden können.](https://pagedjs.org/en/documentation/7-generated-content-in-margin-boxes/)

```CSS
@page {
  margin-top: 20mm;
  margin-bottom: 25mm;
  margin-left: 10mm;
  margin-right: 35mm;
  /* oder kurz: */
  /* margin: 20mm 35mm 25mm 10mm;*/
  @top-center{
    content: 'my Book';
  }
  @bottom-left-corner{
    content: counter(page);
  }
}

```

### bleed

Bleed can also be defined with the bleed attribute.

Bleed bezeichnet den Anschnitt. Dort benutzen wir in den meisten Fällen 3mm

```CSS
@page{
  bleed: 3mm;
}
```

## Named Pages / Spezifische Seiten ansprechen

To style specific pages we can use pseudo selectors or named pages.

_Here we can use all @page rules except for size and bleed. These have to be defined in the general @page query!_

Um Seiten spezifisch zu gestalten können wir entweder Pseudoselektoren oder benannten Seiten benutzen.

_Hier können alle @page regeln verwendet werden, außer size und bleed. Diese können nur einmal im
generellen @page query festgelegt werden!_

### Page Pseudo Selectors / Page Pseudo Selektoren

There exist the following pseudo selectors:

Um spezifische Seiten zu stylen gibt es folgende CSS page Selektoren:

- `:first` (for the first page / für die erste Seite)
- `:blank` (all pages without content / für alle Seiten ohne Inhalt)
- `:right` (all odd or right pages / alle ungeraden Seiten; alle rechten Seiten)
- `:left` (all even or left pages / alle geraden Seiten; alle linken Seiten)

[Dokumentation](https://pagedjs.org/documentation/5-web-design-for-print/#%40page-rule)
[Cheat Sheet](https://pagedjs.org/documentation/cheatsheet/)

This is how we use them:

Diese werden folgendermaßen eingesetzt:

```CSS
@page:left {
    /*...*/
}
@page:right {
    /*...*/
}
@page:blank {
    /*...*/
}
@page:first {
    /*...*/
}
```

This allows us for example to define a different page layout for left and right pages, to have a constant inner and outer margin.

So können wir zum Beispiel den Satz spiegel für links und rechts seperat einstellen, damit der Abstand
zum Bund und zur Außenkante rechts und links gleich ist.

```CSS
@page:left {
  margin: 5mm 10mm 5mm 5mm;
}

@page:right {
  margin: 5mm 5mm 5mm 10mm;
}
```

### named pages

Named pages allow us to e.g. style different chapters specifically. For this we define a name in the container/wrapper of the chapter:

Mit named pages können wir z.B. verschiedene Kapitel in unserem Dokument anders stylen. Dafür geben
wir im Container/Wrapper des Kapitels an, wie diese heißen soll:

```CSS
div.chapter-introduction {
    page: Introduction;
}
```
Every page which contains our container/wrapper can be styled now:

Jede Seite auf der unser Container/Wrapper enthalten ist, kann nun seperat gestyled werden:

```CSS
@page Introduction {
    background: red;
}
```

Of course we can also use the pseudo selectors in combination:

Auch hier können wir mit den page Pseudo Selektoren arbeiten:

```CSS
@page Introduction:first {
    margin: 0;
    @bottom-left {
        content: "Introduction";
    }
}
```

## desinging for print / Für print gestalten

### @media print

Media queries [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries) are used to design websites for different devices. With `@media print`, we can design specifically for printing, and with `@media screen` for the screen. Pagedjs recognizes `@media print` in CSS and implements the rules.

Um websites für verschiedene Geräte zu gestalten werden [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries) verwendet. Mit `@media print` können wir spezifisch für den Druck gestalten und mit `@media screen` für den Bildschirm.
Pagedjs erkennt `@media print` im CSS und setzt die Regeln um.

_Properties in `@media print` are only applied when printing. Properties in `@media screen` are only applied on the web._

_Eigenschaften in `@media print` werden nur beim Druck angewendet. Eigenschaften in `@media screen` nur im Web._

```CSS
@media print {
    .no-print {
        display: none;
    }
}
```

Here, for example, we have a class that is not visible in print.

Hier haben wir zum Beispiel eine Klasse die im Druck nicht sichtbar ist.

### break-before, break-after, break-inside

There are three CSS properties that can be used to influence page flow/column flow

Um den Seitenumfluss/Spaltenumfluss zu beeinflussen gibt es die drei CSS Eigenschaften

- [break-after](https://developer.mozilla.org/en-US/docs/Web/CSS/break-after?retiredLocale=de)
- [break-before](https://developer.mozilla.org/en-US/docs/Web/CSS/break-before)
- [break-inside](https://developer.mozilla.org/en-US/docs/Web/CSS/break-inside)

#### break-before, break-after

As the names suggest, break-before influences the behavior of the page/column flow before the element,
and break-after influences the behavior after the element.
The following values can be specified here:

- `always` (the next page/column is broken before/after)
- `page` (the next page is broken before/after)
- `column` (the next column is broken before/after)
- `left` (the left page is broken before/after)
- `right` (a break is made before/after it on the right-hand page)
- `avoid` (an attempt is made to avoid a page/column break before/after it, unless there is no other option)
- `avoid-page` (only for page breaks)
- `avoid-column` (only for column breaks)

Wie die Namen schon sagen beeinflusst break-before das Verhalten des Seiten-/Spaltenumflusses vor dem Element
und break-after den nach dem Element.
Hier können folgende Werte angegeben werde:

- `always` (es wird davor/danach auf die nächste Seite/Spalte umgebrochen)
- `page` (es wird davor/danach auf die nächste Seite umgebrochen)
- `column` (es wird davor/danach in die nächste Spalte umgebrochen)
- `left` (es wird davor/danach auf die linke Seite umgebrochen)
- `right` (es wird davor/danach auf die rechte Seite umgebrochen)
- `avoid` (es wird versucht einen Seiten-/Spaltenumbruch davor/danach zu umgehen, außer es geht nicht anders)
- `avoid-page` (nur für Seitenumbrüche)
- `avoid-column` (nur für Spaltenumbrüche)

#### break-inside

With break-inside, we can try to prevent elements from being split and wrapped across a
page/column.

Mit break-inside können wir versuchen zu verhindern, dass Elemente aufgespalten werden und über eine
Seite/Spalte umgebrochen werde.

```CSS
.non-breaking-block {
    break-inside: avoid;
}
```

#### Example / Beispiel

With elements such as lead pages that are always on their own, we can specify that the page layout should always be broken before and/or after them.

Wenn wir Elemente wie Aufmacher-Seiten, die immer auf einer Seite alleine sind, haben, dann können wir festlegen das vor und/oder nach ihnen immer das Seitenlayout umgebrochen wird.

```CSS
.hero {
    break-before: page;
    break-after: page;
}
```

Sollen Aufmacher Seiten immer links stehen:

```CSS
.hero {
    break-before: left;
    break-after: page;
}
```

### using paged.js classes / pagedjs-Klassen nutzen

Pagedjs assigns several classes to the page layout. We can use these to style elements that appear on specific pages.
The best way to view these is with the Inspect Tool in your browser.

Pagedjs gibt dem page layout einige Klassen. Diese können wir benutzen um Elemente die sich auf bestimmten Seiten befinden zu stylen.
Diese könnt ihr am besten mit dem Inspect Tool im Browser ansehen.

To design specific elements on the left or right, we need to use the page classes created by pagedjs.

Um Elemente links oder rechts spezifisch zu gestalten, müssen wir die page-Klassen nutzen, die pagedjs erstellt.

```CSS
.pagedjs_left_page p{
    color: blue;
}
.pagedjs_right_page p{
    color: red;
}
```

## Generated Content

### Content property

The CSS content property can be used to specify what the content of an element should be.
This can be text, images, color gradients, HTML attributes, etc. [Here](https://developer.mozilla.org/en-US/docs/Web/CSS/content#syntax)
is a brief overview.

Die CSS content Eigenschaft kann benutzt werden um festzulegen was der Inhalt eines Elements sein soll.
Das können Text, Bilder, Farbverläufe, HTML attribute, etc. sein. [Hier](https://developer.mozilla.org/en-US/docs/Web/CSS/content#syntax)
ist eine kleine Übersicht.

pagedjs allows us to fill the margin boxes we saw at the beginning with the `content` property:

pagedjs erlaubt es uns die Margin Boxen, welche wir am Anfang gesehen haben mit der `content` Eigenschaft zu befüllen:

```CSS
@page:left {
    @bottom-center {
        content: "Mein Kolumnentitel"
    }
}
```

Next, we will look at some interesting content that can be used as content.

Nachfolgen schauen wir uns ein paar interessante Inhalte an, die als content eingesetzt werden können.

### Counter

You may be familiar with CSS counters (https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Counter_Styles/Using_CSS_counters) from OL elements.
In theory, counters are just a variable (number) that can be used as `content`.
They work with two CSS commands:

[CSS Counter](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Counter_Styles/Using_CSS_counters) kennt ihr vielleicht von OL Elementen.
Theoretisch sind counter nur eine Variable (Zahl) die als `content` benutzt werden kann.
Diese funktionieren mit zwei CSS Befehlen:

#### Counter Increment

We specify which elements should increase the counter. To do this, we enter the name of the counter.
Footnotes are a good example.

Wir legen fest welche Elemente den Counter erhöhen sollen. Dafür geben wir den Namen des Counters an.
Als Beispiel bieten sich Fußnoten an.

```CSS
.footnote {
    counter-increment: footnote;
}
```

#### Counter Reset

With `counter-reset`, we specify which element resets the counter. In our example,
it makes sense to reset the footnote counter with each new article heading so that we start again
at 1.

Mit dem `counter-reset` legen wir fest, welches Element den Counter zurücksetzt. In unserem Beispiel
macht es Sinn den Fußnoten Counter bei jeder neuen Artikel überschrift zurückzusetzen, damit wir wieder
bei 1 starten.

```CSS
h1 {
    counter-reset: footnote;
    /* es ist auch möglich diesen auf eine spezifische Zahl zurückzusetzen */
    counter-reset: footnote 4;
}
```

#### Display / Darstellung

To display the counter, we just need to display it as `content` (here, for example, before the footnote):

Um den Counter anzuzeigen müssen wir ihn nur als `content` darstellen (hier z.B. vor der Fußnote):

```CSS
.footnote::before {
    content: counter(footnote);
}
```

### Paged Media Counters

When printing a web page, the browser automatically creates the following counters:

- `page` (the current page number)
- `pages` (the total number of pages)

This allows us to insert page numbers into our margin boxes:


Der Browser erstellt automatisch beim Druck einer Webseite folgende Counter:

- `page` (die aktuelle Seitenzahl)
- `pages` (die Gesamtanzahl der Seiten)

Damit können wir Seitenzahlen in unsere Margin Boxen einfügen:

```CSS
@page:left {
    @bottom-left-corner {
        content: counter(page) /* Hier würde das dann auf Seite 2 so aussehen: 2 */
        content: counter(page) "/" counter(pages) /* Seite 2 (bei insgesamt 22 Seiten): 2/22 */
        /* Man kann der content Eigenschaft auch mehrere Inhalte geben und diese einfach
         nur durch ein Leerzeichen trennen, siehe oben. Diese werden dann hintereinander
         dargestellt.*/
    }
}
```

### running head / Kolumnentitel

Finally, we will look at how we can get article titles into our margin boxes, for example.
To do this, we need the `string-set` property. This works like a variable that can store text.
As with the counter, we specify which element influences it. For example, we can store the text content of a headline
as a string-set:


Als letztes schauen wir uns noch an wie wir z.B. Titel von Artikeln in unsere Margin Boxen bekommen.
Dafür brauchen wir die `string-set` Eigenschaft. Diese funktioniert wie eine Variable, die Text speichern kann.
Wie beim Counter geben wir an welches Element Einfluss auf diese hat. Wir können zum Beispiel den Textinhalt einer Überschrift
als string-set speichern:

```CSS
h1 {
    string-set: articleTitle content(text);
    /* Wir geben erst den Namen unserer Variable an (hier: articleTitle) und dann den Inhalt */
    /* Die variablen müssen nicht initiiert werden, sobald der Name auftaucht existiert diese als solche */
}
```
Then display the title in our margin boxes:

Und dann den Titel in unseren Margin Boxen darstellen:

```CSS
@page {
    @bottom-center {
        content: string(articleTitle);
    }
}
```

If several h1 elements appear on our page, we can specify which title we want to use:

Sollten auf unserer Seite mehrere h1 Elemente auftauchen können wir angeben welchen der Titel wir benutzen wollen:

- `first`
- `last`
- `start`

For example, we always want to use the first title on the page:

Wir wollen zum Beispiel immer den ersten Titel auf der Seite benutzen:

```CSS
@page {
    @bottom-center {
        content: string(articleTitle, first);
    }
}
```

### running elements

Another quite interesting option is to use running elements to place in our margin boxes. We give the element a `position: running(variableName);` property and pagedjs removes this element from the pageflow and places it where we define `content: element(variableName);`

Eine andere interessante option sind running elements die wir in unsere Marginalränder einbetten können. Dafür benutzen wir die property `position: running(variableName);` die pagedjs dazu auffordert das Element aus dem pageflow zu nehmen und dort einzusetzen wo definiert `content: element(variableName);` ist.

example / Beispiel
```CSS
img.running-element {
    position: running(cornerSymbol);
}

@page {
    /* ... */
    @top-left-corner {
        content: element(cornerSymbol);
        /* ... */
    }
}
```

There are several other options here, and the [pagedjs documentation] (https://pagedjs.org/documentation/7-generated-content-in-margin-boxes/) can also help.

Hier gibt es noch einige andere Möglichkeiten, da hilft auch die [pagedjs Doku.](https://pagedjs.org/documentation/7-generated-content-in-margin-boxes/)

### Exkurs: TOC

To create a table of contents with the correct page numbers, we create our table of contents as links.
i.e.:

Um ein Inhaltsverzeichnis mit den richtigen Seitennummern zu erstellen, legen wir unser Inhaltsverzeichnis als Links an.
z.B.:

```HTML
      <ul>
        <li>
          <a href="#headline1">headline1</a>
        </li>
      </ul>
```
In our text, the headline is then assigned the ID:

In userem Text wird die Headline dann mit der Id versehen:

```HTML
    <!-- ... -->
    <h1 id="headline1">
        Mein Titel
    </h1>
    <!-- ... -->
```
We can do this manually or [with JavaScript](https://pagedjs.org/posts/build-a-table-of-contents-from-your-html/).
We then insert the page number using CSS with a pseudo-element.

Dies können wir manuell oder [mit JavaScript](https://pagedjs.org/posts/build-a-table-of-contents-from-your-html/) machen.
Die Seitenzahl fügen wir dann über CSS mit einem pseudo-element ein.

```CSS
 li a::after{
    content: target-counter(attr(href), page);
}
```
CSS now fills the element with a counter for the page that is the target of our link. 
We can also use [CSS counters](https://pagedjs.org/documentation/6-generated-content/#generated-counters) for page and line numbers or similar numbering.

CSS füllt jetzt das Element mit einem Counter der Seite die das Ziel unseres Links hat.
[CSS Counter](https://pagedjs.org/documentation/6-generated-content/#generated-counters) können wir auch für Seiten- und Zeilenzahlen oder ähnliche Nummerierungen nutzen.

## Helpful CSS rules / Hilfreiche CSS-Regeln

### columns

We can use the [CSS columns rule](https://developer.mozilla.org/en-US/docs/Web/CSS/columns) if we want to run text in multi-column layouts.
The advantage of columns over a CSS grid is that reflow works better in print.

Wir können die [CSS-Columns-Regel](https://developer.mozilla.org/en-US/docs/Web/CSS/columns) nutzen, wenn wir Text in mehrspaltigen Layouts laufen lassen wollen.
Der Vorteil von columns gegenüber einem css grid ist der Umfluss der im Print besser funktioniert.

```CSS
.multi-column{
    columns: 2 100mm; //shorthand
    column-count: 2;
    column-width: 100mm
}
```
To break columns, we can use `break-after` and `break-before` with `column`.

Um spalten umzubrechen können wir `break-after` und `break-before` mit `column` nutzen.

### widows, orphans (CHROME OLNLY / NUR CHROME)

`widows` determine the minimum number of lines of text that should appear at the beginning of a page/column.

`orphans` determine the minimum number of lines of text that should appear before a page/column break.

`widows` legen fest wie viele Zeilen Text am Anfang einer Seite/Spalte mindestens stehen sollen.

`orphans` legen fest wie viele Zeilen Text vor einem Seiten-/Spaltenumbruch mindestens stehen sollen.

```CSS
p {
    widows: 2;
    orphans: 3;
}
```

### floated Elements / floated Elemente

For elements that should be wrapped by text, there is the CSS property `float`.
[MDN link](https://developer.mozilla.org/en-US/docs/Web/CSS/float)

Für Elemente die vom Text umfloßen werden sollen gibt es die CSS Eigenschaft `float`.
[MDN link](https://developer.mozilla.org/en-US/docs/Web/CSS/float)

## pagedjs variables / pagedjs Variablen

Pagedjs also provides these variables for calculation (values are only examples and come from the @page rule)

Pagedjs stellt zur Berechnung auch diese Variablen zur Verfügung (Werte sind nur Beispiel und kommen aus der @page regel)

```CSS
/* Page size / Seitengröße */
  --pagedjs-pagebox-width: 148mm;
  --pagedjs-pagebox-height: 210mm;
/* Page size with bleed / Seitengröße mit Anschnitt */
  --pagedjs-width: calc( 148mm + 3mm + 3mm );
  --pagedjs-height: calc( 210mm + 3mm + 3mm );
/* Margins / Ränder */
  --pagedjs-margin-top: 31.5mm;
  --pagedjs-margin-right: 22mm;
  --pagedjs-margin-left: 9mm;
  --pagedjs-margin-bottom: 7mm;
/* Bleed / Anschnitt */
  --pagedjs-bleed-top: 3mm;
  --pagedjs-bleed-right: 3mm;
  --pagedjs-bleed-bottom: 3mm;
  --pagedjs-bleed-left: 3mm;
```

Diese könnt ihr anwenden um zum Beispiel Dinge in den Anschnitt zu ziehen:

```CSS
.pagedjs_left_page img {
    margin-left: calc(-1 * (var(--pagedjs-bleed-left) + var(--pagedjs-margin-left)))
}
```
Here, we give images on the left side a negative margin that is as large as the left margin + the bleed.

Hier geben wir Bildern auf einer linken Seite ein negative Margin, die so groß ist wie die linke Margin + den Anschnitt.

## Javascript Hooks

For even more control, [pagedjs hooks](https://pagedjs.org/documentation/10-handlers-hooks-and-custom-javascript/) are available, which can be attached at certain points during the
generation of the print preview. (For advanced users)

Für noch mehr Kontrolle stellt [pagedjs hooks](https://pagedjs.org/documentation/10-handlers-hooks-and-custom-javascript/) zur Verfügung, an die man sich an gewissen Momenten der
Generierung der Druckvorschau hängen kann. (Für Fortgeschrittene)

## Plugins

- [Imposition](https://gitlab.coko.foundation/pagedjs/pagedjs-plugins/booklet-imposition) (for imposing brochures / zum Ausschießen von Broschüren)
- [full-page](https://gitlab.coko.foundation/pagedjs/pagedjs-plugins/full-page) (to display images or other elements across the entire surface of a page; but this can also be done simply with pure CSS
 / um Bilder oder andere Elemente vollflächig auf einer Seite darzustellen; geht aber auch einfach mit purem CSS)
- [table of content](https://gitlab.coko.foundation/pagedjs/pagedjs-plugins/table-of-content) (to create a table of contents / um ein Inhaltsverzeichnis zu erstellen)
