![Preview of the overlay using Dohna Dohna no Uta as an example song.](https://user-images.githubusercontent.com/20557147/171977472-4c82e5d6-abc9-41ce-8f62-871fab8f3e53.png)

Now playing overlays for Foobar2000.
---------------------------------------------------------------------

Setup:

Install SkipyRich's **Now Playing Simple** plugin into Foobar2000.
The original site is gone unfortunately, but the WaybackMachine thankfully still has the page saved.
https://web.archive.org/web/20211122004949/https://skipyrich.com/wiki/Foobar2000:Now_Playing_Simple

The Download button is at the top of the Links section.
If you aren't familiar with Foobar2000 components, install it in **File > Preferences > Components** in Foobar2000.
You can drag and drop the file into the **Installed components** list or click the Install button and select it through there.

Then download and extract the files of this repo to it's final location.

Now configure it under **File > Preferences > Tools > Now Playing Simple**.

Set it to write to ```nowplaying.json``` within the top level of the Now Playing folder, where the html files are.
Then check all the events under the events tab, by default **On every second** is not active and needs to be changed.

Once done, copy the following into the **Formatting string**:
```
{
	"nowplaying": {
		"playing": $replace(%isplaying%,'?','0'),
		"paused": $replace(%ispaused%,'?','0'),
		"albumartist": "$replace(%album artist%,'"','\"','\','\\')",
		"album": "$replace(%album%,'"','\"','\','\\')",
		"artist": "$replace(%artist%,'"','\"','\','\\')",
		"title": "$replace(%title%,'"','\"','\','\\')",
		"tracknumber": $add($replace(%track number%,'?','0'),0),
		"length": $replace(%length_seconds%,'?','0'),
		"elapsed": $replace(%playback_time_seconds%,'?','0'),
		"path": "$replace($directory_path(%path%),'"','\"','\','\\')"
	},
	"config": {
		"fadeout": 0
	}
}
```

It should look like in this photo:

![Example photo of the settings.](/NPS_Config_Example.png)

Configuration
---------------------------------------------------------------------
By default the **fadeout** is set to 0 seconds which will perpetually show the overlay without fading after the initial track change.

You can change the duration to a positive number of seconds that the overlay will show for on track change by changing fadeout under config in the JSON file. <sub>i.e. the **Formatting string**</sub>

---
To make Album Art Work:

So pretty much, Foobar2000 searches the folder the music file is in for a **folder.jpg**. 
When that happens the component captures that instance of folder.jpg to show on the overlay.

It needs to be specifically be a jpg named folder, anything else like folder.png will not work.

As far as this overlay is concerned that's all that matters.
So separate your albums into different folders with the art you want them to use.

Specifics:
Foobar2000 itself usually then uses folder.jpg with priority over any album art the music file might be embedded with.
Depending on the specifics Foobar2000 might elect to use the embedded art over folder.jpg, but even if that happens it doesn't effect the component.
Likewise, it doesn't matter if the music has embedded art that works just fine in Foobar2000 or whatever else, if there's no folder.jpg, nothing will show.
The component is not capturing the album art Foobar2000 extracts from the music file itself, only copying the instance of folder.jpg it finds while it's choosing what to show.

For example:

Say I want to play a song from the Needy Girl Overdose OST on stream.

<details>
  <summary>(Example Image Set without folder.jpg - **Click To Expand**)</summary>
  
![Folder with no folder.jpg](https://i.imgur.com/F5V8LRH.png)
![Embedded album art shown in Foobar2000](https://i.imgur.com/kgMhwil.png)
</details>

It has this album art embedded into the metadata which shows up fine in Foobar2000, but without the folder.jpg, it won't show in the overlay.

It still requires a folder.jpg like this, which it used over the embedded art in this case in Foobar2000 itself as well.

\NEEDY GIRL OVERDOSE OST\03 - INTERNET OVERDOSE.flac
\NEEDY GIRL OVERDOSE OST\folder.jpg

The folder's name doesn't actually matter, but ideally for the sake of organization outside it should be something reasonable like the album name.

<details>
  <summary>(Example Image Set without folder.jpg - **Click To Expand**)</summary>
  
![Folder with folder.jpg containing different album art](https://i.imgur.com/5qWWAQe.png)
![folder.jpg being used in Foobar2000, taking priority over the embedded almum art](https://i.imgur.com/gSjZ37Z.png)
</details>

Also don't leave a stray foo_np_simple.dll in the Now Playing folder in the off chance you have a really messy setup for whatever reason.
It'll prioritize using the foo_np_simple.dll connected to nothing in that folder over using the one in Foobar2000.

To add in OBS:
--------------

Add a Browser source, local file, and then the html file for the overlay you want.

For Album Art Overlay:
Set it to Width 1280 & Height 480
Then considering cropping 30 off all sides while transforming it to the size you want if you want it to fit nicely with other sources, or 40 if you need it to fit edge to edge.

No Album Art Overlay:
Width 220 & Height 120 
