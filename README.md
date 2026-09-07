Generate a bingo board prior to Zootopia 3! Refresh the page to get a new board.

Based on Alex Irpan's [Mystery Hunt Bingo](https://www.alexirpan.com/mystery-hunt-bingo/).

<table border="1" cellpadding="0" cellspacing="0">
    <tr>
        <td width="125" height="125" id="00"></td>
        <td width="125" height="125" id="01"></td>
        <td width="125" height="125" id="02"></td>
        <td width="125" height="125" id="03"></td>
        <td width="125" height="125" id="04"></td>
    </tr>
    <tr>
        <td width="125" height="125" id="10"></td>
        <td width="125" height="125" id="11"></td>
        <td width="125" height="125" id="12"></td>
        <td width="125" height="125" id="13"></td>
        <td width="125" height="125" id="14"></td>
    </tr>
    <tr>
        <td width="125" height="125" id="20"></td>
        <td width="125" height="125" id="21"></td>
        <td width="125" height="125" id="22">Waiting for Javascript to populate entries. Please wait...</td>
        <td width="125" height="125" id="23"></td>
        <td width="125" height="125" id="24"></td>
    </tr>
    <tr>
        <td width="125" height="125" id="30"></td>
        <td width="125" height="125" id="31"></td>
        <td width="125" height="125" id="32"></td>
        <td width="125" height="125" id="33"></td>
        <td width="125" height="125" id="34"></td>
    </tr>
    <tr>
        <td width="125" height="125" id="40"></td>
        <td width="125" height="125" id="41"></td>
        <td width="125" height="125" id="42"></td>
        <td width="125" height="125" id="43"></td>
        <td width="125" height="125" id="44"></td>
    </tr>
</table>

<div style="text-align:center">
    <input id="seed" type="text">
    <button id="generate">Generate</button>
</div>

<script>
var PHRASE_LIST = [
    "Bellwether returns",
    "Pawbert returns",
    "Twist villain",
    "Twist hero",
    "Nick's family",
    "Judy and Nick's real wedding",
    "Judy and Nick's fake wedding",
    "Bonnie and Stu matter to plot",
    "a Hopps sibling named & speaks",
    "Brennan Lee Mulligan cameo",
    "Nocturnal District",
    "Eagles (band) reference",
    "Eagles (football team) reference",
    "Long timeskip after Z2",
    "Post Credits Scene",
    "They go where birds are from",
    "Fakeout death",
    "Gazelle",
    "Reference to other WDAS movies",
    "Bogo retires",
    "New mayor",
    "Teases another sequel",
    "Dance party end credits",
    "Judy leaves the force",
    "Nick leaves the force",
    "Gary works for the police",
    "Doctor Fuzzby",
    "Three Wheeled Jokemobile",
    "It's called a something, sweetheart",
    "The Hustle by Van McCoy",
    "Whoopsie, double whoopsie",
    "Junior Ranger Scouts",
    "Mr. Big helps",
    "Nick's apartment",
    "Judy's apartment",
    "Bucky & Pronk on-screen",
    "Villain knows about the carrot pen",
];

// From https://github.com/bryc/code/blob/master/jshash/experimental/cyrb53.js
// Generate 53-bit hash
// Should generate enough randomness / be impossible to rig even with source code.
const cyrb53 = (str, seed = 0) => {
  let h1 = 0xdeadbeef ^ seed,
    h2 = 0x41c6ce57 ^ seed;
  for (let i = 0, ch; i < str.length; i++) {
    ch = str.charCodeAt(i);
    h1 = Math.imul(h1 ^ ch, 2654435761);
    h2 = Math.imul(h2 ^ ch, 1597334677);
  }

  h1 = Math.imul(h1 ^ (h1 >>> 16), 2246822507) ^ Math.imul(h2 ^ (h2 >>> 13), 3266489909);
  h2 = Math.imul(h2 ^ (h2 >>> 16), 2246822507) ^ Math.imul(h1 ^ (h1 >>> 13), 3266489909);

  return 4294967296 * (2097151 & h2) + (h1 >>> 0);
};

// From https://github.com/bryc/code/blob/master/jshash/PRNGs.md#mulberry32
// Seedable PRNG.
function mulberry32(a) {
    return function() {
      a |= 0; a = a + 0x6D2B79F5 | 0;
      var t = Math.imul(a ^ a >>> 15, 1 | a);
      t = t + Math.imul(t ^ t >>> 7, 61 | t) ^ t;
      return ((t ^ t >>> 14) >>> 0) / 4294967296;
    }
}


function cleanSeed(seed) {
    var cleaned = seed.replace(/[^0-9a-zA-Z]/g, '');
    cleaned = cleaned.toUpperCase();
    return cleaned;
}

function shuffle(array, prng) {
    var currentIndex = array.length
      , temporaryValue
      , randomIndex
      ;

    // While there remain elements to shuffle...
    while (0 !== currentIndex) {

      // Pick a remaining element...
      randomIndex = Math.floor(prng() * currentIndex);
      currentIndex -= 1;

      // And swap it with the current element.
      temporaryValue = array[currentIndex];
      array[currentIndex] = array[randomIndex];
      array[randomIndex] = temporaryValue;
    }

    return array;
}

function randomSeed() {
    const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
    let lst = [];
    for (var i = 0; i < 9; i++) {
        lst[i] = chars.charAt(Math.floor(Math.random() * chars.length));
    }
    return lst.join('');
}

// Write a random seed value.
// This is needed to make it work properly on refresh - the browser seems to cache
// the input value which makes it pass the check in generate()
var seedElem = document.getElementById('seed');
seedElem.value = randomSeed();

function generate() {
    if (!seedElem.value) {
        // Generate for them
        console.log(seedElem.value);
        seedElem.value = randomSeed();
    }
    var cleaned = cleanSeed(seedElem.value);
    var prng = mulberry32(cyrb53(cleanSeed(seedElem.value)));

    var phraseList;

    // Shuffle then take first 24 entries.
    phraseList = [...PHRASE_LIST];
    phraseList = shuffle(phraseList, prng);

    var count = 0;
    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            // Assign entries
            var id = i.toString() + j.toString();
            var element = document.getElementById(id);
            if (i === 2 && j === 2) {
                element.innerHTML = "FREE SQUARE: Birds!";
                element.style.fontWeight = "bold";
            } else {
                element.innerHTML = phraseList[count++];
            }
            // Misc styling
            element.style.textAlign = "center";
            element.style.verticalAlign = "middle";
        }
    }
}

// connect to button and generate intial page
document.getElementById('generate').onclick = function() { generate(); }
generate();
</script>