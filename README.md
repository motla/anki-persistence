# anki-persistence
Persist data between both sides of an anki flashcard. While the following example explicitly shows the random number, it could instead be used internally to do many different things:

* choose one of several pictures (at random) *- Maybe someone wants to learn the names of instruments from a picture depicting it; One of multiple views of the instrument could be shown, a different one in every review.*
* reorder elements in the card *- Shuffle the answers of a multiple choice question.*
* ...

It could also be used completely different as well. To let the user input something on the frontside and use that same input on the backside for example.

## Example: Random number

This example shows how anki-persistence can be used to display the same random number on both sides of an Anki flash card. **All of these images depict the same note!** You can try it out yourself with [this exported Anki deck](examples/random-number/anki-persistence.apkg).

### Result
| Client                 | Front | Back |
| ----------------------:|:-----:|:----:|
|                    Web | ![Random number example on the web client - Front](examples/random-number/Web-Front.jpg) | ![Random number example on the web client - Back](examples/random-number/Web-Back.jpg) |
|                Android | ![Random number example on the Android client - Front](examples/random-number/AnkiDroid-Front.jpg) | ![Random number example on the Android client - Back](examples/random-number/AnkiDroid-Back.jpg) |
| Android (card preview) | ![Random number example on the Android client (card preview) - Front](examples/random-number/AnkiDroid_Preview-Front.jpg) | ![Random number example on the Android client (card preview) - Back](examples/random-number/AnkiDroid_Preview-Back.jpg) |
|                    iOS | ![Random number example on the iOS client - Front](examples/random-number/iOS-Front.jpg) | ![Random number example on the iOS client - Back](examples/random-number/iOS-Back.jpg) |
|     iOS (card preview) | ![Random number example on the iOS client (card preview) - Front](examples/random-number/iOS_Preview-Front.jpg) | ![Random number example on the iOS client (card preview) - Back](examples/random-number/iOS_Preview-Back.jpg) |
|                    Mac | ![Random number example on the Mac client - Front](examples/random-number/Mac-Front.png) | ![Random number example on the Mac client - Back](examples/random-number/Mac-Back.png) |
|     Mac (card preview) | ![Random number example on the Mac client (card preview) - Front](examples/random-number/Mac_Preview-Front.png) | ![Random number example on the Mac client (card preview) - Back](examples/random-number/Mac_Preview-Back.png) |
|                Windows | ![Random number example on the Windows client - Front](examples/random-number/Windows-Front.jpg) | ![Random number example on the Windows client - Back](examples/random-number/Windows-Back.jpg) |
| Windows (card preview) | ![Random number example on the Windows client (card preview) - Front](examples/random-number/Windows_Preview-Front.jpg) | ![Random number example on the Windows client (card preview) - Back](examples/random-number/Windows_Preview-Back.jpg) |
|                  Linux | ![Random number example on the Linux client - Front](examples/random-number/Linux-Front.png) | ![Random number example on the Linux client - Back](examples/random-number/Linux-Back.png) |
|     Linux (card preview) | ![Random number example on the Linux client (card preview) - Front](examples/random-number/Linux_Preview-Front.png) | ![Random number example on the Linux client (card preview) - Back](examples/random-number/Linux_Preview-Back.png) |

**Note that Persistence is not available for Windows card preview (```Persistence.isAvailable()``` returns ```false```), thus a default (0.4) is chosen.**

### Setup
#### Front side
~~~html
<script>
// v0.2.1 - https://github.com/SimonLammer/anki-persistence/blob/8a3648baa02a9cd5c1f51e5adf3772dd5d494757/script.js
"undefined"===typeof window.Persistence&&(window.Persistence=new function(){var a=!1;try{"object"===typeof window.sessionStorage&&(a=!0,this.store=function(a){sessionStorage.setItem("github.com/SimonLammer/anki-persistence",JSON.stringify(a))},this.load=function(){return JSON.parse(sessionStorage.getItem("github.com/SimonLammer/anki-persistence"))})}catch(e){}for(var d=["py","qt"],b=0;!a&&b<d.length;b++){var c=window[d[b]];"object"===typeof c&&(a=!0,this.store=function(a){c["github.com/SimonLammer/anki-persistence"]=
a},this.load=function(){return c["github.com/SimonLammer/anki-persistence"]||null})}this.isAvailable=function(){return a}});
</script>

{{Front}}

<div id="front"></div>

<script>
var number = 0.4;                // Default to 0.4.
if (Persistence.isAvailable()) { // Check whether Persistence works on the client.
  number = Persistence.load();   // Load a previously stored number. (In case {{FrontSide}} is used)
  if (number == null) {          // If there was a previously stored number:
    number = Math.random();      //   1. Create a random number.
    Persistence.store(number);   //   2. Store that number
  }
}
window.front.appendChild(document.createTextNode(number)); // Print the number.
</script>
~~~

#### Back side

~~~html
<script>
// v0.2.1 - https://github.com/SimonLammer/anki-persistence/blob/8a3648baa02a9cd5c1f51e5adf3772dd5d494757/script.js
"undefined"===typeof window.Persistence&&(window.Persistence=new function(){var a=!1;try{"object"===typeof window.sessionStorage&&(a=!0,this.store=function(a){sessionStorage.setItem("github.com/SimonLammer/anki-persistence",JSON.stringify(a))},this.load=function(){return JSON.parse(sessionStorage.getItem("github.com/SimonLammer/anki-persistence"))})}catch(e){}for(var d=["py","qt"],b=0;!a&&b<d.length;b++){var c=window[d[b]];"object"===typeof c&&(a=!0,this.store=function(a){c["github.com/SimonLammer/anki-persistence"]=
a},this.load=function(){return c["github.com/SimonLammer/anki-persistence"]||null})}this.isAvailable=function(){return a}});
</script>

{{Back}}

<div id="back"></dv>

<script>
var number = 0.4;                // Default to 0.4.
if (Persistence.isAvailable()) { // Check whether Persistence works on the client.
  number = Persistence.load();   // Load the previously stored number
  Persistence.store(null);       // Clear the storage, so a new random number will be created on the next card.
}
window.back.appendChild(document.createTextNode(number)); // Print the number.
</script>
~~~

#### Note

The note has two fields: ```Front``` and ```Back```.
These are set to ```front``` and ```back``` respectively.

## Preparation

To use anki-persistense, follow these steps:
1. Download the script from [the latest release](releases/latest). We recommend using the minified version (```minified.js```) to save space in the anki card type.
1. Copy and paste the file contents between to the beginning of front and back side of the card type.
1. Ensure that both script blocks are enclosed in ```<script>``` and ```</script>```.

*This can be seen in [the example setup](#setup).*

## Usage

Anki clients vary in their implementation and JavaScript can behave differently in each one. Therefore, check for availability before using other Persistence methods.

~~~javascript
if (Persistence.isAvailable()) {
	// do stuff
}
~~~

Other methods:

|                       Name    | Description |
| -----------------------------:|:----------- |
| ```Persistence.store(data)``` | Persists the data, so it can be retrieved later. |
|      ```Persistence.load()``` | Retrieves previously stored data. If no data has been stored yet, null is returned. |

*Some implementations of Persistence may use JSON.stringify and JSON.parse in the process of persisting and retrieving data.*

### Clear storage

```Persistence.store``` may persist data across cards, this should be stopped by calling ```Persistence.store(null)``` at the end of the backside. (If this gets called on the frontside's beginning instead, you cannot use anki's ```{{FrontSide}}``` special field in the backside *- because this would delete the persisted data*)

# Acknowledgements

Huge thanks to

* [RunasSudo, whose code kick-started this project](https://yingtongli.me/blog/2015/03/15/random-question-generator-on-anki-using.html)!
* [u/CheCheDaWaff for providing test information and screenshots of the woking random number example on **mac** and **iOS** clients](https://www.reddit.com/r/Anki/comments/8ksjqb/pass_data_between_both_sides_of_an_anki_flashcard/dzbpfdd/)
* [u/qwiglydee for providing test information and screenshots of the working random number example on the **linux** client](https://www.reddit.com/r/Anki/comments/8ksjqb/pass_data_between_both_sides_of_an_anki_flashcard/dzbpnbm/)


## Other references

* [r/Anki - Passings state between Fields](https://www.reddit.com/r/Anki/comments/4mhfmm/passing_state_between_fields/)
* [r/Anki - Pass data between both sides of an Anki flashcard](https://www.reddit.com/r/Anki/comments/8ksjqb/pass_data_between_both_sides_of_an_anki_flashcard/)