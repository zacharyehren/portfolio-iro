---
layout: post
title: BlocJams
feature-img: "img/sample_feature_img.png"
thumbnail-path: "https://d13yacurqjgara.cloudfront.net/users/3217/screenshots/2030966/blocjams_1x.png"
short-description: My introduction to Angular 

---
Before starting this project, I had been learning the front-end languages in increments of complexity. Starting with vanilla javascript, I made my way through jQuery and finally my first framework - the infamous ANGULAR! This is my first project completed through this framework. 

{:.center}
![]({{ site.baseurl }}/img/bloc-jams-homepage.png)

Through a combination of following along with the code provided in the checkpoints and assignments that required me to come up with my own code, I built this program by refractoring the previous version of the same app that was built with vanilla JS & jQuery. 

The original version of the app that wasn't built through Angular had JS files that were clunky, unorganized and had 300+ lines of code. For an outside developer, this could be difficult to follow. Take the following code from the album view as an example: 

{% highlight javascript %}
var createSongRow = function(songNumber, songName, songLength) {
var template =
'<tr class="album-view-song-item">'
+ '  <td class="song-item-number" data-song-number="' + songNumber + '">' + songNumber + '</td>'
+ '  <td class="song-item-title">' + songName + '</td>'
// Assignment #3
+ '  <td class="song-item-duration">' + filterTimeCode(songLength) + '</td>'
+ '</tr>'
;
var $row = $(template);
var clickHandler = function() {
var songNumber = parseInt($(this).attr('data-song-number'));

if (currentlyPlayingSongNumber !== null) {
// Revert to song number for currently playing song because user started playing new song.
var currentlyPlayingCell = getSongNumberCell(currentlyPlayingSongNumber);
currentlyPlayingCell = getSongNumberCell(currentlyPlayingSongNumber); currentlyPlayingCell.html(currentlyPlayingSongNumber);
}
if (currentlyPlayingSongNumber !== songNumber) {
// Switch from Play -> Pause button to indicate new song is playing.
setSong(songNumber);
currentSoundFile.play();
updateSeekBarWhileSongPlays();
currentSongFromAlbum = currentAlbum.songs[songNumber - 1];

var $volumeFill = $('.volume .fill');
var $volumeThumb = $('.volume .thumb');
$volumeFill.width(currentVolume + '%');
$volumeThumb.css({left: currentVolume + '%'});

$(this).html(pauseButtonTemplate);
updatePlayerBarSong();
} else if (currentlyPlayingSongNumber === songNumber) {
if (currentSoundFile.isPaused()) {
$(this).html(pauseButtonTemplate);
$('.main-controls .play-pause').html(playerBarPauseButton);
currentSoundFile.play();
updateSeekBarWhileSongPlays();
} else {
$(this).html(playButtonTemplate);
$('.main-controls .play-pause').html(playerBarPlayButton);
currentSoundFile.pause();
}
}
};
var onHover = function(event) {
var songNumberCell = $(this).find('.song-item-number');
var songNumber = parseInt(songNumberCell.attr('data-song-number'));

if (songNumber !== currentlyPlayingSongNumber) {
songNumberCell.html(playButtonTemplate);
}
};

var offHover = function(event) {
var songNumberCell = $(this).find('.song-item-number');
var songNumber = parseInt(songNumberCell.attr('data-song-number'));

if (songNumber !== currentlyPlayingSongNumber) {
songNumberCell.html(songNumber);
}
};
// #1
$row.find('.song-item-number').click(clickHandler);
// #2
$row.hover(onHover, offHover);
// #3
return $row;
console.log("songNumber type is " + typeof songNumber + "\n and currentlyPlayingSongNumber type is " + typeof currentlyPlayingSongNumber);
};
{% endhighlight %}

What you see there is over 60 lines of code that creates a song row that includes information about each song and then dictates how the clicks should be handled throughout this page. It starts by building the row through finding the classes that pertain to each element that will comprise the song's row - such as the song's number, title and length. From there I test two conditions to determine whether the clicked song was already playing or not:
* `if (currentlyPlayingSongNumber !== null)`  
* `if (currentlyPlayingSongNumber !== songNumber)`

This will then dicate whether or not the song should start playing or be paused. From there, I developed the logic for the volume control, clicking the pause or play buttons, and how the screen should render when the mouse hovers over certain elements. These latter pieces of logic are all down through jQuery's `find`, `html`, & `click` methods.

This created the rows and click-handling functionality of the section of the album page below:

{:.center}
![]({{ site.baseurl }}/img/album-view.png)

Now, as verbose as that group of code was, jQuery did help cut down on the amount of lines required to apply all the logic. However, as Shakespear may or may not have said "Why write a program in 100 lines of code when you can do it in 50?". Taking our favorite poet's quote to heart, I then learned how to accomplish the same logic by using less code and breaking it up into more digestible chunks. 

Instead of creating a very large function that handles logic for the clicks, I was able to create several smaller functions that dictates what each click does in an Angular service. 

For example, in order to play the song, I first declare an empty variable which is what will constitute the audio file:

{% highlight javascript %}
var currentBuzzObject = null;
{% endhighlight %}

I then if SongPlayer.currentSong matches the chosen song. If it doesnt, it'll set the newly chosen song and play it. If the current song matches the chosen song, it is assumed that the song was paused and will then play it:

{% highlight javascript %}
SongPlayer.play = function(song) {
song = song || SongPlayer.currentSong;
if (SongPlayer.currentSong !== song) {
setSong(song);
playSong(song);
} else if (SongPlayer.currentSong === song) {
if (currentBuzzObject.isPaused()) {
playSong(song);
}
}
};
{% endhighlight %}

This public function is passed through the view's controller and applied on the page through Angular's `ng-click` functionality:

{% highlight html %}
<a class="album-song-button" ng-show="hovered && !song.playing" ng-click="album.songPlayer.play(song)"></a>
{% endhighlight %}

The result of building the app through Angular was a much leaner and more digestible group of programs that any developer would be able to pick up and work on without navigating through hundreds of lines of code. 

Since this was my first experience with a framework, my time was spent on learning the new ways to format the code along with the concepts behind everything. The latter of which was spending time gaining an understanding of how controllers fit into the picture and what their designated scopes do to the page they live on. This project was an excellent introduction to all of these concepts along with the benefits of using a framework over writing everything out manually. I will continue to gain a better understanding with upcoming projects. 

