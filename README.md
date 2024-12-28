# EdoBox

EdoBox is an online tool for sketching and sharing instrumental music.
It is a modification of [JummBox](https://jummbus.bitbucket.io), a modification of the [original BeepBox](https://beepbox.co). Most of the work was done by them, and I do not claim to have made this program by myself.

All song data is packaged into the URL at the top of your browser. When you make
changes to the song, the URL is updated to reflect your changes. When you are
satisfied with your song, just copy and paste the URL to save and share your
song!

BeepBox, and JummBox and EdoBox by extension, are passion projects and will always be free to use. If you find it
valuable and have the means, please support the original creator, [John Nesky](http://www.johnnesky.com/), via
[PayPal](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=QZJTX9GRYEV9N&currency_code=USD)!
EdoBox is developed by UnbihexiumFan.

## Compiling

The compilation procedure is identical to the repository for BeepBox and Jummbox. Here is the excerpt on compiling from the Beepbox page's readme below for convenience:

The source code is available under the MIT license. The code is written in
[TypeScript](https://www.typescriptlang.org/), which requires
[node & npm](https://www.npmjs.com/get-npm), so install those first. Then to
build this project, open the command line and run:

```
git clone https://github.com/unbihexiumfan/edobox-11edo.git
cd edobox-11edo
npm install
npm run build
```

JummBox and Edobox make a divergence from BeepBox that necessitates an additional dependency:
rather than using the (rather poor) default HTML select implementation, the custom
library [select2](https://select2.org) is employed. select2 has an explicit dependency
on [jQuery](https://jquery.com) as well, so you may need to install the following
additional dependencies if they are not picked up automatically.

```
npm install select2
npm install @types/select2
npm install @types/jquery
```

## Code

The code is divided into several folders. This architecture is identical to BeepBox's.

The [synth/](synth) folder has just the code you need to be able to play JummBox
songs out loud, and you could use this code in your own projects, like a web
game. After compiling the synth code, open website/synth_example.html to see a
demo using it. To rebuild just the synth code, run:

```
npm run build-synth
```

The [editor/](editor) folder has additional code to display the online song
editor interface. After compiling the editor code, open website/index.html to
see the editor interface. To rebuild just the editor code, run:

```
npm run build-editor
```

The [player/](player) folder has a miniature song player interface for embedding
on other sites. To rebuild just the player code, run:

```
npm run build-player
```

The [website/](website) folder contains index.html files to view the interfaces.
The build process outputs JavaScript files into this folder.

## Dependencies

Most of the dependencies are listed in [package.json](package.json), although
JummBox and Edobox also have an indirect, optional dependency on
[lamejs](https://www.npmjs.com/package/lamejs) via
[jsdelivr](https://www.jsdelivr.com/) for exporting .mp3 files. If the user
attempts to export an .mp3 file, JummBox and EdoBox will direct the browser to download
that dependency on demand.

## Modifying for other tunings

If you want to use a tuning system other than 11edo, everything you need can be found in synth/SynthConfig.ts. This example uses 14edo.

For changing the edo:

```
public static readonly edo: number = 11;
```

Can be replaced with any number:

```
public static readonly edo: number = 14;
```

It's important to change the ```keys``` array too, or else everything will break. For 14edo, it might look something like this:
```
public static readonly keys: DictionaryArray<Key> = toNameMap([
    { name: "C", isWhiteKey: true, basePitch: 0 + Config.edo},
    { name: "C+", isWhiteKey: false, basePitch: 1 + Config.edo},
    { name: "D", isWhiteKey: true, basePitch: 2 + Config.edo},
    { name: "D+", isWhiteKey: false, basePitch: 3 + Config.edo},
    { name: "E", isWhiteKey: true, basePitch: 4 + Config.edo},
    { name: "E+", isWhiteKey: false, basePitch: 5 + Config.edo},
    { name: "F", isWhiteKey: true, basePitch: 6 + Config.edo},
    { name: "F+", isWhiteKey: false, basePitch: 7 + Config.edo},
    { name: "G", isWhiteKey: true, basePitch: 8 + Config.edo},
    { name: "G+", isWhiteKey: false, basePitch: 9 + Config.edo},
    { name: "A", isWhiteKey: true, basePitch: 10 + Config.edo},
    { name: "A+", isWhiteKey: false, basePitch: 11 + Config.edo},
    { name: "B", isWhiteKey: true, basePitch: 12 + Config.edo},
    { name: "B+", isWhiteKey: false, basePitch: 13 + Config.edo},
]);
```

The 11edo scales won't work in 14edo, and if the list ```flags``` is shorter than ```Config.edo```, it causes a lot of problems. Namely, nothing will display. So you have to update the scales to match the tuning system.

```
public static readonly scales: DictionaryArray<Scale> = toNameMap([
    { name: "Free", realName: "14edo", flags: [true, true, true, true, true, true, true, true, true, true, true, true, true, true]},
    { name: "Even Heptatonic", realName: "7edo", flags: [true, false, true, false, true, false, true, false, true, false, true, false, true, false]},
    { name: "Bright Basic Pentatonic", realName: "pentic 4|0", flags: [true, false, true, false, false, false, true, false, false, false, true, false, true, false]},
    { name: "Major Basic Pentatonic", realName: "pentic 3|1", flags: [true, false, true, false, true, false, true, false, false, false, true, false, false, false]},
    { name: "Neutral Basic Pentatonic", realName: "pentic 2|2", flags: [true, false, false, false, true, false, true, false, true, false, true, false, false, false]},
    { name: "Minor Basic Pentatonic", realName: "pentic 1|3", flags: [true, false, false, false, true, false, false, false, true, false, true, false, true, false]},
    { name: "Dark Basic Pentatonic", realName: "pentic 0|4", flags: [true, false, true, false, true, false, false, false, true, false, false, false, true, false]},
    { name: "Bright Soft Pentatonic", realName: "manual 4|0", flags: [true, false, false, true, false, false, true, false, false, true, false, false, true, false]},
    { name: "Major Soft Pentatonic", realName: "manual 3|1", flags: [true, false, false, true, false, false, true, false, false, true, false, true, false, false]},
    { name: "Neutral Soft Pentatonic", realName: "manual 2|2", flags: [true, false, false, true, false, false, true, false, true, false, false, true, false, false]},
    { name: "Minor Soft Pentatonic", realName: "manual 1|3", flags: [true, false, false, true, false, true, false, false, true, false, false, true, false, false]},
    { name: "Dark Soft Pentatonic", realName: "manual 0|4", flags: [true, false, true, false, false, true, false, false, true, false, false, true, false, false]},
    { name: "Bright Hard Pentatonic", realName: "antipentic 4|0", flags: [true, false, false, false, true, false, false, false, true, true, false, false, false, true]},
    { name: "Major Hard Pentatonic", realName: "antipentic 3|1", flags: [true, false, false, false, true, true, false, false, false, true, false, false, false, true]},
    { name: "Neutral Hard Pentatonic", realName: "antipentic 2|2", flags: [true, false, false, false, true, true, false, false, false, true, true, false, false, false]},
    { name: "Minor Hard Pentatonic", realName: "antipentic 1|3", flags: [true, true, false, false, false, true, false, false, false, true, true, false, false, false]},
    { name: "Dark Hard Pentatonic", realName: "antipentic 0|4", flags: [true, true, false, false, false, true, true, false, false, false, true, false, false, false]},
]);
```

You can find a 14edo version of EdoBox [here](https://unbihexiumfan.github.io/edobox/14edo)
